```C++
class IndInternal : public IndBuffer {
	public:
		IndInternal();
		IndInternal(int blockNum);
		
		int getEntry(void *ptr, int indexNum);
		int setEntry(void *ptr, int indexNum);
}
```

### Contructors
##### IndInternal()
```C++
IndInternal() : IndBuffer('I') {}
```
##### IndInternal(int blockNum)
```C++
IndInternal(int blockNum) : IndBuffer(blockNum) {}
```

### Methods
##### getEntry(void \*ptr, int indexNum)
```C++
int getEntry(void *ptr, int indexNum) {
	if (indexNum < 0 or indexNum >= MAX_KEYS_INTERNAL) {
		return E_OUTOFBOUND;
	}
	
	unsigned char *bufferPtr;
	int ret = getBlockAndLoadBufferPtr(&bufferPtr);
	if (ret != SUCCESS) {
		return ret;
	}
	
	struct InternalEntry *internalEntry = (struct InternalEntry*) ptr;
	unsigned char *entryPtr = bufferPtr + HEADER_SIZE + (indexNum * 20);
	
	memcpy(&(internalEntry->lChild), entryPtr, sizeof(int32_t));
	memcpy(&(internalEntry->attrVal), entryPtr + sizeof(int32_t), ATTR_SIZE);
	memcpy(&(internalEntry->rChild), entryPtr + sizeof(int32_t) + ATTR_SIZE, sizeof(int32_t));
	
	return SUCCESS;
}
```
