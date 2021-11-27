---
title: "Find Duplicate File Names Using Diff"
created: 2021-11-26 11:49:50
tags: search
keywords: filenames, duplicate, diff, directory, directories, same
---

# Find Duplicate File Names Using Diff

## Find identical file names

```bash
diff -sqr dir1/ dir2/ | grep identical
```

## Find file names "Only in" directory

```bash
diff -sqr dir1/ dir2/ | grep "Only in"
```

-s | --report-identical-files
: Report when two files are the same

-r | --recursive
: Recursively compare any subdirectories found

-q | --brief
: Output only whether files differ
