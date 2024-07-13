---
layout: page
title: Session 5
parent: Assignments
---

# Homework Assignment: MongoDB Operators

## Overview

In this assignment, you will practice using various MongoDB operators. The tasks will involve field operators, array operators, query operators, projection operators, and update operators. You will perform CRUD operations to demonstrate your understanding of these operators.

## Objectives

1. Understand and apply field operators in MongoDB.
2. Manipulate arrays using array operators.
3. Construct queries using comparison, logical, and element operators.
4. Use projection operators to shape query results.
5. Perform updates using update operators.

## Tasks

### Setup

1. Ensure you have MongoDB installed and running on your machine.
2. Use MongoDB Compass or the MongoDB shell for executing the commands.

### Database and Collection Setup

1. Create a database named `school`.
2. Create a collection named `students` with the following documents:

```json
[
  {
    "_id": 1,
    "name": "Alice",
    "age": 21,
    "grades": [85, 90, 92],
    "address": { "city": "New York", "zip": "10001" }
  },
  {
    "_id": 2,
    "name": "Bob",
    "age": 23,
    "grades": [78, 82, 85],
    "address": { "city": "Los Angeles", "zip": "90001" }
  },
  {
    "_id": 3,
    "name": "Charlie",
    "age": 20,
    "grades": [88, 90, 95],
    "address": { "city": "Chicago", "zip": "60601" }
  }
]
```

### Field Operators

1. Use the `$set` operator to update Alice's city to "San Francisco".
2. Use the `$min` operator to update Bob's age to 22 if the current value is greater than 22.
3. Use the `$max` operator to update Charlie's highest grade to 100 if it is less than 100.
4. Use the `$mul` operator to multiply Alice's age by 1.1.
5. Use the `$unset` operator to remove Bob's zip field from the address.
6. Use the `$rename` operator to rename the address field to location for Charlie.
7. Use the `$currentDate` operator to add a lastModified field with the current date to Alice's document.

### Array Operators

1. Use the `$push` operator to add the grade 89 to Bob's grades.
2. Use the `$pop` operator to remove the last grade from Alice's grades.
3. Use the `$pull` operator to remove the grade 90 from Charlie's grades.
4. Use the `$[]` operator to increment each grade in Bob's grades by 5.
5. Use the `$[identifier]` operator to multiply each grade in Alice's grades by 1.1 if the grade is less than 90.

### Query Operators

1. Use the `$eq` operator to find the student(s) with the age equal to 23.
2. Use the `$gt` operator to find the student(s) with grades greater than 90.
3. Use the `$gte` operator to find the student(s) with age greater than or equal to 21.
4. Use the `$in` operator to find the student(s) with the name in ["Alice", "Charlie"].
5. Use the `$lt` operator to find the student(s) with age less than 21.
6. Use the `$lte` operator to find the student(s) with age less than or equal to 23.
7. Use the `$ne` operator to find the student(s) with the age not equal to 23.
8. Use the `$nin` operator to find the student(s) with the name not in ["Bob"].
9. Use the `$and` operator to find the student(s) with age greater than 20 and grades containing 90.
10. Use the `$or` operator to find the student(s) with age less than 21 or grades containing 85.
11. Use the `$not` operator to find the student(s) without grades equal to 90.
12. Use the `$nor` operator to find the student(s) without age 20 or grades 85.
13. Use the `$exists` operator to find the student(s) with an address.
14. Use the `$type` operator to find the student(s) with the grades field of type array.

### Projection Operators

1. Use projection to include only the name and age fields in the results of a query that finds all students.
2. Use projection to exclude the address field from the results of a query that finds all students.

### Update Operators

1. Use the `$inc` operator to increment Charlie's age by 1.
2. Use the `$set` operator to update Bob's name to "Robert".
3. Use the `$unset` operator to remove the grades field from Alice's document.
4. Use the `$push` operator to add a new grade to Charlie's grades.
