Creating a instance of a block using `BlockBuffer` for each block will required a `readBlock` (access to secondary memory) making the operation inefficient.
So, we allow the storage of `32` blocks in the `StaticBuffer`. Such that, when a new block needs to be instantiated, we first check whether it is in the `StaticBuffer`. If yes, it will take from it, otherwise go to the secondary memory

```C++
class StaticBuffer {
	friend class BlockBuffer;
	
	public:
		StaticBuffer();
		~StaticBuffer();
		
		static int getStaticBlockType(int blockNum);
		static int setDirtyBit(int blockNum);
		
	private:
		static unsigned char blocks[BUFFER_CAPACITY][BLOCK_SIZE];
		static struct BufferMetaInfo metainfo[BUFFER_CAPACITY];
		static unsigned char blockAllocMap[DISK_BLOCKS];
		
		static int getBufferNum(int blockNum);
		static int getFreeBuffer(int blockNum);
}
```

- BlockBuffer can access the private and protected methods of StaticBuffer

### Constructor and Destructor
##### StaticBuffer()
- Constructor for StaticBuffer
- Copies the BMAP from disk to `blockAllocMap`
- Initialize the metainfo

```C++
StaticBuffer::StaticBuffer() {
	for (int blockNum = 0; blockNum < 4; blockNum++) {
		unsigned char buffer[BLOCK_SIZE];
		Disk::readBlock(buffer, blockNum);
		memcpy(blockAllocMap + blockNum * BLOCK_SIZE, buffer, BLOCK_SIZE);
	}
	
	for (int bufferIndex = 0; bufferIndex < BUFFER_CAPACITY; bufferIndex++) {
		metainfo[bufferIndex].free = true;
		metainfo[bufferIndex].dirty = false;
		metainfo[bufferIndex].blockNum = -1;
		metainfo[bufferIndex].timeStamp = -1;
	}
}
```

##### ~StaticBuffer()
- Destructor for StaticBuffer
- Copies back the BMAP from `blockAllocMap` to disk
- Copies back the dirty blocks

```C++
StaticBuffer::~StaticBuffer() {
	for (int blockNum = 0; blockNum < 4; blockNum++) {
		unsigned char buffer[BLOCK_SIZE];
		memcpy(buffer, blockAllocMap + blockNum * BLOCK_SIZE, BLOCK_SIZE);
		Disk::writeBlock(buffer, blockNum);
	}
	
	for (int bufferIndex = 0; bufferIndex < BUFFER_CAPACITY; bufferIndex++) {
		if (!metainfor[bufferIndex].free and metainfo[bufferIndex].dirty) {
			Disk::writeBlock(blocks[bufferIndex], metainfo[bufferIndex].blockNum);
		}
	}
}
```

### Attributes
```C++
unsigned char blocks[BUFFER_CAPACITY][BLOCK_SIZE];
struct BufferMetaInfo metainfo[BUFFER_CAPACITY];
unsigned char blockAllocMap[DISK_BLOCKS];
```

### Methods
##### getStaticBlockType(int blockNum)
- checks whether the `blockNum` is valid
- return the blockType from the `blockAllocMap`
```C++
int getStaticBlockType(int blockNum) {
	if (blockNum < 0 or blockNum >= DISK_BLOCKS) {
		return E_OUTOFBOUND;
	}
	
	return (int) blockAllocMap[blockNum];
}
```

>[!info] 
>Returns
>- SUCCESS
>- E_OUTOFBOUND
##### setDirtyBit(int blockNum)
- Check if the block is in the `blocks`
- Find the `bufferNum` (the index of the block in `blocks`)
- checks whether the `blockNum` is valid
- update the `metainfo`
```C++
int setDirtyBit(int blockNum) {
	int bufferNum = getBufferNum(blockNum);
	
	if (bufferNum == E_BLOCKNOTINBUFFER) {
		return E_BLOCKNOTINBUFFER;
	}
	
	if (blockNum < 0 or blockNum >= DISK_BLOCKS) {
		return E_OUTOFBOUND;
	}
	
	metainfo[bufferNum].dirty = true;
	return SUCCESS;
}
```

>[!info] 
>Returns
>- SUCCESS
>- E_BLOCKNOTINBUFFER
>- E_OUTOFBOUND

##### getBufferNum(int blockNum)
- Checks if the `blockNum` is valid
- Iterate through the `blocks` and find the `bufferNum` corresponding to the given `blockNum` using `metainfo`
```C++
int getBufferNum(int blockNum) {
	if (blockNum < 0 or blockNum >= DISK_BLOCKS) {
		return E_OUTOFBOUND;
	}
	
	for (int bufferNum = 0; bufferNum < BUFFER_CAPACITY; bufferNum++) {
		if (metainfo[bufferNum].blockNum == blockNum) {
			return bufferNum;
		}
	}
	
	return E_BLOCKNOTINBUFFER;
}
```

##### getFreeBuffer(int blockNum)
- Checks whether `blockNum` is valid
- Increment the timestamp for all blocks
- Find a block which is free
- Find the block with highest timestamp
- If a free block is not got
	- If the block with highest TS is dirty, write back the block
	- use the block with highest TS
- update the metainfo for the allocated block
```C++
int getFreeBuffer(int blockNum) {
	if (blockNum < 0 or blockNum >= DISK_BLOCKS) {
		return E_OUTOFBOUND;
	}
	
	for (int bufferNum = 0; bufferNum < BUFFER_CAPACITY; bufferNum++) {
		if (!metainfo[bufferNum].free) {
			metainfo[bufferNum].timeStamp++;
		}
	}
	
	int allocatedBuffer = -1;
	int bufferNumWithHighestTS = 0;
	
	for (int bufferNum = 0; bufferNum < BUFFER_CAPACITY; bufferNum++) {
		if (metainfo[bufferNum].free) {
			allocatedBuffer = bufferNum;
			break;
		}
		
		if (metainfo[bufferNum].timeStamp > metainfo[bufferNumWithHighestTS].timeStamp) {
			bufferNumWithHighestTS = bufferNum;
		}
	}
	
	if (allocatedBuffer == -1) {
		if (metainfo[bufferNumWithHighestTS].dirty) {
			Disk::writeBlock(blocks[bufferNumWithHighestTS], metainfo[bufferNumWithHighestTS].blockNum);
		}
		
		allocatedBuffer = bufferNumWithHighestTS;
	}
	
	metainfo[allocatedBuffer].free = false;
	metainfo[allocatedBuffer].dirty = false;
	metainfo[allocatedBuffer].blockNum = blockNum;
	metainfo[allocatedBuffer].timeStamp = -1;
	
	return allocatedBuffer;
}
```

