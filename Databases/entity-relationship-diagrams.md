# Entity Relationship Diagrams

While you can use [lucidCharts](https://lucid.app/pricing/lucidchart#/pricing) you can also use [app.diagrams.net](https://app.diagrams.net/) (formerly [draw.io](draw.io)) to map out the ERDs.

## General Terms

1. An **Entity** is any data you want to track (A customer, and order, a product, etc.)
   - Think of a **row** in a table
2. An **Attribute** is a certain aspect of the datapoint you want to track (customer_id, customer_firstName, etc.)
3. A **Record** is a single entity datapoint (Customer #547/ order #3273813, etc.)

## Cardinalities

Entities are related to each other, and lines are drawn to shown these connections. Cardinalities are little shapes attached to the end of the lines to explain the relationships even further.

1. [ X ]-----|-[ Y ] X has one Y
2. [ X ]------<[ Y ] X has Many Y
3. [ X ]----||-[ Y ] X has one and only one Y
4. [ X ]----o|-[ Y ] X has zero or one Y
5. [ X ]-----|<[ Y ] X has one or many Y
6. [ X ]-----o<[ Y ] X has zero or many Y

7. [ X ]>------[ Y ] Y has many X
8. [ X ]-|-----[ Y ] Y has one X
   etc.
9. [ X ]>----|-[ Y ] x has one Y **AND** Y has many X
   etc.

Example:

```
|---------------|                |--------------|
|Customer       |                |Order         |
|---------------|                |--------------|
|PK| Customer_ID|-||--|          |PK|Order_ID   |
|FK|Order_ID    |     |--------o<|FK|Customer_ID|
|  |            |                |  |           |
|  |            |                |  |           |
|---------------|                |--------------|
```

**Note:** When drawing the lines, try to match up PK and FK between Entities where possible

## Primary Keys (PK), Foreign Keys (FK), and Composite Primary Keys (CPK)

1. A **Primary Key** is an attribute that identifies every record within an entity
   - PKs must be unique
   - PKs Must never change
   - PKs can't be null
   - Only one PK can exist per record
2. A **Foreign Key** is a Primary Key located in a different record/entity
   - Foreign Keys can be repeated in a table
   - An entity can have multiple different types of foreign keys
