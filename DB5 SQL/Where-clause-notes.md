# Subqueries in WHERE clause

You can write a subquery in the WHERE clause. 

It's kinda like the relational algebra equivalent--you can write some kind of select statement where CONDITION, and condition is another select clause selecting a group with some conditions. 

It's like, select student ids, names, from Student and Apply where the student sID is equal to the apply sID and major is equal to CS...

You can do a "select student id, student name from Student where student id is in (select student id from apply where major equals CS).

There is an issue with duplicates to consider. 

## Using a join instead of a subquery

