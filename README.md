CsvReader
=========

A fork from another popular site.
http://www.codeproject.com/Articles/9258/A-Fast-CSV-Reader


Introduction
===========
One would imagine that parsing CSV files is a straightforward and boring task. I was thinking that too, until I had to parse several CSV files of a couple GB each. After trying to use the OLEDB JET driver and various Regular Expressions, I still ran into serious performance problems. At this point, I decided I would try the custom class option. I scoured the net for existing code, but finding a correct, fast, and efficient CSV parser and reader is not so simple, whatever platform/language you fancy.

I say correct in the sense that many implementations merely use some splitting method like String.Split(). This will, obviously, not handle field values with commas. Better implementations may care about escaped quotes, trimming spaces before and after fields, etc., but none I found were doing it all, and more importantly, in a fast and efficient manner.

And, this led to the CSV reader class I present in this article. Its design is based on the System.IO.StreamReader class, and so is a non-cached, forward-only reader (similar to what is sometimes called a fire-hose cursor).

Benchmarking it against both OLEDB and regex methods, it performs about 15 times faster, and yet its memory usage is very low.

To give more down-to-earth numbers, with a 45 MB CSV file containing 145 fields and 50,000 records, the reader was processing about 30 MB/sec. So all in all, it took 1.5 seconds! The machine specs were P4 3.0 GHz, 1024 MB.

Supported Features
==================
This reader supports fields spanning multiple lines. The only restriction is that they must be quoted, otherwise it would not be possible to distinguish between malformed data and multi-line values.

Basic data-binding is possible via the System.Data.IDataReader interface implemented by the reader.

You can specify custom values for these parameters:

Default missing field action;
Default malformed CSV action;
Buffer size;
Field headers option;
Trimming spaces option;
Field delimiter character;
Quote character;
Escape character (can be the same as the quote character);
Commented line character.
If the CSV contains field headers, they can be used to access a specific field.

When the CSV data appears to be malformed, the reader will fail fast and throw a meaningful exception stating where the error occurred and providing the current content of the buffer.

A cache of the field values is kept for the current record only, but if you need dynamic access, I also included a cached version of the reader, CachedCsvReader, which internally stores records as they are read from the stream. Of course, using a cache this way makes the memory requirements way higher, as the full set of data is held in memory.


