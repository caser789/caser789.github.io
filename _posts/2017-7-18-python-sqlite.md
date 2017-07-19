---
layout: post
title: sqlite in python
date:   2017-07-18 16:18:01 +0800
categories: python sqlite
tag: sqlite
---

* content
{:toc}

```
$ sqlite3 test.db
sqlite> .tables
sqlite> .exit
$ ls
test.db
sqlite> .mode column
sqlite> .headers on
```

## a example to get sqlite version

```
#!/usr/bin/python
# -*- coding: utf-8 -*-

import sqlite3 as lite
import sys

conn = None

try:
    conn = lite.connect('/tmp/test.db')
    cur = conn.cursor()
    cur.execute("SELECT SQLITE_VERSION()")
    data = cur.fetchone()
    print "SQLite version: {}".format(data)
except lite.Error as e:
    print "Error {}".format(e.args[0])
    sys.exit(1)
finally:
    if conn:
        conn.close()
```

## context manager

```
#!/usr/bin/python
# -*- coding: utf-8 -*-

import sqlite3 as lite

conn = lite.connect('/tmp/test.db')
with conn:

    cur = conn.cursor()
    cur.execute("SELECT SQLITE_VERSION()")
    data = cur.fetchone()
    print "SQLite version: {}".format(data)
```

## create table and insert

```
#!/usr/bin/python
# -*- coding: utf-8 -*-

import sqlite3 as lite

conn = lite.connect('/tmp/test.db')
with conn:

    cur = conn.cursor()
    cur.execute("CREATE TABLE Cars(Id INT, Name TEXT, Price INT)")
    cur.execute("INSERT INTO Cars VALUES(1, 'Audi', 52642)")
    cur.execute("INSERT INTO Cars VALUES(2,'Mercedes',57127)")
    cur.execute("INSERT INTO Cars VALUES(3,'Skoda',9000)")
    cur.execute("INSERT INTO Cars VALUES(4,'Volvo',29000)")
    cur.execute("INSERT INTO Cars VALUES(5,'Bentley',350000)")
    cur.execute("INSERT INTO Cars VALUES(6,'Citroen',21000)")
    cur.execute("INSERT INTO Cars VALUES(7,'Hummer',41400)")
    cur.execute("INSERT INTO Cars VALUES(8,'Volkswagen',21600)")
```

## create table and insert with executemany

```
#!/usr/bin/python
# -*- coding: utf-8 -*-

import sqlite3 as lite

cars = (
    (1, 'Audi', 52642),
    (2, 'Mercedes', 57127),
    (3, 'Skoda', 9000),
    (4, 'Volvo', 29000),
    (5, 'Bentley', 350000),
    (6, 'Hummer', 41400),
    (7, 'Volkswagen', 21600)
)

conn = lite.connect('/tmp/test.db')
with conn:
    cur = conn.cursor()
    cur.execute("DROP TABLE IF EXISTS Cars")
    cur.execute("CREATE TABLE Cars(Id INT, Name TEXT, Price INT)")
    cur.executemany("INSERT INTO Cars VALUES(?, ?, ?)", cars)
```

## executescript

```
#!/usr/bin/python
# -*- coding: utf-8 -*-

import sqlite3 as lite
import sys
try:
    conn = lite.connect('/tmp/test.db')
    cur = conn.cursor()
    cur.executescript("""
                      CREATE TABLE Cars(Id INT, Name TEXT, Price INT);
                      INSERT INTO Cars VALUES(1,'Audi',52642);
                      INSERT INTO Cars VALUES(2,'Mercedes',57127);
                      INSERT INTO Cars VALUES(3,'Skoda',9000);
                      INSERT INTO Cars VALUES(4,'Volvo',29000);
                      INSERT INTO Cars VALUES(5,'Bentley',350000);
                      INSERT INTO Cars VALUES(6,'Citroen',21000);
                      INSERT INTO Cars VALUES(7,'Hummer',41400);
                      INSERT INTO Cars VALUES(8,'Volkswagen',21600);
                      """)
    conn.commit()

except lite.Error as e:
    if conn:
        conn.rollback()
    print "Error {}".format(e.args[0])
    sys.exit(1)
finally:
    if conn:
        conn.close()
```

