# OTD 1.0.0
This document outlines basic requirements for an OTD file and should be used in conjunction with the [schema.json](./schema.json) file provided.


## File Format
The OTD records described above are stored as utf-8 encoded file format containing the messages described above in json-seq format as described by RFC-7464.  This is as opposed to using a single json array to house all records.  This approach becomes inconvenient when managing large data collects as typical parsers attempt to read all data into memory prior to the parse.  RFC7464 also allows stream style writing of the file and minimal corruption if a writer is abruptly shut down before flushing a buffer (vs having the entire file not be formatted correctly).  See the JSON streaming wikipedia page for a nice overview of the various issues that can be accounted.

Files are stored using the extension ".otd".  Typically files are compressed after creation as a gzip (.gz) file.  At the user’s discretion, large files may be stored in multiple file chunks.  It is recommended that this “chunk” size be on the order of 100MB uncompressed.  Wherever possible, it is desirable to not break tracks across files (though parsers should be able to handle it).  

For transmission and archival of data, all files *should* be stored in a .tar archive, which is in turn compressed using gzip (.tar.gz).  If "chunking", files should lead with ascending order numeric identifiers followed by the “_” character, ideally zero padded to the largest file size.  This is recommended when doing “offline” conversions of data already stored in files.  Date stamp prefixes are also acceptable, e.g. “YYYYMMDDHHmmSS_”.  This is preferred when outputting a file from a live stream of input data as the total amount can be open ended.  It can also be used in the offline case so long as the timestamp corresponds to the first data record in the original input data.

All records in the file *shall* be in time ascending order when created as a result of an offline conversions.  All records *should* be in time ascending order when recording live data streams.  This is in deference to the fact that some systems may have complicated data streams “under the hood” which cause measurements to arrive at the output out of order.  Implementers might want to include a buffer of a few seconds to resolve this disorder.

## Nulls
When a field is not required and is not filled out by a writer, the field is simply excluded from the structure.  The field should not include an empty string or object.  That said, if a reader encounters such an output, it should be robust such that it treats it the same as an excluded field.

## Precision
In general, care should be taken not to output excessive precision.  Doing so unnecessarily increases the size of the OTD file.  For example, ECEF coordinates and velocities likely should only be output to one or two decimal places.  Anything beyond that would be well below the accuracy of the data source.  For smaller data sets this doesn't present an issue, but in aggregate such increases can unnecessarily significantly bloat the file.

## Timestamps
All time data should follow [ISO-8601](https://en.wikipedia.org/wiki/ISO_8601).

While any properly formatted ISO8601 string of millisecond precision is acceptable, writers should strive to use the format to be used corresponds to:
YYYYMMDDTHHmmss.fffffffffZ

Where:

```
YYYY = four digit year
MM = two digit month (01 = January, 12 = December)
DD = two digit day of the month (01, 02, …, 30, 31)
HH = two digit hour using 24 hour clock
mm = two digit minute between 00 and 59
ss = two digit second between 00 and 60 (where 60 is only used to denote an added leap second)
fffffffff = sub-second up to 9 (minimum of 3) decimal places (1,000,000,000ths place) for nanosecond precision
Z = Zulu time; UTC +00:00
```

Note 1: This timestamp should correspond to the middle of the measurement/integration period from the sensor.

Note 2: This is effectively the time of applicability for the sensor data and the time of measurement for the track data.

Note 3: Do not zero pad fractional seconds beyond known clock accuracy.

## Aux Data
The auxiliary bucket is a generic “catch all” container that can be used to house any detailed data necessary. It is free form and so should not be counted on by parsers.  In general if a field is set, it should be considered applicable from that timestamp forward even if it is excluded from later records (as a space savings on static data).

If aux data changes as a function of time, the writer may also choose to write it out only on the timestamps where it changes.  If a writer is only outputting on change and is operating in a “chain style” recording, the aux info must be rewritten to the first record of the new file.  This is to ensure that all otd files can be processed in a standalone fashion.

### Well known aux fields
Some aux fields are considered "well known" and are thus documented in [schema.json](./schema.json).  Where feasible/applicable, these fields names should be used by parsers rather than adding further field names.
