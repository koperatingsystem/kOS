# kFS File Index format

This format makes up the file table used for kFS

## Layout

<pre>
<--------><32-><--------><~><-------->
 1         2    3         4  5
</pre>

### Key

`<>` = section <br>
`~` = 1 bit<br>
`-` = 1 byte<br>
`n-` = `n` bytes<br>
`...` = variable number of bytes<br>

### Sections

Index | Stores | Type | Limits | Extra notes
- | - | - | - | - 
1 | File index | 64 bit unsigned int | < number of files |
2 | SHA-256 filename hash | 32 byte hash |  |
3 | Start sector | 64 bit unsigned int | =< number of sectors  | 
4 | Is folder? toggle | 1 bit boolean | =< number of sectors |  File = False<br>Folder = True
5 | Parent folder | 64 bit unsigned int | < number of files | [Root folder](#root-folder)

TODO: Add permissions

### Root folder

The root folder's file index is always 0<br>
In the root folder section 5 is not present since it is not needed, there is no parent folder for the root folder