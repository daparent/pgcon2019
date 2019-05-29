# Postgres for All Your Data

## Part 1 - Making Postgres Central in Your Data Center
* adding datatypes is as simple as inserting a row into the pg_type table
* can overload types for the datatypes - so overload +, -, >, <, etc.
* all internal datatypes follow the exact same rules as anything a user can create
* adding custom tables to the data model is handled through contrib modules right now
* system tables have ways to squirrel away data, not sure this is what one would really want to do though
* plpgsql can be used but you can also install plpythonu
	* By default plpythonu is set to Trusted false, it is set that way because it can access OS and resources outside of plpgsql
	* plpythonu can only be used by superusers, which is too bad
* plpgsql is Trusted true allowing all users to use it, it is protected
* PL/v8 is not really supported anymore due to difficulties pulling v8 out of chrome
* There are extensions that add additional functionality to postgres, they just need to be installed
* According to the speaker plug-ins are "not a pain"
* NoSQL development increases over time?  I'd like to understand this better
* Slides go over strengths and weaknesses well based on the presenter's point over view
* NoSQL with JSONB
	* use it as key/value store
	* can be indexed
	* ->> returns a value as a text field, the slide does not have a typo (slide 28)
	* there are support functions for getting JSON types but it would likely require a case statement in the SQL to check the type and then do appropriate comparison
* Data analytics support in postgres, can run R code on the database
* Analytics - create a read-only slave to keep the work off the main database
* Foreign Data Wrappers to connect to external services using a REST based connection
	* Pushes JOINs, ORDER BYs, and aggregates to other machine
