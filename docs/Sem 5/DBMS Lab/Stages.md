#### Stage  0
- Installation and Preparation
#### Stage 1
- Understanding the Disk Interface
- Creates
	- `Disk::readBlock`
	- `Disk::writeBlock`
#### Stage 2
- Record Blocks and Catalogs
- Creates
	- `BlockBuffer::BlockBuffer(int blockNum)`
	- `RecBuffer::RecBuffer(int blockNum)`
	- `RecBuffer::getHeader(struct HeadInfo *head)`
	- `RecBuffer::getRecord(union Attribute *rec, int slotNum)`
#### Stage 3
- The Disk Buffer and Catalog Caches
- Creates
	- `BlockBuffer::loadBlockAndGetBufferPtr`
	- `StaticBuffer::StaticBuffer()`
	- `StaticBuffer::~StaticBuffer()`
	- `StaticBuffer::getBufferNum(int blockNum)`
	- `RelCacheTable::getRelCatEntry(int relId, RelCatEntry *relCatBuf)`
	- `RelCacheTable::recordToRelCatEntry(Attribute record[], RelCatEntry *relCatEntry)`
	- `AttrCacheTable::getAttrCatEntry(int relId, int attrOffset, AttrCatEntry *attrCatBuf)`
	- `AttrCacheTable::recordToAttrCatEntry(Attribute record[], AttrCatEntry *attrCatEntry)`
	- `OpenRelTable::OpenRelTable()`
	- `OpenRelTable::~OpenRelTable()`
- Updates
	- `BlockBuffer::getRecord(union Attribute *rec, int slotNum)`
	- `BlockBuffer::getHeader(struct HeadInfo *head)`
#### Stage 4
- Linear Search on Relations
#### Stage 5
- Opening Relations
#### Stage 6
- Buffer Management and Disk Write-back
#### Stage 7
- Inserting Records into Relations
#### Stage 8
- Creating and Deleting Relations
#### Stage 9
- Selection and Projection on Relations
#### Stage 10
- B+ Tree Search on Relations
#### Stage 11
- Index Creation and Deletion
#### Stage 12
- Join on Relations

