# DataBase 比较
## Cassandra vs. MongoDB vs. MySQL vs. PostgreSQL vs. TiDB
 
[db-engines.com](https://db-engines.com/en/system/Cassandra;PostgreSQL;MongoDB;MySQL;TiDB)




|Name|Cassandra  |MongoDB  |MySQL  |PostgreSQL |TiDB  |
|--------|--------|--------|--------|--------|--------|
|Description|Wide-column store based on ideas of BigTable and DynamoDB<br>->Optimized for write access|One of the most popular document stores|Widely used open source [RDBMS](https://db-engines.com/en/article/RDBMS)|Widely used open source [RDBMS](https://db-engines.com/en/article/RDBMS)<br>->Developed as objectoriented DBMS (Postgres), gradually enhanced with 'standards' like SQL|A horizontally scalable [RDBMS](https://db-engines.com/en/article/RDBMS) backed by transactional [ Key-Value Stores](https://db-engines.com/en/article/Key-value+Stores) and compatible with the [MySQL](https://db-engines.com/en/system/MySQL) protocol|
|Primary database model|[Wide column store](https://db-engines.com/en/article/Wide+Column+Stores)|[Document store](https://db-engines.com/en/article/Document+Stores)|[Relational DBMS](https://db-engines.com/en/article/RDBMS)<br>->Key/Value like access via memcached API|[Relational DBMS](https://db-engines.com/en/article/RDBMS)<br>->with object oriented extensions, e.g.: user defined types/functions and inheritance. Handling of key/value pairs with hstore module.|[Relational DBMS](https://db-engines.com/en/article/RDBMS)|
|Additional database models|||[Document store](https://db-engines.com/en/article/Document+Stores)[Key-value store](https://db-engines.com/en/article/Key-value+Stores)|[Document store](https://db-engines.com/en/article/Document+Stores)[Key-value store](https://db-engines.com/en/article/Key-value+Stores)|[Key-value store](https://db-engines.com/en/article/Key-value+Stores)|
|Website|[cassandra.apache.org](http://cassandra.apache.org/) |[www.mongodb.com](https://www.mongodb.com/) |[www.mysql.com](https://www.mysql.com/) |[www.postgresql.org](https://www.postgresql.org/) |[pingcap.com](https://pingcap.com/) |
|Technical documentation|[docs.datastax.com](http://docs.datastax.com/) |[docs.mongodb.com/­manual](https://docs.mongodb.com/manual/) |[dev.mysql.com/­doc](https://dev.mysql.com/doc/) |[www.postgresql.org/­docs/­manuals](https://www.postgresql.org/docs/manuals/) |[pingcap.com/­docs](https://pingcap.com/docs/) |
|Developer|Apache Software Foundation <br>->Apache top level project, originally developped by Facebook|MongoDB, Inc|Oracle <br>->since 2010, originally MySQL AB, then Sun|PostgreSQL Global Development Group <br>->[www.postgresql.org/­developer](https://www.postgresql.org/developer/) |PingCAP|
|Initial release|2008|2009|1995|1989 <br>->1989: Postgres, 1996: PostgreSQL||
|Current release|3.11.1, October 2017|3.6.1, December 2017|5.7.20, October 2017|10.1, November 2017||
|License <br>->Commercial or Open Source|Open Source <br>->Apache version 2|Open Source <br>->AGPL version 3, commercial license available|Open Source <br>->GPL version 2. Commercial licenses with extended functionallity are available|Open Source <br>->BSD|Open Source <br>->Apache 2.0|
|Cloud-based <br>->Only available as a cloud service|no|no|no|no|no|
|Implementation language|Java|C++|C and C++|C|Go|
|Server operating systems|BSDLinuxOS XWindows|LinuxOS XSolarisWindows|FreeBSDLinuxOS XSolarisWindows|FreeBSDHP-UXLinuxNetBSDOpenBSDOS XSolarisUnixWindows|Linux|
|Data scheme|schema-free|schema-free <br>->Although schema-free, documents of the same collection often follow the same structure|yes|yes||
|Typing <br>->predefined data types such as float or date|yes|yes <br>->string, integer, double, boolean, date, object_id|yes|yes||
|XML support |||yes|yes <br>->specific XML-type available, but no XML query functionality.||
|Secondary indexes|restricted <br>->only equality queries, not always the best performing solution|yes|yes|yes||
|SQL <br>->Support of SQL|SQL-like DML and DDL statements (CQL)|no|yes <br>->with proprietary extensions|yes <br>->standard with numerous extensions||
|APIs and other access methods|Proprietary protocol <br>->CQL,Thrift|proprietary protocol using JSON|ADO.NETJDBCODBC|native C  librarystreaming API for large objectsADO.NETJDBCODBC||
|Supported programming languages|C#,C++,Clojure,Erlang,<br>Go,Haskell,Java,<br>JavaScript,Node.js,Perl,PHP,<br>Python,Ruby,Scala|Actionscript,C,C#,C++,<br>Clojure,ColdFusion,D,Dart,Delphi,<br>Erlang,Go, Groovy, Haskell,Java,<br>JavaScript,Lisp, Lua,<br> MatLab ,Perl,PHP,PowerShell,<br> Prolog, Python,R,<br>Ruby,Scala,Smalltalk |Ada,C,C#,C++,<br>D,Delphi,Eiffel,Erlang,<br>Haskell,Java,JavaScript(Node.js),<br>Objective-C,OCaml,<br>Perl,PHP,Python,<br>Ruby,Scheme,Tcl|.Net,C,C++,Delphi,Java,<br>Perl,PHP,Python,Tcl||
|Server-side scripts <br>->Stored procedures|no|JavaScript|yes <br>->proprietary syntax|user defined functions <br>->realized in proprietary language PL/pgSQL or with common languages like Perl, Python, Tcl etc.||
|Triggers|yes|no|yes|yes||
|Partitioning methods <br>->Methods for storing different data on different nodes|Sharding <br>->no "single point of failure"|Sharding|horizontal partitioning, sharding with MySQL Cluster or MySQL Fabric|declarative partitioning (by range or by list) since PostgreSQL 10.0 <br>->prior to 10.0, partitioning could be realized with table inheritance||
|Replication methods <br>->Methods for redundantly storing data on multiple nodes|selectable replication factor <br>->Representation of geographical distribution of servers is possible|Master-slave replication|Master-master replicationMaster-slave replication|Master-slave replication <br>->other methods possible by using 3rd party extensions||
|MapReduce <br>->Offers an API for user-defined Map/Reduce methods|yes|yes|no|no||
|Consistency concepts <br>->Methods to ensure consistency in a distributed system|Eventual ConsistencyImmediate Consistency <br>->can be individually decided for each write operation|Eventual ConsistencyImmediate Consistency <br>->can be individually decided for each write operation|Immediate Consistency|Immediate Consistency||
|Foreign keys <br>->Referential integrity|no|no <br>->typically not used, however similar functionality with DBRef possible|yes <br>->not for MyISAM storage engine|yes||
|Transaction concepts <br>->Support to ensure data integrity after non-atomic manipulations of data|no <br>->Atomicity and isolation are supported for single operations|no <br>->atomic operations within a single document possible|ACID <br>->not for MyISAM storage engine|ACID||
|Concurrency <br>->Support for concurrent manipulation of data|yes|yes|yes <br>->table locks or row locks depending on storage engine|yes||
|Durability <br>->Support for making data persistent|yes|yes <br>->optional|yes|yes||
|In-memory capabilities <br>->Is there an option to define some or all structures to be held in-memory only.||yes <br>->In-memory storage engine introduced with MongoDB version 3.2|yes|no||
|User concepts <br>->Access control|Access rights for users can be defined per object|Access rights for users and roles|Users with fine-grained authorization concept <br>->no user groups or roles|fine grained access rights according to SQL-standard||
