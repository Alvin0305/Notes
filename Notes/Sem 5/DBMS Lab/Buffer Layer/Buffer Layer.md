The physical layer is working with files (secondary memory). But to work with the blocks, we need to bring a block from secondary memory to the primary memory. This is done by Buffer layer

The classes used in this layer are:
- [[Static Buffer]]
- [[Block Buffer]]
	- [[Rec Buffer]]
	- [[Ind Buffer]]
		- [[Ind Internal]]
		- [[Ind Leaf]]

Static Buffer is a friend class of Block Buffer
Ind Buffer and Rec Buffer inherits Block Buffer
Ind Internal and Rec Buffer inherits Ind Buffer

The structures used in this layer are:
- [[#BufferMetaInfo]]
- [[#HeadInfo]]
- [[#Attribute]]
- [[#InternalEntry]]
- [[#Index]]
Miscellaneous structures used in this layer are:
- [[#RecId]]
- [[#IndexId]]

### Methods
##### compareAttrs(Attribute attr1, Attribute attr2, int attrType)
- Used to compare two attributes based on its type
```C++
int compareAttrs(union Attribute attr1, union Attribute attr2, int attrType) {
	double diff;
	if (attrType == STRING) {
		diff = strcmp(attr1.sVal, attr2.sVal);
	} else {
		diff = attr1.nVal - attr2.nVal;
	}
	
	if (diff > 0) return 1;
	if (diff < 0) return -1;
	return 0;
}
```

### Structures
##### BufferMetaInfo
```C++
struct BufferMetaInfo {
	bool free;
	bool dirty;
	int blockNum;
	int timeStamp;
}
```
##### HeadInfo
```C++
struct HeadInfo {
	int32_t blockType;
	int32_t pBlock;
	int32_t lBlock;
	int32_t rBlock;
	int32_t numEntries;
	int32_t numAttrs;
	int32_t numSlots;
	unsigned char reserved[4];
}
```
##### Attribute
```C++
union Attribute {
	double nVal;
	char sVal[ATTR_SIZE];
}
```
##### InternalEntry
```C++
struct InternalEntry {
	int32_t lChild;
	union Attribute attrVal;
	int32_t rChild;
}
```
##### Index
```C++
struct Index {
	union Attribute attrVal;
	int32_t block;
	int32_t slot;
	unsigned char reserved[8];
}
```

- Miscellaneous
##### RecId
```C++
struct RecId {
	int block;
	int slot;
}
```
##### IndexId
```C++
struct IndexId {
	int block;
	int index;
}
```

