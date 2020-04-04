A file is a set of blocks. A block contains many records but is usually not "full." There is space in a block to hold newly inserted records.

 

The DB needs to know which block to use for a newly created record/row. It can use the free space map.
