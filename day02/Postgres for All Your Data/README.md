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
	* there is a way to constrain values to types on INSERT so that using a CAST becomes safe when doing comparisons
		* using relational constraints not by a supplied JSON schema definition
* Data analytics support in postgres, can run R code on the database
* Analytics - create a read-only slave to keep the work off the main database
* Foreign Data Wrappers to connect to external services using a REST based connection
	* Pushes JOINs, ORDER BYs, and aggregates to other machine

## Part 2 - Non-Relational Postgres
* Arrays are **1 based** NOT 0 based
* Convert arrays into rows (pivot the array) using unnest
* Can take rows and aggregate them into an array
* [ is exclusive and ) is inclusive - see slide 11, it looks like a mistake but it is not when working with a range
* A GIST index can index a range type
* Can make sure that there are no overlapping ranges, see slide 14
* POINT datatype stores x and y co-ordinates together
* GIST index for POINT column will index POINTs
* Can do nearest neighbour search based on the GIST index
* Can write xpath queries in postgres, interesting but somewhat dated for most "modern" databases
* JSON, **not** JSONB
	* is stored as a text field but can do WHERE clauses
	* does support a simple INDEX but JSONB has more (possibly better) support
* JSONB **not** JSON
	* understands types
	* index all keys and values
	* binary search lookup for speedy searches/access
	* will remove duplicate keys and last matching key wins?  Looks like it
	* can use a GIN index - store key once, matches multiple times - good for JSONB
	* with GIN you don't have to specify what field to index, it will index all of them
* Supports full text search, how does it compare to ElasticSearch?  Is it worth testing?  See slide 52
* create a GIN index for full text search queries and the searches will use an index
* can define your own stop words and other lexical values.  It may need to be done through text files - interesting but unlikely to be something I ever work with.
* supports only tri-grams if you use the pg_trgm extension
* the use of tri-grams is quite powerful for full text searching

## Part 3 - Flexible Indexing With Postgres
* why partial indexes?  Well the indexes are smaller
	* say searches are frequently done on a particular city why index all of them?
* can use BRIN indexes to index all columns in a table
* BRIN indexes can be very helpful if you don't yet know how data will be queried
* For BRIN to really work the tables should be ordered (for example time-series) this allows skipping potentially large chunks of the stored data
