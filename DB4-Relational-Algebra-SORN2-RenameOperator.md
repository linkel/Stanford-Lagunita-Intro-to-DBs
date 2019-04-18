## Rename Operator

Uses "ro" letter. (Looks like a P.)

1. P R(A1, ..., An)
2. P R(E)
3. P A1, ..., An(E)

General form is #1, the rest are abbreviations. 

### Unifying schema

The rename operator unifies schemas for set operators. Remember how she mentioned that schemas for union and intersection needed the same schema?

PROJECT cName College UNION PROJECT sName Student

The above is technically not correct! You have to rename, to make the schema match. 

(RENAME c(name) (PROJECT cName College)) UNION (RENAME c(name) (PROJECT sName Student))

A syntactic necessity.

### Disambiguation in "self-joins"

Okay, so let's say we want to find pairs of colleges in the same state. 
We have Stanford and Berkeley, and Berkeley and UCLA. We have to combine two instances of the college relation...

SELECT state = state (College CROSS PRODUCT College)

But how's it know which state is which? It doesn't. That doesn't make sense. We actually should rename those two instances of College table so that they have different names...

SELECT s1 = s2 (RENAME c1(name1,state1,enrollment1) (College) CROSS PRODUCT RENAME c2(name2,state2,enrollment2) (College))

Here's another way to do it! Without the cross product!

RENAME c1(name1,state,enrollment1) (College) NATURAL JOIN RENAME c2(name2,state,enrollment2) (College)

The natural join is going to require equality on the two states, since they're named the same in the above query, so it will do what we want, mostly! --problem with the query is that colleges are gonna get paired with THEMSELVES! We don't want that. 

So we need a condition where name1 is not equal to name2. 


SELECT n1 != n2 (RENAME c1(name1,state,enrollment1) (College) NATURAL JOIN RENAME c2(name2,state,enrollment2) (College))

Yep! However, we will still get duplicates in a way--Stanford Berkeley and Berkeley Stanford, while repeats, will still show up due to the order difference. 

SELECT n1 < n2 (RENAME c1(name1,state,enrollment1) (College) NATURAL JOIN RENAME c2(name2,state,enrollment2) (College))

What? So now we only get colleges that are "less than" the second college. We only get Berkeley Stanford, since b is less than s in the alphabet (or ASCII? Not sure how this evaluates it). But that's pretty clever! 

The rename operator was needed to do these queries. It does add expressiveness to the language. 

## Quiz

1. Suppose relation Student has 20 tuples. What is the minimum and maximum number of tuples in the result of this expression:
RENAME s1(i1,n1,g,h) Student NATURAL JOIN RENAME s2(i2,n2,g,h) Student

ANSWER: min is 20, max is 400. 

This is because if every student had a unique GPA and highschool combo, then students join only with themselves and there are 20 tuples in the result. If every student has identical GPA and high school, then all pairs would join and you'd have 20 x 20. 

2. Suppose relations College, Student, and Apply have 5, 20, and 50 tuples in them respectively. Remember that cName is a key for College. Do not assume sName is a key for Student. Do assume that college names in Apply also appear in College. What is the minimum and maximum number of tuples in the result of this expression:

PROJECT cName College UNION RENAME cName(PROJECT sName Student) UNION PROJECT cName Apply

Minimum is definitely 5, right? Since if maybe they're all the same, then college's keys are cName so those are unique, so at minimum 5 are unique. 

If every student has a unique name and none of them are college names, then there's 20 from students and 5 from colleges, but apply's colleges are all also in College as specified in the question, so no new things come from Apply.
