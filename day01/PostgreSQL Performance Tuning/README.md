# Server Architecture & Server Tuning

* pg_xlog renamed to pg_wal
* uses file system for storage, nothing written to abstract? Sounds like it
* not in shared_buffers requests data from OS
* modification happens in shared_buffers - changes get written to the log right away to ensure consistency
* larger caches will quickly increase performance to a point, then it levels off
* split into 16MB chunks
	* helps with cleanup
	* avoid forcing checkpoints by filling transaction logs (which is a problem in other dbs)
	* Assumptions made that overwriting an already allocated file will be more performant, this may / may not be true based on underlying filesystem (for example zfs will not benefit from this feature and it can be disabled)
* for checkpoints large shared_buffers can be detrimental to performance based on the schedule for checkpoints to occur
* default checkpoints is 5 minutes which quite often is too frequent (30-60 minutes can be better)
* going higher than 30-60 minutes has the downside of causing the database to take longer to recover from a disaster
* spread checkpoints do just that, spread out checkpoints over a given time making the disk hit spread over time
* too many checkpoints can also cause the transaction log to fill faster, which causes a checkpoint, which then causes a large transaction log, which then causes a checkpoint, so don't do it too frequently!
* ideally 90-95% of checkpoints should be triggered by timeout - easier to predict, database has a better chance of doing a spread checkpoint
* checkpoint_completion_target - percentage of time between checkpoints to finish writing to disk, 0.5 is 50% of the time to complete.  If 30 minute checkpoints it will spread out writes over 15 minutes.
* background writer behaviour has a very conservative amount of data written, it could be increased to flush more out without detrimentally impacting user queries.  It has to be used in such a way that won't write so much data that it does
* how does one know the size of the cache?
	1. look at system catalogs: \d pg_stat_database for blocks read and blocks hit (compute hit ratio) select blks_hit * 100.0 / (blks_hit + blks_read + 1.0) from pg_stat_database;
	2. pg_buffercache can give insight into the question as well
memory budget of about 4GB should be fine?  Did anyone else pick up on this one?

## Basic Tuning 
* as per suggestions, take with grain of salt and determine based on workload
* better to be conservative with max_connections and use a connection pooler, ~2x CPU cores is a good estimate
* shared_buffers start low (higher than the default of 128MB) maybe start at 1GB, run for a week, take measurements, slowly increase if performance improves.  Once it doesn't improve then revert to previous values because you've reached a plateau and continuing to size up will have little to no impact.
* for lots of writes take a look at cache hit ratio - try to improve that value possibly through increasing shared_buffer size
* work_mem may have to be increased for very complex queries whether or not it is because of analytics or not
* higher max_connections, lower memory to balance the amount of memory used
* maintenance_work_mem - large changes rarely show large performance gains, tune in small increments
* for reasonably fast storage increasing effective_io_concurrency to something like 16 can increase performance

Basic tuning talked about will get about 95% of the way to solving server perf issues, it might be better to spend additional time on the application accessing the database.

## VACUUM, Freezing & Avoiding Wraparound
* readers and writers do **not** block each other
* modifications (updates) will actually do a copy of the row, will mark old version as updated get it ready for delete and then insert the new row
* special fields on every table, xmin, xmax, and x
* changes rolled back are simply marked as aborted until a commit occurs
* VACUUM removes the dead rows, can be run manually
* long running queries (read / write) can block VACUUM from freeing up rows.  COMMITs need to be done as soon as possible.
* don't leave transactions idle
* vacuum verbose table_name;
* vacuum doesn't necessarily free space it marks allocated space as available
* vacuum full will free space to the OS but it is a sledgehammer that locks the whole table and is resource intensive
	* vacuum full is rarely the good choice, think hard before using it
* database can monitor what is happening on a table and can decide how often to use vacuum (autovacuum)
* increasing the number of autovacuum_max_workers will not make vaccuming speed, it will just make each individual worker slower thereby taking the same amount of time
* rule of thumb, if it hurts to run a vacuum do it more often
* postgres has extremely long defaults for autovacuuming - it should likely be tuned to happen more frequently.
* select * pg_stat_all_tables;
	* n_dead_tup (garbage in table)
	* n_live_tup (someone has a view of these, will not be vacuumed)
* set vacuum size based on the percentage value of the largest table in the database

## Index Types
* default index type is B-tree
	* index up to 32 columns - for insane people that want tables that are larger than the 8k page size
* hash indexes are crash safe but have fewer features than B-tree
	* if you believe that hash indexes will be faster then double check what is being done with B-tree it probably isn't true
* indexes not necessarily a silver bullet and can actually be ignored by query planner.
	* work on a realistice amount of data
	* test the index results vs non-indexed to ensure it is worth having or has been indexed properly
* be careful adding and **removing** indexes, make sure the indexes are annotated so that others understand why they exist.  A once a year used index may be very important and deleting it would be a bad idea.
