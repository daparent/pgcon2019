# Graph Databases

* Talking about a graph up on screen showing how graphs work
* Abbreviation for a property graph is pg, just like postgres, wonderful
* RDF: triple, subject, predicate, object
* property graph and RDF approaches are not interoperable in terms of their ecosystems
* RDF is used mostly for web, standardized by W3C
* PG is working to be standardized by ISO
* PG serialized in CSV?
* RDF serialized in XML, JSON
* PG logic: closed-world - everything in your database is true, everything that is not is false
* RDF logic: open-world - everything in your database is true, everything that is not is unknown
* logic type has consequences:
	* unique constraint one person can have one biological father
	* bob is father of alice
	* then robert is father of alice comes in
	* in closed-world you would get a violation
	* in open-world you would get a collision however you can conclude that Bob is also Robert
* SPARQL is the RDF query language - de facto standard?  Sounds like it is usually used
* uses an ontology, the query is "interesting"
* Cypher most popular property graph query language
	* query a graph get back a table
* PGQL poperty graph query language developed by Oracle
	* more like SQL, still unique but looks more like SQL
* G-CORE graph query research language by LDBC
	* abstract query language to specify benchmarks that can then be translated into other query languages?
* The GQL Manifesto: GQL = Cypher + PGQL + G-CORE
* SQL/PGQ will be new in SQL:202x part 16
