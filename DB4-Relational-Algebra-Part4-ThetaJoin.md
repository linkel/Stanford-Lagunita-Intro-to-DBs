## Theta Join

Doesn't add expressiveness, like the Natural Join. 

It takes two expressions and combines them using Natural Join but it has a subscript theta. 

Expression1 THETA JOIN Expression2

(looks like a bowtie with subscript THETA)

It is the same as SELECT sub theta (Exp1 x Exp2).

This is the default join in most relational databases--it combines two expressions using the cross product and then selects based on the theta condition. 

## More explanation

https://stackoverflow.com/questions/7870155/difference-between-a-theta-join-equijoin-and-natural-join

The Natural Join is usually not present in DBMS because it isn't that useful in that context. 

For example we have two relations here, the Product table and the Component table. 

Product(PName, Price)
====================
Laptop,   1500
Car,      20000
Airplane, 3000000


Component(PName, CName, Cost)
=============================
Laptop, CPU,    500
Laptop, hdd,    300
Laptop, case,   700
Car,    wheels, 1000


If we theta joined using equality:

SELECT *
FROM Product
JOIN Component
  ON Product.Pname = Component.Pname

We get the following!

|  PNAME | PRICE |  CNAME | COST |
----------------------------------
| Laptop |  1500 |    CPU |  500 |
| Laptop |  1500 |    hdd |  300 |
| Laptop |  1500 |   case |  700 |
|    Car | 20000 | wheels | 1000 |

What if instead of the earlier product table we had:

Product(PName, Cost)
====================
Laptop,   1500
Car,      20000
Airplane, 3000000

Since Cost is the same name as Cost in Component, if we had NATURAL JOIN'd this, it would remove the duplicate columns. None of the numbers in Cost for Product and Cost for Componet match, so we'd get 0 results. Unhelpful!

The Theta Join allows joining on more conditions than just equality, but equality is what most people use for JOINs in DB management systems, and most are optimized for it. 

