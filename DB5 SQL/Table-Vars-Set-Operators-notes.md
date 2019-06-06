## Table Variables

1) they make queries more readable
2) they rename relations used in the FROM clause, for example especially we have two instances of the same relation

We kinda did that in RA, didn't we?

## Set operators

Union, Intersect, and Except (which is like the minus operator from RA)

## Examples

For table variables, we can do something like

from Student S, College C, Apply A

And then we can do A.sID or S.sID to write shorter bits. This is usage number one. 

Now for usage number two. 

We might have two instances of the Student table that we want to look at a cross product and look at the GPA of students who have the same GPA. 

select S1.sID, S1.sName, S1.GPA, S2.sID, S2.sName, S2.GPA
from Student S1, Student S2
where ... what goes here? 

Now, if we just did something like S1.GPA = S2.GPA then we'd also get students who are the same student paired with themselves. 

So instead, we can do a where S1.GPA = S2.GPA and s1.sID <> S2.sID. 

But this still will pair a student A with a student B and also havev another row with B with A, so it's a duplicate with different order. 

Now we can do 
where S1.GPA = S2.GPA and s1.sID < S2.sID. This will only make it one way, instead of using the less equal. 

## Union operator example

So if we do

select cName from College
union
select sName from Student

This will now combine these names all together, a jumble of student names and college names. And it'll just slap a label to show the result. In the example the label was cName. 

If we want to unify the name as name, then

select cName as name from College
union
select sName as name from Student

In the example, the names happened to come out sorted. Professor says that the union operator in SQL eliminates duplicates by default, and she is using SQLite to show examples, and SQLite sorts the result to get rid of dupes. Other flavors may not do this. 

What if we want to keep duplicates? 

We say `union all`!

What if we want this result to be sorted? 

ORDER BY name. 

## Intersect operator example

select sID from Apply where major = 'CS'
intersect
select sID from Apply where major = 'EE'

Boom, we get students who applied to both CS and EE majors. 

Some dbs don't support intersect operator. Since intersect never added any additional language power but is actually equivalent to a thoughtful ANDing. 

select A1.sID
from Apply A1, Apply A2
where A1.sID = A2.sID AND A1.major = 'CS' AND A2.major = 'EE'

This still has duplicates, so we just have to change `select` to `select distinct`. 

## Except operator example

select sID from Apply where major = 'CS'
except
select sID from Apply where major = 'EE'

Same dealio. Now, again, some dbs don't support the except operator. Then we must rewrite it like we did with intersect, right? 

Naively we might think we can slap on a "where major doesn't equal EE", like so:

select distinct A1.sID
from Apply A1, Apply A2
where A1.sID = A2.sID AND A1.major = 'CS' AND A2.major <> 'EE'

But we still get a bunch of results. Why? Hey, it's because we are finding all pairs of apply records where it's the same student and the applied to CS in one pair and they didn't apply to EE in the other. So we've found students who applied to CS and also applied to another major that's not EE. This means that for example a student who applied to CS and applied to Biology and applied to EE would still get selected by our query! Because they do fit the query of applying to CS and applying to something not EE. Joke's on us, with the current constructs we have in SQL we cannot rewrite the except/minus query without that operator. 

It's like we discussed back in relational algebra--union and minus operator add expressive power. Intersection operator doesn't, and that's why we can rewrite that one. 

But there's more constructs to be revealed that let us do what we want...! 