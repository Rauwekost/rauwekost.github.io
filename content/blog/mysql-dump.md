---
author: "Robert den Harink"
date: 2016-02-04
title: Preventing encoding issues with mysqldump
tags: [programming, databases]
colour: red
---

Take the good old command:

```bash
mysqldump -u user -p database > dump.sql
```
the above command is simple and effective but it doesn’t care about encodings and redirects the output to a file instead of to standard output. This can lead to all sorts of encoding issues because of the local settings on the operating system and in the database.

To make the command care about encodings we can use the following options:

* default-character-set: ensures the character-set for each field
* result-file: prevents data from passing through the operating system which has its own encoding and might mess up the dump data.

With the new options it looks like this:

```bash
mysqldump \
  -u user \
  -p \
  --default-character-set utf8 \
  --result-file database.sql \
  database
```

This way you can always asure the right encoding and save yourself some headaches!
