# Advanced Authentication
* peer
	* unix socket based auth that passes through unix user connected
* gss (Kerberos) / sspi (Windows)
	* recommended for Enterprise deployment
* cert (SSL Certificate Based)
	* map CNs to PG usernames in pg_ident.conf
* scram - password-based but strong otherwise
* pam modules are run as postgres user, not as root - this can get ugly fast
* radius with Enterprise RADIUS solutions
* password based - use scram instead if you do this
* presenter is not a fan of (recommends against the use of):
	* md5
	* ldap
	* ident
	* trust
* pg_hba, runs top to bottom, first match used
* pg_ident configuration defines mappings from system user to postgres user
* pg_hba set map=peermap, then have a peermap entry in pg_ident that maps the user appropriately
* lots of info on windows kerberos - wah wah
* Do we need MIT Kerberos?  I know we've considered this in the past to move to NFSv4, given how ldap is a not recommended version
* the /etc/krb5.conf can be complicated to configure
* In PG12:
	* GSSAPI Encryption support exists
		* Only available when using GSS client and server
* Public Key Infrastructure
	* libpq uses the client cert
	* make sure keyUsage is uncommented in opnssl.cnf under v3_ca
	* keyUsage = cRLSign, keyCertSign
	* when connecting with the client using sslmode=verify-full is recommended
