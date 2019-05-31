# Breaking PostgreSQL at Scale
* Be careful about speed on a new database - decisions that seem to have no impact on performance can easily become problems as data grows
* For small databases (around 10GB):
	* they should be able to fit into memory for the server they are deployed to
	* Backing up a small database like this would be a simple pg_dump (5GB takes about 90 seconds)
	* Tuning on a small database would be memory-related parameters
	* See Tuning values on slides that will eventually be available on [thebuild.com](https://thebuild.com)
	* Upgrades should be a dump/restore
* For sort of bigger databases (100GB or so):
	* memory on server should fit at least the top 1-3 largest indexes
		* ideally, effective_cache_size > largest index
	* pg_dump won't cut it, PITR backups: pgBackRest
	* See Tuning values on slides that will eventually be available on [thebuild.com](https://thebuild.com)
	* Watch logs when tuning especially for work_mem
	* Load balancing read traffic to streaming replicas but be aware that there are lags
	* recommendation is to have the app route queries whether they are read or write
	* pgpool as the option if your app can't figure it out
	* Start monitoring the database - minimally run logs through pgbadger
	* Start monitoring queries for query performance, they may need tuning
	* Will start noticing the lack of indexes during queries
		* but only create indexes when they are absolutely necessary, they have overhead:
			* disk space
			* insert performance since the index has to be updated as the insert occurs
		* if in doubt start with a BRIN index rather than a b-tree index, they are very small and take little disk space.  If the perfomance still isn't there then move to another index type based on data being indexed
	* High availability starts becoming important around this time too:
		* pgpool2
		* Patroni
	* Upgrade using pgupgrade which offers in-place and low downtime
		* be careful if you are using extensions though
* For databases getting in the range of 1TB
	* Can never get enough memory
	* PITR backups are taking a long time
	* I/O throughput becomes much more important
	* do incremental backups: pgBackRest supports this
	* change checkpoint parameters as seen on [thebuild.com](https://thebuild.com)
	* keep shared buffers in 16-32GB range
	* Load balancing is required with read replicas
	* Start off-loading services that really don't need to be in the database
	* **Let vacuum jobs complete**
	* Indexes are getting large, will partial indexes suffice?  If so use them
	* See which indexes are actually used, drop the indexes that aren't being used
	* Time to start partitioning
	* Look into using parallel query execution
	* Use appropriate index types
	* pgupgrade still works fine
* For databases getting in the range of 10TB
	* Anything involving copying is becoming impractical
	* Avoid using tablespaces
	* May want to start doing reindex/replace scripts to fix bloated index - there are queries that you can use to figure out if an index is bloated
	* might want to consider sharding solutions
* For databases that are larger (huge)
	* If it is mostly archival data move it to a data warehouse (archival system)
	* Start using large-scale sharding
