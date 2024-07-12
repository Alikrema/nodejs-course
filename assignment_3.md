---
layout: page
title: Session 4 - Assignment 2
parent: Assignments
---

# Mongo Exercises

### MongoDB Query Exercises

**Note:** Assume you have a MongoDB collection called `movies` which contains documents with the following structure:

```json
{
  "_id": ObjectId(),
  "title": "Inception",
  "director": "Christopher Nolan",
  "year": 2010,
  "genre": ["Action", "Sci-Fi", "Thriller"]
}

```

### Exercise 1: Insert a Document

Insert a new document into the `movies` collection for the movie "The Matrix", directed by "Lana Wachowski", released in 1999, and classified under genres "Action" and "Sci-Fi".

### Exercise 2: Insert Multiple Documents

Insert the following two movies into the `movies` collection:

1. "Avatar", directed by "James Cameron", released in 2009, genres: "Action", "Adventure", "Fantasy".
2. "The Avengers", directed by "Joss Whedon", released in 2012, genres: "Action", "Adventure", "Sci-Fi".

### Exercise 3: Find a Document

Retrieve the details of the movie "Inception".

### Exercise 4: Find Multiple Documents

Find all movies that were released in the 2000s (from the year 2000 to 2009 inclusive).

### Exercise 5: Update a Document

Update the movie "The Matrix" to add the genre "Drama" to its list of genres.

### Exercise 6: Update Multiple Documents

Increase the year of release by 1 for all movies released before 2000.

### Exercise 7: Delete a Document

Delete the movie "Avatar" from the collection.

### Exercise 8: Delete Multiple Documents

Delete all movies that have the genre "Drama".

### Exercise 9: Count Documents

Count how many movies are in the `movies` collection.

### Exercise 10: Find with Projection

Retrieve only the title and director of each movie in the `movies` collection.

### Submission

For each exercise, write the MongoDB query you would use to perform the specified operation. Test your queries using a MongoDB environment such as the MongoDB shell or a GUI tool like MongoDB Compass. Record the outcomes or any interesting observations you make while executing these queries.