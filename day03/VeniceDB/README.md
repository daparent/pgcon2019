# VeniceDB
* generating about 5TB
* about 200TB per month
* Dashboard is used to determine if a windows update is ready for users
* 6 million queries per day from dashboard pages
* started with SQL Server using a proxy for fanout
* ran for about 1.5 years and couldn't keep going (ran out of steam)
* 2nd gen worked for about 1.5 years
* 3rd gen is currently in production
* Failed because:
	* unable to scale dimensionally - had a hard time to generate cubes (analytic service from MS)
	* unable to deal with data variety and calculation complexity
	* unable to serve concurrent queries
* Started off 3rd gen with kafka queues on Azure Blob Storage and removed it after time and moved to using native Azure Blob Storage
* 3rd gen uses Citus/Postgres cluster
	* Citus was bought by Microsoft
* pretty big cluster - lots of cores, RAM and storage (multi-terabytes of RAM, petabyte of disk) - see slide 6
* 5TB of data per day ingested and deleted
* used JSONB column on the unified schema
* Were impressed with go program that could run non-stop for months at a time - GC seems to work well
* dealing with 800,000,000 device ids - huge amounts of data - however the way queries were performed outliers would bubble to the top.  For example a test machine rebooting 10,000 times a day would bubble up as a warning.
* Used grafana as a visual debugger so that they could watch performance as changes were made
* Warning about auto vacuum - it can sneak up on you if you don't configure it properly
* If auto vacuum can't keep up then the tables will keep growing possibly out of control
* Used pgbouncer for connection pool management - had pg bouncer turn off select queries that were causing a type of denial of service for maintenance jobs
* Would like more fine grained control over the shared_buffers - workarounds were devised, (see Discussions II slide)

