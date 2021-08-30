# kFS

## Preamble

kFS is the main filesystem made for kOS. It uses a file table with hashed paths to index all the files and stores these files into multiple HDD sectors.

## The layout

There are two formats used in kFS - the [File Index format](File%20Index.md) (used in the file table) and the [File Storage format](File%20Storage.md) (used to store the data). The file index table starts at the top of the partition's available space (Last Sector -> First Sector) and the file storage sectors go from the bottom of the partitions available space (First Sector -> Last Sector).

## Links to other relavent files

[File Index format](File%20Index.md)<br>
[File Storage format](File%20Storage.md)