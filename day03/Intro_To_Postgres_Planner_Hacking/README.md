# Intro to Postgres Planner Hacking
* 4 questions when reviewing PRs:
	1. Does it always retain semantic correctness?
	2. Does it inhibit downstream optimizations?
		* Optimization Order Matters
		* An optimization for one query can be a regression for another
		* Planning steps have expectations for the query tree
	3. Is the improvement in execution time worth the cost in planning time?
		* Not in the case of exhaustive join order
	4. Is the complexity cost commensurate with the performance benefit?
* NULL semantics, see slides

