#### Disk
```C++
class Disk {
	public:
		Disk();
		~Disk();
		
		static readBlock(unsigned char *block, int blockNum);
		static writeBlock(unsigned char *block, int blockNum);
}
```
#### StaticBuffer
```C++
struct BufferMetaInfo {
	bool free;
	bool dirty;
	int blockNum;
	int timeStamp;
}

class StaticBuffer {
	friend class BlockBuffer;
	
	private:
		static unsigned char blocks[DISK_BLOCKS][BLOCK_SIZE];
		static unsigned char blockAllocMap[BUFFER_CAPACITY];
		static struct BufferMetaInfo metainfo[BUFFER_CAPACITY];
		
		static int getFreeBlock(int blockNum);
		static int getBufferNum(int blockNum);
		
	public:
		StaticBuffer();
		~StaticBuffer();
		
		static int setDirtyBit(int blockNum);
		static int getStaticBlockType(int blockNum);
}
```
#### BlockBuffer
```C++
struct HeadInfo {
	int32_t blockType;
	int32_t pblock;
	int32_t lblock;
	int32_t rblock;
	int32_t numRecs;
	int32_t numAttrs;
	int32_t numSlots;
	unsigned char reserved[4];
};

typedef union Attribute {
	char sVal[ATTR_SIZE];
	double nVal;
} Attribute;

struct InternalEntry {
	int32_t lChild;
	Attribute attrVal;
	int32_t rChild;
};

struct Index {
	Attribute attrVal;
	int32_t block;
	int32_t slot;
	unsigned char unused[8];
};

class BlockBuffer {
	protected:
		int blockNum;
		
		int loadBlockAndGetBufferPtr(unsigned char **bufferPtr);
		int getFreeBlock(int blockType);
		int setBlockType(int blockType);
	
	public:
		BlockBuffer(char blockType);
		BlockBuffer(int blockNum);
		
		int getBlockNum();
		int getHeader(struct HeadInfo *head);
		int setHeader(struct HeadInfo *head);
		void releaseBlock();
}

class RecBuffer : public BlockBuffer {
	public:
		RecBuffer();
		RecBuffer(int blockNum);
		
		int getSlotMap(unsigned char *slotMap);
		int setSlotMap(unsigned char *slotMap);
		int getRecord(union Attribute *rec, int slotNum);
		int setRecord(union Attribute *rec, int slotNum);
}

class IndBuffer : public BlockBuffer {
	public:
		IndBuffer(int blockNum);
		IndBuffer(char blockType);
		
		virtual int getEntry(void *ptr, int indexNum) = 0;
		virtual int setEntry(void *ptr, int indexNum) = 0;
}

class IndInternal : public IndBuffer {
	public:
		IndInternal();
		IndInternal(int blockNum);
		
		int getEntry(void *ptr, int indexNum);
		int setEntry(void *ptr, int indexNum);
}

class IndLeaf : public IndBuffer {
	public: 
		Indleaf();
		IndLead(int blockNum);
		
		int getEntry(void *ptr, int indexNum);
		int setEntry(void *ptr, int indexNum);
}
```
#### OpenRelTable
```C++
typedef struct OpenRelTableMetaInfo {
	bool free;
	char relName[ATTR_SIZE];
} OpenRelTableMetaInfo;

class OpenRelTable {
	private:
		static OpenRelTableMetaInfo tableMetaInfo[MAX_OPEN];
		
		static int getFreeOpenRelTablEntry();
		
	public:
		OpenRelTable();
		~OpenRelTable();
		
		static int getRelId(char relName[ATTR_SIZE]);
		static int openRel(char relName[ATTR_SIZE]);
		static int closeRel(int relId);
}
```
#### RelCacheTable
```C++
typedef struct RelCatEntry {
	char relName[ATTR_SIZE];
	int numAttrs;
	int numRecs;
	int firstBlk;
	int lastBlk;
	int numSlotsPerBlk;
} RelCatEntry;

typedef struct RelCacheEntry {
	RelCatEntry relCatEntry;
	bool dirty;
	RecId recId;
	RecId searchIndex;
} RelCacheEntry;

class RelCacheTable {
	friend class OpenRelTable;
	
	private:
		static RelCacheEntry *relCache[MAX_OPEN];
		
		static void recordToRelCatEntry(union Attribute record[ATTR_SIZE], RelCatEntry *relCatEntry);
		static void relCatEntryToRecord(RelCatEntry *relCatEntry, union Attribute record[ATTR_SIZE]);
		
	public:
		static int getRelCatEntry(int relId, RelCatEntry *relCatBuf);
		static int setRelCatEntry(int relId, RelCatEntry *relCatBuf);
		static int getSearchIndex(int relId, RecId *searchIndex);
		static int setSearchIndex(int relId, RecId *searchIndex);
		static int resetSearchIndex(int relId);
}
```
#### AttrCacheTable
```C++
typedef struct AttrCatEntry {
	char relName[ATTR_SIZE];
	char attrName[ATTR_SIZE];
	int attrType;
	bool primaryFlag;
	int rootBlock;
	int offset;
} AttrCatEntry;

typedef struct AttrCacheEntry {
	AttrCatEntry attrCatEntry;
	bool dirty;
	RecId recId;
	IndexId searchIndex;
	AttrCacheEntry *next;
} AttrCacheEntry;

class AttrCacheTable {
	friend class OpenRelTable;
	
	private:
		static AttrCacheEntry attrCache[MAX_OPEN];
		
		static void recordToAttrCatEntry(union Attribute record[ATTR_SIZE], AttrCatEntry *attrCatEntry);
		static void attrCatEntryToRecord(AttrCatEntry *attrCatEntry, union Attribute record[ATTR_SIZE]);
		
	public:
		static int getAttrCatEntry(int relId, char attrName[ATTR_SIZE], AttrCatEntry *attrCatBuf);
		static int getAttrCatEntry(int relId, int offset, AttrCatEntry *attrCatBuf);
		static int setAttrCatEntry(int relId, char attrName[ATTR_SIZE], AttrCatEntry *attrCatBuf);
		static int setAttrCatEntry(int relId, int offset, AttrCatEntry *attrCatBuf);
		static int getSearchIndex(int relId, char attrName[ATTR_SIZE], IndexId *searchIndex);
		static int getSearchIndex(int relId, int offset, IndexId *searchIndex);
		static int setSearchIndex(int relId, char attrName[ATTR_SIZE], IndexId *searchIndex);
		static int setSearchIndex(int relId, int offset, IndexId *searchIndex);
		static int resetSearchIndex(int relId, char attrName[ATTR_SIZE]);
		static int resetSearchIndex(int relId, int offset);
	
}
```
#### BlockAccess
```C++
class BlockAccess {
	public:
		static int search(int relId, Attribute *record, char *attrName, Attribute attrVal, int op);
		static int insert(int relId, union Attribute *record);
		static int renameRelation(char *oldName, char *newName);
		static int renameAttribute(char *relName, char *oldName, char *newName);
		static int deleteRelation(char *relName);
		static RecId linearSearch(int relId, char *attrName, Attribute attrVal, int op);
		static int project(int relId, Attribute *record);
}
```
#### Algebra
```C++
class Algebra {
	public:
		static int insert(char relName[ATTR_SIZE], int numAttrs, char record[][ATTR_SIZE]);
		static int select(char relName[ATTR_SIZE], char targetRel[ATTR_SIZE], char attr[ATTR_SIZE], int op, char strVal[ATTR_SIZE]);
		static int project(char relName[ATTR_SIZE], char targetRel[ATTR_SIZE]);
		static int project(char relName[ATTR_SIZE], char targetRel[ATTR_SIZE], int tar_nAttrs, char tar_Attrs[][ATTR_SIZE]);
		static int join(char srcRelOne[ATTR_SIZE], char srcRelTwo[ATTR_SIZE], char targetRel[ATTR_SIZE], char attrOne[ATTR_SIZE], char attrTwo[ATTR_SIZE]);
}
```
#### Schema
```C++
class Schema {
	public:
		static int createRel(char relName[], int numAttrs, char attrNames[][ATTR_SIZE], int attrType[]);
		static int deleteRel(char relName[]);
		static int createIndex(char relName[], char attrName[]);
		static int dropIndex(char relName[], char attrName[]);
		static int renameRel(char olRelName[], char newRelName[]);
		static int renameAttr(char relName[], char oldAttrName[], char newAttrName[]);
		static int openRel(char relName[]);
		static int closeRel(char relName[]);
}
```
#### BPlusTree
```C++
class BPlusTree {
	private:
		static int findLeafToInsert(int rootBlock, Attribute attrVal, int attrType);
		static int insertIntoLeaf(int relId, char attrName[], int blockNum, Index entry);
		static int splitLeaf(int leafBlockNum, Index indices[]);
		static int insertIntoInternal(int relId, char attrName[], int intBlockNum, InternalEntry entry);
		static int createNewRoot(int relId, char attrName[], Attribute attrVal, int lChild, int rChild);
	
	public:
		static int bPlusCreate(int relId, char attrName[]);
		static int bPlusInsert(int relId, char attrName[], union Attribute attrVal, RecId recordId);
		static RecId bPlusSearch(int relId, char attrName[], union Attribute attrVal, int op);
		static int bPlusDestroy(int rootBlockNum);
}
```
#### Frontend
```C++
public class Frontend {
	
}
```