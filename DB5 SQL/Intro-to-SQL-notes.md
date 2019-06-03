## Couple of parts to SQL

Data Definition Language:

This is stuff like `create table` or `drop table` that defines the schema of your data. 

Data Manipulation Language:

This is the select/insert/delete/update that composes queries you make on your data. 

Other Commands:

indexes, constraints, views, triggers, transactions, authorization, etc. 

## SELECT

SELECT A1, A2, ...An FROM R1, R2, ...Rn WHERE condition

This is equivalent to relational algebra's

PROJECT A1 ... An (SELECT condition (R1 CROSS PRODUCT R2...Rn, ))

So the relational algebra's select is really the WHERE of SQL, and all those relations get cross product'd. 

In our example, we're using the College, Student, and Apply tables again that we used in relational algebra. 

```
select sID, sName, GPA
from Student
where GPA > 3.6
```

We got the students who have GPA higher than 3.6. 

What if we want students with their names and majors where they applied?

```
select sName, major
from Student, Apply
where Student.sID = Apply sID
```

In SQL we need to write the join condition explicitly. The natural join from RA would already connect them based on the sID since it shares the same name, but in SQL you'll see we've specified. 

SQL has duplicate values here--if we don't want them, we can do `select distinct sName, major` instead. 

What if we want the names of students and their GPAs whose size of high school was less than 1000 and we want CS majors that applied to Stanford? 

```
select sName, GPA, decision
from Student, Apply
where Student.sID = Apply.sID
    and sizeHS < 1000 and major = 'CS' and cname = 'Stanford'
```

In SQL make sure we remember to specify that join information, since it isn't like the natural join in relational algebra!

Check out the following--does it look right?

```
select cName
from College, Apply
where College.cName = Apply.cName
    and enrollment > 20000 and major = 'CS'
```

We get an ambiguous column name error. 

When you refer to an attribute that both tables have, you have to specify which one you want. We happened to set them equal here but in order to run the query, we've gotta choose. So we change it to select College.cName instead of just select cName.

Video gives an example of a bigger join on all the tables. 

### SQL is an unordered model

If we want our results to be sorted a certain way, we will specify with the ORDER BY clause. 

order by GPA desc;
order by GPA asc (default behavior is ascending)

We can specify another attribute to sort by as well.

order by GPA desc, enrollment;

So enrollment will tiebreak. 

### The LIKE predicate

Like lets us do some simple string matching. 

```
select sID, major
from Apply
where major like `%bio%`;
```

We can see students who have applied to majors that have bio in the name.

### SELECT *

What if we want all attributes from a query?We use the asterisk, or the star. Now we get everything.

```
select *
from Student, College;
```

This gets us the cross product of Student and College.

### Arithmetic

You can do arithmetic in queries!

```
select sID, sName, GPA, sizeHS, GPA*(sizeHS/1000.0)
from Student;
```

This query modifes the GPA based on the size of their high school. The bigger the HS, the bigger the GPA.