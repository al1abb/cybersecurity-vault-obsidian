**Compression** is a method of encoding that aims to reduce the original size of a file without losing information (or only losing as little as possible).

[[gzip]] or [[bzip2]] can be used from command line to compress and decompress

To figure out what decompression we need to use, look at the first bytes in the hexdump to find the file signature. We can search for these first bytes in a [list of file signatures](https://en.wikipedia.org/wiki/List_of_file_signatures). An alternative would be to use the `find` command.