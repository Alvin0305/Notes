```C++
class IndBuffer : public BlockBuffer {
	public:
		IndBuffer(char blockType);
		IndBuffer(int blockNum);
		
		virtual int getEntry(void *ptr, int indexNum) = 0;
		virtual int setEntry(void *ptr, int indexNum) = 0;
}
```
- methods getEntry and setEntry are virtual and are implemented in IndInternal and IndLeaf classes
### Contructors
##### IndBuffer(char blockType)
```C++
IndBuffer(char blockType) : BlockBuffer(blockType) {}
```
##### IndBuffer(int blockNum)
```C++
IndBuffer(int blockNum) : BlockBuffer(blockNum) {}
```