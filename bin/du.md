# du

Summarize disk usage of the set of FILEs, recursively for directories.

```
Usage: du [OPTION]... [FILE]...
  or:  du [OPTION]... --files0-from=F


  -B, --block-size=SIZE  scale sizes by SIZE before printing them; e.g.,
                           '-BM' prints sizes in units of 1,048,576 bytes;
                           see SIZE format below

  -h, --human-readable  print sizes in human readable format (e.g., 1K 234M 2G)

  -d, --max-depth=N     print the total for a directory (or file, with --all)
                          only if it is N or fewer levels below the command
                          line argument;  --max-depth=0 is the same as
                          --summarize

  -s, --summarize       display only a total for each argument

  -t, --threshold=SIZE  exclude entries smaller than SIZE if positive,
                          or entries greater than SIZE if negative

  -X, --exclude-from=FILE  exclude files that match any pattern in FILE

The SIZE argument is an integer and optional unit (example: 10K is 10*1024).
Units are K,M,G,T,P,E,Z,Y (powers of 1024) or KB,MB,... (powers of 1000).

```

## examples

Example (human-readable, --max-depth=1)
```
du -h -d 1
```

Example (display MB)
```
du -BM
```

Example (display GB)
```
du -BG
```

sort by filesize
```
du -h -d 1 | sort -h
```