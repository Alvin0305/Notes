```C++
class RecBuffer : public BlockBuffer {
	public:
		RecBuffer();
		RecBuffer(int blockNum);
		
		int getSlotMap(unsigned char *slotMap);
		int setSlotMap(unsigned char *slotMap);
		int getRecord(union Attribute *record, int slotNum);
		int setRecord(union Attribute *record, int slotNum);
}
```

### Constructors
##### RecBuffer()
```C++
RecBuffer() : BlockBuffer('R') {}
```
##### RecBuffer(int blockNum)
```C++
RecBuffer(int blockNum) : BlockBuffer(blockNum) {}
```

### Methods
##### getSlotMap(unsigned char \*slotMap)
```C++
int getSlotMap(unsigned char *slotMap) {
	unsigned char *bufferPtr;
	int ret = loadBlockAndGetBufferPtr(&bufferPtr);
	if (ret != SUCCESS) {
		return ret;
	}
	
	struct HeadInfo head;
	getHeader(&head);
	
	int numOfSlots = head.numSlots;
	memcpy(slotMap, bufferPtr + HEADER_SIZE, numOfSlots);
	
	return SUCCESS;
}
```
##### setSlotMap(unsigned char \*slotMap)
```C++
int setSlotMap(unsigned char *slotMap) {
	unsigned char *bufferPtr;
	int ret = loadBlockAndGetBufferPtr(&bufferPtr);
	if (ret != SUCCESS) {
		return ret;
	}
	
	struct HeadInfo head;
	getHeader(&head);
	
	int numSlots = head.numSlots;
	memcpy(bufferPtr + HEADER_SIZE, slotMap, numSlots);
	
	ret = StaticBuffer::setDirtyBit(blockNum);
	return ret;
}
```
##### getRecord(union Attribute \*record, int slotNum)
```C++
int getRecord(union Attribute *record, int slotNum) {
	struct HeadInfo head;
	getHeader(&head);
	
	int attrCount = head.numAttrs;
	int slotCount = head.numSlots;
	
	unsigned char *bufferPtr;
	int ret = loadBlockAndGetBufferPtr(&bufferPtr);
	if (ret != SUCCESS) {
		return ret;
	}
	
	int recordSize = attrCount * ATTR_SIZE;
	unsigned char *slotPointer = bufferPtr + HEADER_SIZE + slotCount + recordSize *slotNum;
	
	memcpy(record, slotPointer, recordSize);
	return SUCCESS;
}
```
##### setRecord(union Attribute \*record, int slotNum)
```C++
int setRecord(union Attribute *record, int slotNum) {
	struct HeadInfo head;
	getHeader(&head);
	
	int attrCount = head.numAttrs;
	int slotCount = head.numSlots;
	
	unsigned char *bufferPtr;
	int ret = loadBlockAndGetBufferPtr(&bufferPtr);
	if (ret != SUCCESS) {
		return ret;
	}
	
	int recordSize = attrCount * ATTR_SIZE;
	unsigned char *slotPointer = bufferPtr + HEADER_SIZE + slotCount + recordSize * slotNum;
	
	memcpy(slotPointer, record, recordSize);
	
	return StaticBuffer::setDirtyBit(blockNum);
}
```