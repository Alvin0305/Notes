#### Header
```C++
struct HeadInfo {
	int32_t blockType;
	int32_t pblock;
	int32_t lblock;
	int32_t rblock;
	int32_t numRecords;
	int32_t numAttrs;
	int32_t numSlots;
	unsigned char reserved[4];
}
```
#### RelCatEntry
```C++
struct RelCatEntry {
	char relName[ATTR_SIZE];
	int numAttrs;
	int numRecs;
	int firstBlk;
	int lastBlk;
	int numSlots;
}
```
#### AttrCatEntry
```C++
struct AttrCatEntry {
	char relName[ATTR_SIZE];
	char attrName[ATTR_SIZE];
	int attrType;
	bool primaryFlag;
	int rootBlk;
	int offset;
}
```
#### RelCacheEntry
```C++
struct RelCacheEntry {
	RelCatEntry relCatEntry;
	bool dirty;
	RecId recId;
	RecId searchIndex;
}
```
#### AttrCacheEntry
```C++
struct AttrCacheEntry {
	AttrCatEntry attrCatEntry;
	bool dirty;
	RecId recId;
	IndexId searchIndex;
	struct AttrCacheEntry *next;
}
```
####  BufferMetaInfo
```C++
struct BufferMetaInfo {
	bool free;
	bool dirty;
	int blockNum;
	int timeStamp;
}
```
#### Attribute
```C++
union Attribute {
	char sVal[ATTR_SIZE];
	float nVal;
}
```
#### InternalEntry
```C++
struct InternalEntry {
	int32_t lChild;
	Attribute attrVal;
	int32_t rChild;
}
```
#### Index
```C++
struct Index {
	Attribute attrVal;
	int32_t block;
	int32_t slot;
	unsigned char[8];
} 
```
#### RecId
```C++
struct RecId {
	int block;
	int slot;
}
```
#### IndexId
```C++
struct IndexId {
	int block;
	int index;
}
```