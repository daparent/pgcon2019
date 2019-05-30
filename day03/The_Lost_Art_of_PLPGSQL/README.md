# The Lost Art of PLPGSQL

* Why plpgsql?
	* server side work, has an effect on round trips
	* it provides a stable api
	* simplifies portability
		* usually works from version to version and across operating systems
		* when the code is written in plpgsql it makes it harder to move away from postgres
* four types of postgres functions
	* internal functions
	* query language functions (functions written in SQL)
	* procedural language functions
	* and 1 more that is on the slide if the slides ever show up on the website
* you **can** write stored procedures in pure SQL
* meant to be a trusted language
	* you can run it within postgres and it can't make it outside of that world
* when doing dynamic query execution "get diagnostics" can be used for finding row count and call stack information
	* Read 42.5.5 Obtaining the Result Status
* PLPGSQL from these old eyes have a format very similar to PLSQL - it is different but it still looks familiar
* $ quoting seems to be a suggested way of developing PLPGSQL
* how to plpgsql:
	* user defined functions
	* [do scripts](https://www.postgresql.org/docs/current/sql-do.html)
	* stored procedures
* DO scripts can use languages other than PLPGSQL
* Can **not** run vacuums in a DO script
* stored procedures:
	* in postgres, functions are equivalent to stored procedures, and can be used interchangeably
		* not quite true, especially if coming from PLSQL
		* a lot can be ported from PLSQL to functions but not everything
	* SQL standard syntax for stored procedures
	* allow transaction control
	* real stored procedures started showing up in postgres 11
	* real stored procedure is very close to a function with some subtle syntax differences
	* '=' equality equal for comparison ':=' for assignment equal - '=' can still do assignment but it is considered bad style and could create subtle bugs if used for assignment.
	* INOUT variables is an Oracle influence, it makes for easier migration of PLSQL where it is used and is now supported in postgres
	* transaction control:
		* can commit, rollback, commit all within a stored procedure - could be useful in a conditional situation allowing for rollback to happen if something needs to be thrown out.
		* you can **not** do vacumms in stored procedures
	* nesting stored procedures - there be dragons due to transaction contexts and how they nest...
* can't do scriptable vacuums - probably ok for our work, we'll live with autovacuum until there is a problem
* what doesn't work:
	* certain ddl - create index concurrently
	* vacuum support
	* multiple result-sets
