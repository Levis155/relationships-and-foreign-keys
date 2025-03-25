# Relationships and Foreign Keys


## What is a relationship?

A relationship is simply a connection or association between 2 or more tables. 

- For our instance, we are going to form a relationship between the person table and the car table. By doing so, we can perform operations like assigning a car to a person and query a combination of car and person to view who owns which car alongside their details and details of the car they own.

- Foreign keys help us form these relationships.

## So then what is a foreign key?

In very simple terms, a foreign key is a column in a table that help us form a relationship with another table through the primary key of that other table.

- For our instance if we wanted to have a corelation between person data and car data without the existence of foreign keys we would have to include additional columns with the car data to our person table. This is however not best practice and can quickly get tedious.

- Thanks to foreign keys. we only need to add a single extra column to table person and this column will act as the foreign key.

- This foreign key will connect to table car through the the primary key of car (which is the id field)

## How do we add foreign keys to our table?

To demonstrate this we'll add a foreign key to table person and we'll call this foreign key **car_id**

We'll start by dropping the tables and then we'll recreate them but this time adding a foreign key to person **(car_id)**

Before adding the foreign key however, there are a number of things we should note:

1. Since the foreign key is connecting to the primary key in car, these two should have the same data type. **However, in our case we can't give our foreign key the BIGSERIAL data type to match id in car because BIGSERIAL is incremented automatically for each row. We'll give it the BIGINT data type which is basically the same type as BIGSERIAL but is'nt incremented for each row.**

1. We will not add the **NOT NULL** constraint because not every person in this relationship is going to be assigned a car.

1. We'll use the REFERENCES keyword to specify which column in car we will refer to to form this relationship (this is usually the primary key).

1. We'll add the **UNIQUE** constraint because only one car can be assigned to each person in the relationship we're going to form. e.g. person 1 and person 2 cannot share the same car (car_id).

   With this in mind the foreign key will thus be added as follows:

```sql
create table person (
	id BIGSERIAL NOT NULL PRIMARY KEY,
	first_name VARCHAR(50) NOT NULL,
	last_name VARCHAR(50) NOT NULL,
	email VARCHAR(50),
	date_of_birth DATE NOT NULL,
	country_of_birth VARCHAR(50) NOT NULL,
	car_id BIGINT REFERENCES car(id),
	UNIQUE (car_id)
);
```
And with this a relationship has been formed between car and person.

Confirm that the foreign key has been added:

```sql
SELECT * FROM person;
```

## How do we update the foreign key?

Now that a relationship has been formed between car and person, we can go ahead and and assign cars to random people. (**This is basically what updating foreign keys is**)

For instance we can assign car 3 to person 7 by executing:

```sql
UPDATE person SET car_id=3 WHERE id=7;
```

Confirm person 7 has now been assigned car 3:

```sql
SELECT * FROM person;
```

Finally as a note we cannot assign a person a car that doesn't exist.

For our scenario, we only have 5 cars and thus cannot assign car 6 for instance. Thus the following command returns an error:

```sql
UPDATE person SET car_id=6 WHERE id=2;
```