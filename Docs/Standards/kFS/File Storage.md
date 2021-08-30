# kFS File Storage format

This format is used for actual file storage for kFS.

## Layout

<pre>
<--------><~><~><--------><--------><--><...>
 1         2  3  4         5         6  7
</pre>

### Key

`<>` = section <br>
`~` = 1 bit<br>
`-` = 1 byte<br>
`n-` = `n` bytes<br>
`...` = variable number of bytes<br>

### Sections
Index | Stores | Type | Limits | Extra notes
-- | - | - | - | - 
1 | File index on table | uint64_t | < number of files
2 | Is last sector | boolean
3 | Is first sector | boolean
4 | Next sector | uint64_t | =< number of sectors | [Next/Last sector sections](#next-and-last-sector-sections)
5 | Last sector | uint64_t | =< number of sectors |  [Next/Last sector sections](#next-and-last-sector-sections)
6 | Length of data on sector | uint16_t | < smaller than sector size
7 | File data | char*| length < sector size

### Next and Last sector sections

If section 3 is set then section 5 won't be present and if section 2 is set then section 4 won't be present.