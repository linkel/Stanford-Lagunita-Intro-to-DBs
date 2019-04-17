## The Natural Join

The natural join performs a cross product on your relations, but it also enforces equality on attributes with the same name! So it will get rid of your copies of duplicate attributes. 

It'll only combine tuples with the same name. So if the student SID is the same as the apply SID, it combines them and you won't have the student.SID and apply.SID columns. 

Convention is that it's written with a BOWTIE! 

Let's collect the names and GPAs of students with HS > 1000 who applied to CS and were rejected.

You'd write 

PROJECT StudentName, GPA (SELECT HS > 1000 AND Major = 'CS' AND decision = 'Rejected' (Student NATURAL JOIN Apply))

Okay, what if we're interested in just apps to colleges where the enrollment is bigger than 20,000?

We're gonna join in the college relation! 

Student NATURAL JOIN Apply NATURAL JOIN College

Reminder: Natural Join does not add any more expressiveness to Relational Algebra. You can always rewrite something to use the cross product instead of Natural Join.

## Quiz

Feels good, got 'em right on the first try. 

1. What is the result of the expression: PROJECT sName, cNAME (SELECT HS>enr (SELECT state = 'CA' College NATURAL JOIN Student NATURAL JOIN (SELECT major = 'CS' Apply)))

Students paired with all CA colleges smaller than the student's high school to which the student applied to major in CS. 

2. Which of the following expressions finds the IDs of all students such that some college bears the student's name?

PROJECT sID (SELECT cName = sName (College x Student))