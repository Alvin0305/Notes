#### Disk Class
```C++
Disk();
~Disk();
static int readBlock(unsigned char *block, int blockNum);
static int writeBlock(unsigned char *block, int blockNum);
```

- Total 8192 Blocks
- Each block is 2048 B
- First 4 blocks reserved for BMAP
- 5th block reserved as Relation Catalog
- 6th block reserved as first block for Attribute Catalog
- Rest of the blocks used for storing records or attribute catalog

##### Disk() constructor
- There is a `disk` and a `disk_run_copy` file in the `Disks/` folder
- On calling the Disk() constructor, we copy the content of `disk` to `disk_run_copy`
- Whatever we do in nitcbase will be reflected in `disk_run_copy` only
- This ensures that, the data in disk won't get corrupted even if we force stop nitcbase

```C++
Disk::Disk() {
  std::ifstream src(DISK_PATH, std::ios::binary);
  std::ofstream dst(DISK_RUN_COPY_PATH, std::ios::binary);

  dst << src.rdbuf();
  src.close();
  dst.close();
}

```
##### ~Disk() destructor
- Called on graceful termination of nitcbase
- On calling ~Disk() destructor, we copy back the contents of `disk_run_copy` back to `disk`

```C++
Disk::~Disk() {
  std::ifstream src(DISK_RUN_COPY_PATH, std::ios::binary);
  std::ofstream dst(DISK_PATH, std::ios::binary);

  dst << src.rdbuf();
  src.close();
  dst.close();
}
```

##### readBlock(unsigned char \*block, int blockNum)
- Used to read a specific block from the disk to the pointer `block`
- Opens the `disk_run_copy` file.
- Checks if the `blockNum` is valid
- read the particular block into the pointer `block`
	- start = `blockNum` \* `BLOCK_SIZE`
	- length = `BLOCK_SIZE`

>[!info]
>readBlock returns an int 
>- SUCCESS
>- E_OUTOFBOUND

```C++
int Disk::readBlock(unsigned char *block, int blockNum) {
  FILE *disk = fopen(DISK_RUN_COPY_PATH, "rb");
  if (blockNum < 0 || blockNum > DISK_BLOCKS - 1) {
    return E_OUTOFBOUND;
  }
  const int offset = blockNum * BLOCK_SIZE;
  fseek(disk, offset, SEEK_SET);
  fread(block, BLOCK_SIZE, 1, disk);
  fclose(disk);
  return SUCCESS;
}
```

##### writeBlock(unsigned char \*block, int blockNum)
- Used to write data from the pointer `block` to a specific block in the disk
- Opens the `disk_run_copy`
- Checks if the `blockNum` is valid
- write the contents of `block` to the particular block
	- start = `blockNum` \* `BLOCK_SIZE`
	- length = `BLOCK_SIZE`

```C++
int Disk::writeBlock(unsigned char *block, int blockNum) {
  FILE *disk = fopen(DISK_RUN_COPY_PATH, "rb+");
  if (blockNum < 0 || blockNum > DISK_BLOCKS - 1) {
    return E_OUTOFBOUND;
  }
  const int offset = blockNum * BLOCK_SIZE;
  fseek(disk, offset, SEEK_SET);
  fwrite(block, BLOCK_SIZE, 1, disk);
  fclose(disk);
  return SUCCESS;
}
```

>[!info]
>writeBlock returns an int 
>- SUCCESS
>- E_OUTOFBOUND

