---
layout: post
title: PostgreSQL basic commands
categories: [database]
tags: [postgresql, sql, command]
comments: true
---

PostgreSQL basic commands

### Connect to postgresql from command line

```
$ psql DBNAME USERNAME
```

Example:

```
$ psql -h localhost -p 5432 -U hungvi -w
```

### List up databases

```
\list
```

### Select database

```
\connect DBNAME
```
OR:
```
\c DBNAME
```

### How to exit from PostgreSQL command line utility: psql
[stackoverflow.com](https://stackoverflow.com/questions/9463318/how-to-exit-from-postgresql-command-line-utility-psql)
```
$ \q
```
