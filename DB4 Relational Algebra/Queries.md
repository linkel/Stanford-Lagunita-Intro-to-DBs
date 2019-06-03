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

## Q4

Find all pizzerias that serve at least one pizza for less than $10 that both Amy and Fay eat. 

(\project_{pizzeria} \select_{name='Amy' and price < 10}( Serves \join Eats))
\intersect
(\project_{pizzeria} \select_{name='Fay' and price < 10}( Serves \join Eats))

## Q5

Find the names of all people who eat at least one pizza served by Dominos but who do not frequent Dominos. 

\project_{name}(Eats \join \select_{pizzeria='Dominos'} Serves)
\diff
\project_{name}(\select_{pizzeria='Dominos'} Frequents)

## Q6

Find all pizzas that are eaten only by people younger than 24, or that cost less than $10 everywhere they're served. 

\project_{pizza} (\select_{age < 24} (Person \join Eats)) \diff \project_{pizza} (\select_{age>=24} (Person \join Eats))
\union 
(\project_{pizza} \select_{price < 10} Serves \diff \project_{pizza} \select_{price >= 10} Serves)

## Q7

Find the age of the oldest person (or people) who eat mushroom pizza. 

What is necessary here is to select the people who eat mushroom pizza, and select them again! The second group gets renamed to enable their joining. Then you take the first group and join them with the second group where the names do not match, and you get columns of people matched to other people. After that you'd select the ones who have an age that is less than someone else. Now you can subtract that from the whole group to get the person who does not have an age less than anyone else. 

\project_{age} (\select_{pizza = 'mushroom'} (Person \join Eats))
\diff
\project_{age}(\select_{age < ageclone}(\project_{name, age} (\select_{pizza = 'mushroom'} (Person \join Eats))
\join_{ name <> nameclone } \rename_{nameclone, ageclone} 
(\project_{name, age} (\select_{pizza = 'mushroom'} (Person \join Eats)))))

## Q8

Find all pizzerias that serve only pizzas eaten by people over 30.

First find pizzas eaten by people not over 30. Then diff that with pizzas eaten by people over 30. This will give us pizzas that are only eaten by those not over 30. Get the pizzerias that serve those pizzas. 

Now we get the pizzas eaten by people over 30. Get the pizzerias that serve those pizzas. We diff that with the pizzerias that serve the pizzas only eaten by those not over 30. This is because pizzerias that serve pizzas eaten by people over 30 may also serve pizzas eaten by people not over 30, but by diffing it we remove all those that have pizzas that people not over 30 eat, so now this gives us pizzerias that serve pizzas only eaten by those over 30. 

```
\project_{pizzeria}

(Serves \join (\project_{pizza} (\select_{age > 30} Person \join Eats)))

\diff

\project_{pizzeria} (Serves \join(\project_{pizza} Eats 
\diff
\project_{pizza} (\select_{age > 30} Person \join Eats)))
```

## Q9

```
\project_{pizzeria}(

\select_{pizza  < pizzaclone}(

Serves

\join

\project_{pizza} (
\select_{age > 30} 
    Person \join Eats)

\join

\rename_{pizzeria, pizzaclone, priceclone} (
Serves

\join

\project_{pizza} (
    \select_{age > 30} 
    Person \join Eats)
)))
```

The step before the selection looks like: 
```
Chicago Pizza	cheese	7.75	cheese	7.75
Chicago Pizza	cheese	7.75	supreme	8.5
Chicago Pizza	supreme	8.5	cheese	7.75
Chicago Pizza	supreme	8.5	supreme	8.5
Dominos	cheese	9.75	cheese	9.75
Little Caesars	cheese	7	cheese	7
New York Pizza	cheese	7	cheese	7
New York Pizza	cheese	7	supreme	8.5
New York Pizza	supreme	8.5	cheese	7
New York Pizza	supreme	8.5	supreme	8.5
Pizza Hut	cheese	9	cheese	9
Pizza Hut	cheese	9	supreme	12
Pizza Hut	supreme	12	cheese	9
Pizza Hut	supreme	12	supreme	12
Straw Hat	cheese	9.25	cheese	9.25
```
When the select chooses those where pizza is < pizzaclone, it's comparing the strings and picking one where pizza comes before pizza1 in dictionary order. As you can see, for the ones that only have one pizza type offered, they get excluded because they only have one that's the same. I feel like this works only because there's only 2 pizza types for the ones that offer them all, because if there were 1, 2, and 3 pizza types among those pizzerias all in the set where people over 30 eat them, wouldn't the 2 and 3s both pass through the filter on this query?
