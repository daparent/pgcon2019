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

