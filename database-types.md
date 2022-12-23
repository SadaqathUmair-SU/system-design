## Types of Database </br>
**Relational Database** </br>
**Non Relational Database** 


## Relational Database

**Schema** </br>
**ACID**

**Schema** in relational DB refer's to, how the data is going to be structured.
One of the reasons to use Schema is, if the requirement has some constraints.


**ACID Properties:**

**Atomicity** - It is a transaction in database which ensures the transaction happens successfully or doesn't happens at all.</br>
**Consistency** - It ensures to maintain consistency while reading the data so that while fetching the data doesn't gets changed at any point of time.</br>
**Isolation** - It ensures the occurrence of multiple transactions concurrently without a database state leading to a state of inconsistency.</br>
                For ex: Let's there are two transactions happening read and write respectively.</br>
                Isolation ensures to do the transactions in a sequence i.e, to read first and to write it at second.</br>
**Durability** - It ensures that changes made to the database (transactions) that are successfully committed will survive permanently, even in the case of system failures.</br>

**Few below points where Relational DB is difficult to use:**</br>
1. Let's say if you not aware of any constraints and seems like the requirement is going to be changed frequently in the future at that point it is difficult to keep on updating scehema.</br>
2. Using Schema, Vertical Scaling can happen easily whereas Horizontal Scaling is bit difficult


## Non Relational Database

