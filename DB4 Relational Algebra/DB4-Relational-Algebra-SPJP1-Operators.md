## Relational Algebra Intro, Part 1

Key - attribute whose value guaranteed to be unique.

Let's say we have College, Student, and Apply relations (aka tables). 

Simplest query is: Student

Would get a copy of the Student relation. 

### SELECT operator

Select operator denoted by a Sigma with a subscript. 

σGPA > 3.7 Student

Would return subset of student table containing rows with GPA bigger than 3.7

If we want those in additional to high school size less than 1000, we would write

σGPA > 3.7 ^ HS < 1000 Student

where ^ is equal to AND

What about those who applied to the Stanford CS major?

σcName = 'Stanford' ^ major = 'cs' Apply

### PROJECT operator

Project will pick certain columns. Write using Greek pi symbol. 

πsID (student id), dec (decision) Apply

This will spit out the apply table but it will just have the student id and decision columns. 

### Combining operators

To select some rows and some columns at the same time. 

Let's get the ID and name of students with GPA > 3.7.

πsID, sName(σGPA > 3.7 Student)

### Syntax


σCondition(Expression)

πA1,...,An(Expression)

### Quiz

Two SELECTS in a row can be replaced by one SELECT that ANDS ^ the two conditions. 

Two PROJECTS in a row can be replaced by just the outer projection, since the outer projection must be a subset of the inner one. 