### Part 2

What if we want a list of application majors and student decisions?

Pi major, decision Apply

This query would have a lot of duplicate values--but Relational Algebra language would return NO DUPLICATES. Different from SQL. SQL is based on Multisets, but Relational Algebra is only sets (for now in this discussion).

### Cross-product or Cartesian product

Combine two relations. So if we do the cross product of Student X Apply, we're going to get this relation with 8 attributes.

Reminder:

College had college name, state, and enrollment.

Student had student ID, student Name, GPA, and high school size.

Apply had student ID, college Name, major, and decision (y/n). 

So the cartesian product gives this relation that consists of the attributes of student and of apply. It'd say Student.sID and Apply.sID for the ids that are the same with the dot and prefix to denote which relation it came from. 

If the student table had S tuples and the apply relation had A tuples, the cross product will have S X A number of tuples. 

### Specific query

We want to get the names and GPAs of students with HS > 1000 who applied to CS and were rejected. 

We're going to grab Student and Apply and CROSS PRODUCT them first. Now we've got this fat relation.

Now we take a SELECT. 

σ student.sID = Apply.sID 
^ HS > 1000
^ Major = CS
^ Decision = "Rejected"

And we PROJECT ahead of the above, the student name and GPA. 

### Quiz

One of the answers uses a PROJECT that loses the student ID and is wrapped in a SELECT that tries to select student ID, but it's gone by then so it wouldn't work. 