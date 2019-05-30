# pgBackRest Performance

- can handle very large data sets (10-1000TB of data)
- Parallel/async ops
- Very easy to tune and configure

- Interfaces that can be called by postgres itself (push wal segment to the repo)

- Restore vs Recovery, backrest does restore, Postgres takes care of recovery

- Archive push works in background
- Spawns worker threads for checksum,compression,encryption
- Checksum all the thingsto ensure consistency
- Default to synchronous archive, can be changed to be async for large DBs
- process-max of 4 if reasonable and can keep up with thousands of wal files per hours

- Can be configured by file, environemnt vars or command line options
  - 99.9% configurable in config file
- Backup from standby still needs access to the master
  - With multiple standby it can parralelise across all standby for performance (option is not available yet)
  - Configure with all stanby to be used and master, backrest will figure out what is the stanby and master

- Make restores part of the regular workflow!!
- For perfomance pgbackrest spool folder should be on the same device as postgres pg_wal folder (able to do move instead of copy)

## Restore Feature
- Restore perf are very important 
  - Delta restore
    - Use Checksum to figure out which files have changed
    - Default mode expect an empty dir, need to enable delta restore
  - Parallelism

## High Latency
- process-max can speed xfers on high latency storage systems

## Compression
- Changing level from 6 to 3 will reduce load without affectging compressiont too much ~20%

## The future
- Full Migration to C by the end of 2019
  - Gives easy access to postgres internals to further increase performance




