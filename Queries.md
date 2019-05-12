## Q1

Find all pizzas eaten by at least one female over the age of 20. 

\project_{pizza} (\select_{gender='female' and age > 20} (Eats \join Person))

Seems like I need the parentheses! 

## Q2

Find the names of all females who eat at least one pizza served by Straw Hat. (Note: The pizza need not be eaten at Straw Hat.) 

\project_{name} (\select_{gender='female'} ((Eats \join (\select_{pizzeria='Straw Hat'} Serves)) \join Person))

## Q3

Find all pizzerias that serve at least one pizza for less than $10 that either Amy or Fay (or both) eat. 

\project_{pizzeria} (\select_{(name='Amy' or name='Fay') and price < 10} (Serves \join Eats))

