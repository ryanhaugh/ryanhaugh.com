---
layout: post
title: "Your Code Smells Funny - Part 1"
date: 2014-06-22 -0700
comments: true
categories: [dot-net code-smells]
---

![](/images/stock/badsmell.jpg)

What's that smell?  It could be your code...

Let's face it, writing good quality, guilt-free code is tough.  There are just so many ways in which we can mess it up.

The common saying is that if you like the code you wrote six months ago, you haven't learned enough.  For me, I feel like every week I'm finding out that code I wrote the previous week has some sort of new smell that I have never encountered before.

It's not that I'm constantly repeating the same mistakes; it's that I found a brand new way to make my code have an odor.  I want to have dull, stink-free code - how hard can that be?  Hard...

In this series of posts I'm going to identify common code smells and how we can de-stinkify the problem so we get reek-free code.

I'm going to start with the low-hanging fruit and progress to more complex and interesting smells.

First up: **Unnecessary IEnumerable Materialization.**

What the heck is that?

Simply put, it's when you call **ToArray()** / **ToList()** on an IEnumerable&lt;T&gt; to force the IEnumerable to load its items into memory.

To be clear, there is absolutely nothing wrong with using these methods **in the right circumstance**, but **not everything** warrants the use of this.

Let's first talk about where we would use these methods.

A good use-case is when you're querying a database via Entity Framework, NHibernate, LINQ-To-SQL, or some other ORM.  When you use LINQ to query a database, the database isn't queried until you actually consume the LINQ expression.  In other words, consider the following code (that uses Entity Framework):

```csharp
var PeopleContext db = new PeopleContext();

var peopleOver30 = from p in db.People
				   where p.Age > 30
				   select p;

db.Dispose();

foreach (var person in peopleOver30) {
	...
}
```

Let's break down what's happening:

* We're instantiating the variable **db** as a **PeopleContext**
* Then assigning a LINQ expression to the variable **peopleOver30** to get all people whose age is over 30
* Then disposing of **db**
* Finally, we're iterating over **peopleOver30** 

There's a problem with this: **peopleOver30** is an IQueryable (which inherits from IEnumerable) and is dependent on **db** to retrieve the people over 30 from the database, but we've disposed of **db** before materializing **peopleOver30**.  This will result in an exception being thrown because **db** has been told that it's no longer needed.

Let's instead initialize **peopleOver30** like this: 

```csharp
var peopleOver30 = (from p in db.People
				   where p.Age > 30
				   select p).ToList();
```

By calling **ToList()** on the LINQ expression, we're triggering the underlying IQueryable to be processed, which will generate the required SQL from the LINQ expression and query the database to get the results, and return the results as a list of People.

This is a big difference.

In the original code, we were deferring the querying of the database until it was needed in the foreach loop, which was too late because **db** was already disposed of.  In the second example, we're immediately querying the database and storing the results in a List, meaning **peopleOver30** is no longer dependent on **db**.

So I've shown you a case where **ToList()** is needed.  But that's not the point of this post.  I want to show you cases where it is **not** needed.

Consider this:

```csharp
public IEnumerable<Car> GetCars() {
	return new List<Car> {
		new Car {
			Make: "Toyota",
			Model: "Camry"
		},
		new Car {
			Make: "Hyundai",
			Model: "Elantra"
		}
	};
}

public void SomeContrivedMethod() {
	IEnumerable<Car> cars = GetCars();
	
	if (cars.Any()) {
		foreach (var car in cars) {
			...
		}
	}
}
```

If you use ReSharper, you'll notice that ReSharper is giving you the warning **Possible multiple enumeration** on **cars** because we're consuming **cars** twice (**cars.Any()** and **foreach(var car in cars)**).  This is because **cars** is an IEnumerable, and ReSharper can't determine whether **cars** is materialized (i.e. stored in memory) or it has to do something like query a database (like the variable **peopleOver30** was doing in our first example).

So, being one that likes having  ReSharper-error-free files, you make a change to get rid of this error:

```csharp
List<Car> cars = GetCars().ToList();
```

Problem solved, right?  Now Resharper knows that this is a List, so we can't possibly be doing something like re-querying the database.

Here's the problem: even though ReSharper wasn't sure whether **cars** was materialized, we knew.

How?

Because we can look at the code inside the method **GetCars()** and see that it's simply returning a List.  By calling **ToList()** on an IEnumerable that's already storing a List, we're needlessly creating an duplicate List just to store the same values as the original.

In this case, ReSharper was falsely indicating that there may be an issue, so you can simply ignore this one (or suppress the error with a comment).