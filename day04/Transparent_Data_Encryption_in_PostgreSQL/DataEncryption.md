# Transparent Data Encryption in PostgreSQL

- SSL an protect data on the network
- Data at rest Encryption protects it on the drive
  - Usually transparent to users
  - Full disk encryption
    - Platform dependent
    - Logged-in users have access to decrypted data
  - Postgres has to manage the encryption to be able to protect from OS-users
- pgcrypto
  - Very convienient
  - Not transparent to users
  - Requires code to be modified
  - Performance issues

## Proposal
- Per tablespace encryption
  - Encrypt all the things related to the tablespace (index, TOAST table, WAL, catolog, ...)
- Still under discussion on pgsql-hackers

## Buffer level encryption
- Data is encrypted/decrypted at the shared buffer boundary
- Performance is a little slower than vanilla PG but faster than using pgcrypto




# Usage for us
- Not much gains, unless storing the DB on the isilon, as purestorage is already encrypting at rest automatically
  - Chances of disks getting stolen is very low, so even on the isilon gains would be very small
- When running backups we would ideally need to encrypt the data before sending to tape
  - Using this would probably avoid the need of encrypting for backups
- Probably more trouble than what we will gain
- Doest not protect against malicious sys-admin, sql injection, or anything other than disks being stolen
  - Adds a layer of obscurity for a malicious sys-admin as encrytion keys are loaded in memory

