---
layout: post
title: "SQL TIP: Auto Increment in an UPDATE statement."
date: 2004-02-28 -0800
comments: true
disqus_identifier: 215
categories: [code]
---
I needed to create a temp table in SQL with a column that contained an
incrementing integer, without making that column an identity.

For example, suppose I want to select record from a table of users, but
add a column that contains an incrementing counter. The data in the
table should look like so:

**counter**

**UserID**

**Email**

...

0

**3432**

BillBrasky@SNL.COM

...

1

**7913**

zoolander@howsmyhair.com

...

2

**8372**

donaldtrump@ImSoRich.com

...

...

To start, I can run the following query to create the temp table:

```csharp
SELECT counter = 0, * FROM Users INTO #tmp_Users
```

This creates a temp table named \#tmp\_USers where the column "counter"
is set to 0 for each row.

In order to update the counter column so that each row has a counter
value greater than the previous row, run the following statement.

```csharp
DECLARE @counter int
SET @counter = 0
UPDATE #tmp_Users
SET @counter = counter = @counter + 1
```

If you’re looking to master your T-SQL Fu, I recommend this book. It
provides a great reference for advanced T-SQL querying techniques. Keep
in mind that this book is not for beginners. It gets into some deep
analysis of how querying works in SQL 2005

