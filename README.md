---
topics: scraping, sqlite, sql
languages: ruby, sql, sqlite
resources:
---

##Gotta Scrape 'Em All
We've found all 151 pokemon! They're in the `pokemon_index.html`, but if we want to catch them then we have to get them into our database.  This is going to mean scraping.  The other thing we're going to need is a database.

###Database Scraping
To make a database we're going to do so in the `db/` directory.  This is where all sql files should go.  Inside `db/` is a file called `schema_migration.sql`.  There you will write your sql statements to create a table with the proper schema.  The schema should an id column, name column, and a type column. The latter two should be `varchar 255` and the former an integer.

###Scraping
Once the schema is set up we then need can start scraping, and catching our pokemon. However, we still need methods to save our pokemon to the database.  Now we have to decide whose job is it save information about the pokemon to the database.  Is it the scraper, who is grabbing all the data? Or is it the pokemon themselves?  For our purposes, it is the pokemon.  What happens if we decide to add, remove, or even change something about the pokemon? The scraper just shouldn't be responsible about knowing information about the pokemon.  Therefore, we'll need to create a `create` method for the class pokemon that will take the name and the types and insert them into the database using raw sql.

One thing we want to make sure is that our pokemon are getting saved with the right index number.  On the `pokemon_index.html` every pokemon is in the correct order and has an id.  Bulbasaur is the first and starts at one.  Luckily, our sqlite database has auto incrementing based id system that starts at one.  That means if we scrape starting from Bulbasaur every other pokemon will have an id that is created when they are inserted into the database.  But beware, if you create and delete a row that id number will forever be used, and the next id will be the id following the deleted one.