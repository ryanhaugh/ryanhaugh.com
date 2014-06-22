---
layout: post
title: "The Trials & Tribulations of Dynamics"
date: 2014-05-31 -0700
comments: true
categories: [dot-net]
---

People who have worked with me know where I stand on dynamic objects in C# - I strongly dislike them.  I'm sure there is a fantastic use-case for them, but I have not encountered it yet.

And this doesn't just stop with dynamic objects - I'm not a fan of anonymous objects either.  To me, it feels like magic strings masquerading as statically-typed properties.

My biggest complaints are:

* Loss of Intellisense
* Loss of compile-time error checking
* Unable to use data annotations for model validation

That alone is reason enough to rethink their use.  Add in the fact that they're both less efficient than statically-typed objects and ???

It'd be different if we were sacrificing developer maintainability for performance improvements, but to sacrifice developer productivity *and* take a performance hit?  That's just ridiculous!

Long before I joined my current company, the powers that be advocated the use of dynamics when retrieving data from our data store.  Their rational is that the data that's being retrieved from our NoSQL solution is dynamic so we should use dynamic objects.  Makes sense, right?

Except for the fact that we're using them as though they're statically-typed.  What I mean is, we get data back from our data store, de-serialize into a dynamic object, then call properties on that and assume that properties will be the type that we think they should be.

Why is this a bad?  Think about how our tests need to change.

Let's start with how we'd test the following scenario:

public class Course {
	public string Title { get; set; }
	public Professor Professor { get; set; }
	public Student[] Students { get; set; }
}

public class Person {
	public string FirstName { get; set; }
	public string LastName { get; set; }
	
	[JsonIgnore]
	public string FullName {
		get {
			return FirstName + " " + LastName;
		}
	}
}

public class Professor : Person {
	public string Accolades { get; set; }
}

public class Student : Person {
	public string StudentId { get; set; }
	public decimal Gpa { get; set; }
	public int Age { get; set; }
}

public interface ICourseRepository {
	Course GetCourse (string courseId);
}

public class CourseRepository : ICourseRepository {
	public Course GetCourse (string courseId) {
		// Retrieves course data from data store
	}
}

public class SOMETHING {
	private readonly ICourseRepository _courseRepository;
	
	public SOMETHING (ICourseRepository courseRepository) {
		_courseRepository = courseRepository;
	}

	public void CoursePrinter (string courseId) {
		Course course = _courseRepository.GetCourse(courseId);
		
		Console.WriteLine();
	}
}

With this, the unit tests become clear:
* 


* Boxing
* Reflection