# kOS - kFS
kFS is the main filesystem made for the kOS project.  
It uses a file table with hashed paths to index all the files and stores these files into multiple disk sectors.

This document outlines an alpha version - **kFS is not yet finished**.  
This document is subject to change.

## Layout
There are two formats used in kFS - the [File Index header](#File-Index) (used for the file table) and the [File Storage header](#File-Storage) (used to store the data).  
The file index table starts at the top of the partition's available space (first sector -> last sector), and the file storage sectors go from the bottom of the partition's available space (last sector -> first sector).

## Headers
### File Index
This format makes up the file table used for kFS.

```c
// NOTE: this is NOT packed to keep up speed.

typedef struct kFS_File_Index
{
    uint8_t[64] path_hash;
    uint64_t    sector;
    uint64_t    parent_folder;
    uint16_t    attributes;
} kFS_File_Index;

enum kFS_File_Index_Attributes
{
    kFS_File_Index_Attributes_Is_Folder  = 1 << 0,
};
```

| Offset | Content | Type | Limits | Extra notes
|---|---|---|---|---|
| 0 | Path hash (SHA-512) | `uint8_t[64]` |  |
| 64 | (Storage header) sector | `uint64_t` | Must be smaller than the number of sectors. | That sector contains the [File Storage header](#File-Storage). Followed by the data. |
| 72 | Path hash of the parent folder | `uint8_t[64]` |  | This is set to 0 for the root folder. |
| 136 | Attributes | `uint16_t` |  | See below. |

| Bit | Attribute |
|---|---|
| 0 | Sets whether this file is a folder. |

### File Storage
```c
// NOTE: this is NOT packed to keep up speed.

typedef struct kFS_File_Storage_Header
{
    uint8_t[64] path_hash;
    uint16_t    attributes;
    uint64_t    sector_next;
    uint64_t    sector_previous;
    uint16_t    size_data_on_sector;
    uint8_t*    data;
} kFS_File_Storage_Header;

enum kFS_File_Storage_Header_Attributes
{
    kFS_File_Storage_Header_Attributes_Is_First_Sector  = 1 << 0,
    kFS_File_Storage_Header_Attributes_Is_Last_Sector   = 1 << 1
};
```

| Offset | Content | Type | Limits | Extra notes |
|---|---|---|---|---|
| 0 | Path hash | `uint8_t[64]` |  |  |
| 64 | Attributes | `uint16_t` |  | See below. |
| 66 | Next sector | `uint64_t` | Must be smaller or equal to the number of sectors available in total. Must be bigger than the previous sector. | Set to 0 if `kFS_File_Storage_Header_Attributes_Is_Last_Sector` is set. |
| 74 | Previous sector | `uint64_t` | Must be smaller or equal to the number of sectors available in total. Must be smaller than the next sector unless it's 0. | Set to 0 if `kFS_File_Storage_Header_Attributes_Is_First_Sector` is set. |
| 82 | Size of the data on the sector | `uint16_t` | Must be smaller than the sector size. |
| 84 | File data | `uint8_t*` | The length of the data is always smaller or equal to the cumulative size of the sectors used. |

| Bit | Attribute | Notes |
|---|---|---|
| 0 | Sets whether this is the first sector for the file or not. | Incompatible with bit 1. |
| 1 | Sets whether this is the last sector for the file or not. | Incompatible with bit 0. |
