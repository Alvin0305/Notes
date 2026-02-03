This is the generic class which represents a disk block. Operations in a block is done using this class

```C++
class BlockBuffer {
	public:
		BlockBuffer(char blockType);
		BlockBuffer(int blockNum);
		int getBlockNum();
		int getHeader(struct HeadInfo *head);
		int setHeader(struct HeadInfo *head);
		void releaseBlock();
	
	protected:
		int blockNum;
		
		unsigned char *getBufferPtr();
		int getBlock();
		int getFreeBlock(int blockType);
		int setBlockType(int blockType);
}
```

### Constructors and Destructors
##### BlockBuffer(char blockType)
- Identify the type of the block required
- get a free block using `getFreeBlock(blockType)`
- update `blockNum`
```C++
BlockBuffer(char blockTypeChar) {
	unsigned char *bufferPtr;
	int blockType = blockTypeChar == 'R'   ? REC
					: blockTypeChar == 'I' ? IND_INTERNAL
					: blockTypeChar == 'L' ? IND_LEAF
										   : UNUSED_BLK;
										   
	if (blockType == UNUSED_BLK) {
		printf("Invalid Block Type\n");
		return;
	}
	
	int freeBlockNum = getFreeBlock(blockType);
	
	if (freeBlockNum < 0 or freeBlockNum >= DISK_BLOCKS) {
		printf("Failed to get a free block");
		exit(FAILURE);
	}
	
	this->blockNum = freeBlockNum;
}
```
##### BlockBuffer(int blockNum)
- set the `blockNum` of the BlockBuffer
```C++
BlockBuffer(int blockNum) {
	this->blockNum = blockNum;
}
```

### Methods
##### getBlockNum()
- return the `blockNum` of the BlockBuffer
```C++
int getBlockNum() {
	return this->blockNum;
}
```
##### getHeader(struct HeadInfo \*head)
- load the block using `loadBlockAndGetBufferPtr`
- copy each of the attributes from the `bufferPtr` to the `head`
```C++
int getHeader(struct HeadInfo *head) {
	unsigned char *bufferPtr;
	int ret = loadBlockAndGetBufferPtr(&bufferPtr);
	
	if (ret != SUCCESS) return ret;
	
	memcpy(&head->blockType, bufferPtr + 0, 4);
	memcpy(&head->pblock, bufferPtr + 4, 4);
	memcpy(&head->lblock, bufferPtr + 8, 4);
	memcpy(&head->rblock, bufferPtr + 12, 4);
	memcpy(&head->numEntries, bufferPtr + 16, 4);
	memcpy(&head->numAttrs, bufferPtr + 20, 4);
	memcpy(&head->numSlots, bufferPtr + 24, 4);
	
	return SUCCESS;
}
```
##### setHeader(struct HeadInfo \*head)
- get `bufferPtr` of the block using `loadBlockAndGetBufferPtr`
- cast `bufferPtr` to `HeadInfo *`
- copy each of the properties of `head` to `bufferHeader`
- mark the block as dirty
```C++
int setHeader(struct HeadInfo *head) {
	unsigned char *bufferPtr;
	int ret = loadBlockAndGetBufferPtr(&bufferPtr);
	
	if (ret != SUCCESS) return ret;
	
	struct HeadInfo *bufferHeader = (struct HeadInfo*) bufferPtr;
	
	bufferHeader->blockType = head->blockType;
	bufferHeader->pblock = head->pblock;
	bufferHeader->lblock = head->lblock;
	bufferHeader->rblock = head->rblock;
	bufferHeader->numEntries = head->numEntries;
	bufferHeader->numAttrs = head->numAttrs;
	bufferHeader->numSlots = head->numSlots;
	
	int ret_ = StaticBuffer::setDirty(this->blockNum);
	if (ret_ != SUCCESS) return ret_;
	
	return SUCCESS;
}
```
##### releaseBlock()
- when `releaseBlock` is called once, the `blockNum` is set as `INVALID_BLK`
- in this method, we mark the block as free in `metainfo` and update the `blockAllocMap` entry as `UNUSED_BLK` in StaticBuffer
```C++
void releaseBlock() {
	if (blockNum == INVALID_BLOCKNUM or StaticBuffer::blockAllocMap[blockNum] == UNUSED_BLK) return;
	
	int bufferNum = StaticBuffer::getBufferNum(blockNum);
	if (bufferNum >= 0 and bufferNum < BUFFER_CAPACITY) {
		StaticBuffer::metainfo[bufferNum].free = true;
	}
	
	StaticBuffer::blockAllocMap[blockNum] = UNUSED_BLK;
	this->blockNum = INVALID_BLOCKNUM;
}
```
##### loadBlockAndGetBufferPtr(unsigned char \*\*bufferPtr)
```C++
int loadBlockAndGetBufferPtr(unsigned char **bufferPtr) {
	int bufferNum = StaticBuffer::getBufferNum(blockNum);
	
	if (bufferNum == E_BLOCKNOTINBUFFER) {
		bufferNum = StaticBuffer::getFreeBuffer(blockNum);
		
		Disk::readBlock(StaticBuffer::blocks[bufferNum], blockNum);
	} else {
		for (int bufferIndex = 0; bufferIndex < BUFFER_CAPACITY; bufferIndex++) {
			if (bufferIndex == bufferNum) {
				StaticBuffer::metainfo[bufferIndex].timeStamp = 0;
			} else {
				StaticBuffer::metainfo[bufferIndex].timeStamp++;
			}
		}
	}
	
	*bufferPtr = StaticBuffer::blocks[bufferNum];
	
	return SUCCESS;
}
```
##### getFreeBlock(int blockType)
```C++
int getBlockType(int blockType) {
	int blockNum;
	
	for (blockNum = 0; blockNum < DISK_BLOCKS; blockNum++) {
		if (StaticBuffer::blockAllocMap[blockNum] == UNUSED_BLK) {
			break;
		}
	}
	
	if (blockNum == DISK_BLOCKS) {
		return E_DISKFULL;
	}
	
	this->blockNum = blockNum;
	
	int bufferIndex = StaticBuffer::getFreeBuffer(blockNum);
	if (bufferIndex < 0 or bufferIndex >= BUFFER_CAPACITY) {
		printf("ERROR: Buffer full\n");
		return bufferIndex;
	}
	
	HeadInfo header;
	header.lblock = -1;
	header.rblock = -1;
	header.pblock = -1;
	header.numAttrs = 0;
	header.numEntries = 0;
	header.numSlots = 0;
	
	setHeader(&header);
	setBlockType(blockType);
	
	return blockNum;
}
```
##### setBlockType(int blockType)
```C++
int setBlockType(int blockType) {
	unsigned char *bufferPtr;
	int ret = loadBlockAndGetBufferPtr(&bufferPtr);
	if (ret != SUCCESS) {
		return ret;
	}
	
	(*(int32_t *) bufferPtr) = blockType;
	
	StaticBuffer::blockAllocMap[blockNum] = blockType;
	
	int ret_ = StaticBuffer::setDirtyBit(blockNum);
	
	return ret_;
}
```
