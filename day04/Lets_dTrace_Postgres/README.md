# Let's (D)Trace Postgres
- D script, derivative of C
- Gather stats on function call
- Trace any function or syscall with probes
- Can put conditions (predicates) on probes
- Use D script for the action to be done
- flamegraph can show time spent in each function, perl script in github
  - need to adjust
    - stackframes
- Able to read C structures into variables in the D scripts

## Call of Cthulhu
- Pending list max size should be smaller than max work mem
  - Postgres has to prepare the data in smaller chunks and it slows down
- `pg_stat_activity` can show pids of worker process
  - Can use DTrace to pull execution information of the PID from the kernel
- DTrace is very flexible
  - able to hook into specific place inside functions and read CPU register content

## Solutions
- Increase pending list max size
- Disable post-insert merging of the pending list
