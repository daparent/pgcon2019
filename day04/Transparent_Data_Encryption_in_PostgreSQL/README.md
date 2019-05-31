# Transparent Data Encryption in PostgreSQL
* Full disk encryption was considered but it does not protect against any user that is logged into the running system since the filesystem is decrypted until the system is shutdown
* What about pgcrypto?
	* Not transparent to users
		* Need to modify SQL, application code
		* Triggers and views help
	* Could be a cause of performance overhead
* Per tablespace encryption in CREATE TABLESPACE syntax
	* CREAT TABLESPACE <tablespace> ... WITH (encryption = on);
* Specified table and indexes, TOAST table and WAL are transparently encrypted
* Encrypts system catalogs and temp files
* Encrypt between shared buffer and the page cache (kernel page cache which is flushed out to disk)
* When loaded into shared buffer it is decrypted
* Encryption with:
	* AES Symmetric key algorithm
	* AES-256
	* Block cypher - 16 bytes block size
	* Using openssl is preferable (--with-openssl)
		* AES-NI
	* Block cipher mode of operation has to be decided (CBC or XTS)
		* Looks like the choice is XTS
* Use 2-Tier Key Hierarchy
	* Master key
		* Stored outside the database
		* Encrypt/decrypt tablespace keys
		* One key per database cluster
	* Tablespace Key (data key)
		* Stored inside the database
		* Encrypt/Decrypt database objects
		* One key per tablespace
		* key size is 32 bytes with AES-256
	* MySQL and Oracle use 2-Tier key hierarchy to provide the same service
* Proposal to integrate Postgres with a key management system
* Postgres will cache the master key in shared memory?
	* Risk of key leakage when memory dumps
		* using madvise (MADV_DONTDUMP) could help with this problem
	* Risk of key leakage when swapped out
		* mlock(2) could help resolve this issue
* Encrypt WAL records when inserting into WAL buffer
* Only encrypt block data and main data in the WAL record, XLogRecord header has an encrypted flag
* Use same encryption key for WAL encryption that isused for the table
* From perf testing by speaker it was noted that there was a maximum of 7% degradation in execution time
