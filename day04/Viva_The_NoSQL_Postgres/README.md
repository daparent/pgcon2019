# Viva, the NoSQL Postgres!
* SQL/JSON part of the 2016 standard
* **last key wins** when keys are duplicated, that how dedupes are done in JSONB
* Why use Mongo when Postgres has JSONB?  That is the question...
* JSquery developed in 2014 to make the queries much more concise
* Postgres popularity grew very much in line with the JSONB support in the database
	* due to this Postgres is still very interested in developing JSON features
* SQL/JSON standard specifies a JSON null, not necessarily the same thing as a SQL null
* JSON path language is planed for PG12
* two modes for JSON path
	* lax
		* missing keys are ignored
		* arrays are unwrapped
	* strict
		* missing keys return null
		* requires an exact nesting
* multiple filters are supported in a single expression, just keep adding on ?'s
* like_regex is like a Postgres regex - likely using the same engine so that makes sense
* SELECT jsonb_path_query(jsonb '<expression (that can include a filter)>')
* PG 11 JSONB queries are big and complex 
* SQL/JSON has a datetime field but it is not part of JSON, Postgres does not support it in PG12
	* Didn't implement because datetime is not immutable - there are timezones, adding support would make JSONB not immutable meaning it wouldn't be indexable
	* Will get a bad jsonpath representation if datetime method is used
* Gentle guide to JSONPATH is a bit easier to digest than the reference manual that Postgres usually generates
* JSON path use internal engine rather than postgres which has a bit more overhead so query result times are a bit faster with JSON path
* JSON path compared to Mongo was faster in a single test - obviously not a thorough or exhaustive test of performance between the two technologies
* PG12 scheduled for November of 2019 as No-Mess-Martino pointed out
