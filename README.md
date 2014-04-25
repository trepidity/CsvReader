CsvReader
=========

A fork from another popular site.
http://www.codeproject.com/Articles/9258/A-Fast-CSV-Reader


Introduction
=========
One would imagine that parsing CSV files is a straightforward and boring task. I was thinking that too, until I had to parse several CSV files of a couple GB each. After trying to use the OLEDB JET driver and various Regular Expressions, I still ran into serious performance problems. At this point, I decided I would try the custom class option. I scoured the net for existing code, but finding a correct, fast, and efficient CSV parser and reader is not so simple, whatever platform/language you fancy.

I say correct in the sense that many implementations merely use some splitting method like String.Split(). This will, obviously, not handle field values with commas. Better implementations may care about escaped quotes, trimming spaces before and after fields, etc., but none I found were doing it all, and more importantly, in a fast and efficient manner.

And, this led to the CSV reader class I present in this article. Its design is based on the System.IO.StreamReader class, and so is a non-cached, forward-only reader (similar to what is sometimes called a fire-hose cursor).

Benchmarking it against both OLEDB and regex methods, it performs about 15 times faster, and yet its memory usage is very low.

To give more down-to-earth numbers, with a 45 MB CSV file containing 145 fields and 50,000 records, the reader was processing about 30 MB/sec. So all in all, it took 1.5 seconds! The machine specs were P4 3.0 GHz, 1024 MB.