## last row id

```
#!/usr/bin/python
# -*- coding: utf-8 -*-

import sqlite3 as lite
conn = lite.connect('/tmp/test.db')

with conn:
    cur = conn.cursor()
    cur.execute("CREATE TABLE Friends(Id INTEGER PRIMARY KEY, Name TEXT);")
    cur.execute("INSERT INTO Friends(Name) VALUES ('Tom');")
    cur.execute("INSERT INTO Friends(Name) VALUES ('Rebecca');")
    cur.execute("INSERT INTO Friends(Name) VALUES ('Jim');")
    cur.execute("INSERT INTO Friends(Name) VALUES ('Robert');")
    lid = cur.lastrowid
    print "The last Id of the inserted row is {}".format(lid)
```

## fetch all

```
#!/usr/bin/python
# -*- coding: utf-8 -*-

import sqlite3 as lite
conn = lite.connect('/tmp/test.db')

with conn:
    cur = conn.cursor()
    cur.execute("SELECT * FROM Cars")
    rows = cur.fetchall()
    for row in rows:
        print row
```

## fetch one

```
#!/usr/bin/python
# -*- coding: utf-8 -*-

import sqlite3 as lite
conn = lite.connect('/tmp/test.db')

with conn:
    cur = conn.cursor()
    cur.execute("SELECT * FROM Cars")
    while True:
        row = cur.fetchone()
        if row is None:
            break
        print row[0], row[1], row[2]
```

## dict cursor

```
#!/usr/bin/python
# -*- coding: utf-8 -*-

import sqlite3 as lite
conn = lite.connect('/tmp/test.db')

with conn:
    conn.row_factory = lite.Row
    cur = conn.cursor()
    cur.execute("SELECT * FROM Cars")
    rows = cur.fetchall()
    for row in rows:
        print '%s %s %s' % (row['Id'], row['Name'], row['Price'])
```

## parameterized query by ?

```
#!/usr/bin/python
# -*- coding: utf-8 -*-

import sqlite3 as lite

uId = 1
uPrice = 62300
conn = lite.connect('/tmp/test.db')

with conn:
    cur = conn.cursor()
    cur.execute("UPDATE Cars SET Price=? WHERE Id=?", (uPrice, uId))
    conn.commit()
    print "number of rows updated: %d" % cur.rowcount
```

## parameterized query by placeholder

```
#!/usr/bin/python
# -*- coding: utf-8 -*-

import sqlite3 as lite

uId = 4
conn = lite.connect('/tmp/test.db')

with conn:
    cur = conn.cursor()
    cur.execute("SELECT Name, Price FROM Cars WHERE Id=:Id", {"Id": uId})
    conn.commit()
    row = cur.fetchone()
    print row[0], row[1]
```

## insert image
```
#!/usr/bin/python
# -*- coding: utf-8 -*-

import sqlite3 as lite


def readImage():
    with open('/tmp/woman.jpg', 'rb') as fin:
        img = fin.read()
        return img


with lite.connect('/tmp/test.db') as conn:
    cur = conn.cursor()

    data = readImage()
    binary = lite.Binary(data)

    cur.execute("INSERT INTO Images(Data) Values (?)", (binary, ))
    conn.commit()
```

## retrieve image

```
#!/usr/bin/python
# -*- coding: utf-8 -*-

import sqlite3 as lite


def writeImage(data):
    with open('woman2.jpg', 'wb') as fout:
        fout.write(data)

with lite.connect('/tmp/test.db') as conn:
    cur = conn.cursor()
    cur.execute("SELECT Data FROM Image LIMIT 1")
    data = cur.fetchone()[0]
    writeImage(data)
```

## metadata pragma

```
#!/usr/bin/python
# -*- coding: utf-8 -*-

import sqlite3 as lite


with lite.connect('/tmp/test.db') as conn:
    cur = conn.cursor()
    cur.execute("PRAGMA table_info(Cars)")
    data = cur.fetchall()
    for d in data:
        print d[0], d[1], d[2]
```

[refs](http://zetcode.com/db/sqlitepythontutorial/)

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
