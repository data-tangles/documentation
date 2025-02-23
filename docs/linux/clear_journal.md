# Clear Journal

Sometimes you need to clear the journal to clear disk space on a host. Below are some examples of commands to clear the journal.

## Examples

Retain the last 100MB of data

```
sudo journalctl --vacuum-size=100M
```

Retain the last 10 days of data

```
sudo journalctl --vacuum-time=10d
```

## Parameter Explanation

The following can be found while running `man journalctl`

```
--vacuum-size=, --vacuum-time=, --vacuum-files=

Removes the oldest archived journal files until the disk space they use 
falls below the specified size (specified with the usual "K", "M", "G" and 
"T" suffixes), or all archived journal files contain no data older than 
the specified timespan (specified with the usual "s", "m", "h", "days", 
"months", "weeks" and "years" suffixes), or no more than the specified 
number of separate journal files remain. Note that running --vacuum-size= 
has only an indirect effect on the output shown by --disk-usage, as the 
latter includes active journal files, while the vacuuming operation only 
operates on archived journal files. Similarly, --vacuum-files= might not 
actually reduce the number of journal files to below the specified number, 
as it will not remove active journal files.
```

```
--vacuum-size=, --vacuum-time= and --vacuum-files=

may be combined in a 
single invocation to enforce any combination of a size, a time and a 
number of files limit on the archived journal files. Specifying any of 
these three parameters as zero is equivalent to not enforcing the specific 
limit, and is thus redundant.
```