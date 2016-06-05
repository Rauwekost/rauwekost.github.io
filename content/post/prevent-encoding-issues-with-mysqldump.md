+++
date = "2016-02-04T21:07:00+02:00"
description = "When exporting a database with mysqldump you can corrupt the encoding pretty easily."
title = "Prevent encoding issues with mysqldump"
tags = ["code", "database"]

+++
When exporting a database with mysqldump you can corrupt the encoding pretty easily.

take:

```
mysqldump -u user -p database > dump.sql
```

the above command is simple and effective but it doesn't care about encodings and redirects the output to a file instead of to standard output.

To make the command care about encodings we can use the following options:

* default-character-set: ensures the character-set for each field
* result-file: prevents data from passing through the operating system which has its own encoding and might mess up the dump data.

With the new options it looks like this:

```
mysqldump -u user -p --default-character-set utf8 --result-file database.sql database
```

this way you can always asure the right encoding and save yourself some headaches!