# Comprehensive PostgreSQL Interview Question Bank

This extensive collection provides **practical, technical** PostgreSQL interview questions—covering core SQL, administration, performance tuning, high availability, extensions, procedural languages (PL/pgSQL, PL/Python), JSON/JSONB, geospatial, replication, security, observability, and microservices integration. Philosophical or meta-level items are omitted.  

## 1. SQL Fundamentals (1–30)  
1. What is PostgreSQL and how does it differ from other RDBMSs?  
2. Explain ACID properties in PostgreSQL.  
3. Describe table creation syntax and options (WITH, INHERITS).  
4. How do you query data: `SELECT`, `WHERE`, `GROUP BY`, `HAVING`, `ORDER BY`.  
5. What are the set operations: `UNION`, `INTERSECT`, `EXCEPT`?  
6. Explain `JOIN` types: INNER, LEFT/RIGHT/FULL OUTER, CROSS.  
7. How do you use window functions: `ROW_NUMBER()`, `RANK()`, `SUM() OVER()`.  
8. What are Common Table Expressions (CTEs) and recursive queries?  
9. How do you perform pagination efficiently?  
10. Explain `LIMIT`/`OFFSET` and keyset pagination.  
11. How do you enforce constraints: `NOT NULL`, `UNIQUE`, `PRIMARY KEY`, `FOREIGN KEY`, `CHECK`, `EXCLUDE`?  
12. Describe `SERIAL`, `BIGSERIAL`, identity columns.  
13. Explain `ENUM` types and user-defined domains.  
14. How do you use arrays and composite types?  
15. What are range types and their operators?  
16. Describe full-text search: `tsvector`, `tsquery`, `GIN` indexes.  
17. How do you work with JSON (`JSON`, `JSONB`), functions and operators?  
18. Explain lateral joins (`LATERAL`).  
19. How do you perform upserts with `INSERT … ON CONFLICT DO UPDATE`?  
20. How do you implement partitioning (range, list, hash)?  
21. What are materialized views and when to use them?  
22. How do you manage views: `CREATE VIEW`, `REFRESH MATERIALIZED VIEW`.  
23. Explain transactions, isolation levels, and `SERIALIZABLE` vs. `READ COMMITTED`.  
24. What are advisory locks and how are they used?  
25. How do you detect and resolve deadlocks?  
26. Explain `EXPLAIN` and `EXPLAIN ANALYZE`.  
27. Describe the write-ahead log (WAL) and its role.  
28. How do you back up and restore: `pg_dump`, `pg_dumpall`, `pg_restore`, base backups?  
29. What is point-in-time recovery (PITR)?  
30. How do you import/export data: `COPY` vs. `\copy`?

## 2. Indexing & Performance (31–70)  
31. Types of indexes: B-tree, Hash, GiST, GIN, SP-GiST, BRIN.  
32. When to use partial, expression, and covering indexes.  
33. Explain index-only scans and multicolumn indexes.  
34. How do you monitor index usage and bloat?  
35. How do you cluster a table on an index (`CLUSTER`)?  
36. Describe VACUUM (plain vs. full) and `autovacuum`.  
37. Explain `ANALYZE` and planner statistics.  
38. How do you tune `effective_cache_size`, `shared_buffers`, `work_mem`, `maintenance_work_mem`?  
39. What is `checkpoint_timeout` and `max_wal_size` tuning?  
40. Explain `pg_stat_statements` for query tracking.  
41. How to identify slow queries via logs and `auto_explain`.  
42. How do you configure log rotation and log levels?  
43. Explain connection pooling strategies: PgBouncer vs. Pgpool-II.  
44. How do you use `EXPLAIN` to understand join order and cost estimates?  
45. What is `jit` compilation in PostgreSQL 11+?  
46. How do you leverage parallel query execution?  
47. Explain tuning for OLTP vs. OLAP workloads.  
48. How to minimize VACUUM impact on long-running transactions?  
49. What are table bloat and index bloat, and how to reclaim space?  
50. How do you set up and monitor autovacuum with `pg_stat_all_tables`?  
51. Explain `max_parallel_workers_per_gather` and `parallel_tuple_cost`.  
52. How do you use `EXPLAIN (BUFFERS)` to inspect IO?  
53. Explain `max_wal_size` vs. `min_wal_size` interplay.  
54. How do you handle hotspots in heavily updated tables?  
55. What is `UNLOGGED` tables and when to use them?  
56. How to compress large objects with TOAST settings.  
57. Explain caching strategies: `pg_prewarm`.  
58. How to benchmark with `pgbench`.  
59. Explain table sampling with `TABLESAMPLE BERNOULLI`.  
60. How do you set up foreign data wrappers (FDW) with `postgres_fdw`?

## 3. High Availability & Scalability (71–110)  
71. Explain streaming replication and WAL shipping.  
72. How to configure primary and standby servers (`primary_conninfo`).  
73. What is synchronous vs. asynchronous replication?  
74. How do you set up logical replication and `REPLICATION SLOT`s?  
75. How to use `pglogical` extension for logical replication.  
76. Explain failover and switchover procedures with `pg_ctl` or `repmgr`.  
77. How to implement connection routing with `auto_failover` or `Patroni`.  
78. Explain hot, warm, and cold standbys.  
79. How to use `pg_basebackup` for base backups.  
80. What is cascading replication?  
81. Explain split-brain and quorum management in HA clusters.  
82. How to configure load balancing across replicas (PgBouncer, HAProxy).  
83. Explain BDR (bi-directional replication).  
84. How to perform rolling upgrades with minimal downtime?  
85. What is Partitioned-btrees for shard performance?  
86. How to scale reads vs. writes in Postgres?  
87. Explain Citus extension for horizontal scaling.  
88. How to shard tables manually via schema design?  
89. Explain FDW for federated queries across multiple servers.  
90. How to use `pg_dist_node` and `pg_dist_shard`.  
91. Describe using cloud-native patterns: AWS RDS Multi-AZ, GCP Cloud SQL HA.  
92. Explain low-latency replication for geo-distributed setups.  
93. How to handle global data consistency in distributed Postgres.  
94. What is `bdr` vs. `pglogical` trade-off?  
95. How to use Patroni for orchestrating HA clusters.  
96. Explain RAFT consensus in etcd for Patroni.  
97. How do you perform split-mirror backups with streaming replication?  
98. What is `pg_rewind` for failover recovery?  
99. How to monitor replica lag via `pg_stat_replication`.  
100. Explain pgBackRest vs. Barman for backup orchestration.  
101. How to implement low-cost cross-region replication.  
102. Explain cross-data center replication strategies.  
103. How to automate failover tests with `pg_auto_failover`.  
104. How to use Kubernetes operators (CrunchyData PG, Zalando Postgres).  
105. Explain sidecar pattern for Postgres containerized HA.  
106. How to configure Patroni with Consul or Kubernetes API as DCS.  
107. How to use `etcd` or ZooKeeper for HA coordination.  
108. Explain advanced sanity checks pre-failover (timeline, WAL archives).  
109. How to implement read-only routing at application layer.  
110. Best practices for maintenance windows and failover drills.

## 4. Security & Access Control (111–140)  
111. Explain authentication methods: MD5, SCRAM-SHA-256, Kerberos, LDAP, PAM.  
112. How to configure `pg_hba.conf` for host-based authentication rules.  
113. Explain role-based access control: `CREATE ROLE`, `GRANT`/`REVOKE`.  
114. How to enforce row-level security (RLS) and policies.  
115. Explain `SETOF` privileges and `SECURITY DEFINER`.  
116. How to encrypt data at rest with TDE (third-party) or file-system encryption.  
117. How to enable SSL/TLS for client-server communication.  
118. Explain connection encryption parameters (`sslmode`, `sslcert`, `sslkey`).  
119. How to manage certificates via `pgcrypto` and `openssl`.  
120. How to use `pgcrypto` for column-level encryption and hashing.  
121. Explain dynamic data masking patterns.  
122. How to audit activity with `pgaudit` extension.  
123. How to log all DDL/DML via `log_statement` settings.  
124. Explain `ALTER SYSTEM SET log_statement = 'all'`.  
125. How to implement password rotation policies.  
126. How to secure superuser access and limit `postgres` role.  
127. How to integrate with Vault for secret management.  
128. Explain `SECMEM` for sensitive memory scrubbing.  
129. How to use row-level encryption via `pgcrypto` and check constraints.  
130. Best practices for `GRANT EXECUTE` on functions.  
131. How to enforce JSON field encryption with custom UDFs.  
132. How to secure FDWs against SQL injection.  
133. Explain security impact of database extensions.  
134. How to sandbox untrusted code with `untrusted procedural languages`.  
135. How to use `publications` ACL for logical replication security.  
136. How to implement time-based access windows (cron + RLS).  
137. How to secure backups: encryption and restricted access.  
138. How to rotate and revoke TLS certificates seamlessly.  
139. Explain the impact of `row_security = 'on'`.  
140. How to monitor security events and integrate with SIEM.

## 5. Extensions & Procedural Languages (141–180)  
141. How to install and manage extensions (`CREATE EXTENSION`, `ALTER EXTENSION`).  
142. Common extensions: `pg_trgm`, `citext`, `hstore`, `ltree`, `uuid-ossp`.  
143. Explain PostGIS for geospatial queries and indexing.  
144. How to use `pg_stat_statements` and `auto_explain` extensions.  
145. What is `pg_partman` for automated partition management?  
146. Explain `pgcron` for in-database scheduling.  
147. How to write functions in PL/pgSQL: syntax and control flow.  
148. How to handle exceptions and RAISE within PL/pgSQL.  
149. How to use PL/Python, PL/Perl, PL/Tcl for custom logic.  
150. Explain untrusted vs. trusted procedural languages.  
151. How to use triggers (`BEFORE`, `AFTER`, `INSTEAD OF`) in PL/pgSQL.  
152. How to write event-trigger functions for DDL auditing.  
153. Explain `pg_hint_plan` for optimizer hints.  
154. How to use `pg_repack` for online table reorganization.  
155. Explain materialized path vs. nested set for hierarchical data.  
156. How to implement full-text search dictionary and parser functions.  
157. Explain `tablefunc` extension for crosstab queries.  
158. How to use `pg_visibility` for vacuum analysis.  
159. Explain `pg_squeeze` for small table clustering.  
160. How to write custom index access methods with C.  
161. Explain `plv8` JavaScript procedural language.  
162. How to manage extension version upgrades.  
163. How to sandbox untrusted code via chroot or Kubernetes.  
164. Explain `pglogical` for logical replication.  
165. How to use `pg_timetable` for advanced job scheduling.  
166. Explain `timescaledb` for time-series data.  
167. How to use Citus for distributed Postgres sharding.  
168. Explain creating foreign tables with FDW (Oracle_fdw, file_fdw).  
169. How to handle upgrade-in-place for large extensions.  
170. Explain monitoring extensions: `pgmetrics`, `pgbadger`.  
171. How to implement schema migrations with `sqitch` or `Flyway`.  
172. Explain CITEXT extension for case-insensitive text.  
173. How to handle ephemeral data with UNLOGGED tables.  
174. Explain stating replication slots for physical and logical slots.  
175. How to troubleshoot extension failures during `initdb`.  
176. Explain `pg_xlogdump` vs. `pg_waldump` differences.  
177. How to write UDFs in C for performance-critical operations.  
178. Explain `pg_qualstats` for predicate statistics.  
179. How to extend Postgres with new data types in C.  
180. Explain `pg_visibility` and `pg_freespacemap` internals.

## 6. Observability & Maintenance (181–210)  
181. How to monitor `pg_stat_activity`, `pg_stat_database`.  
182. Explain `pg_stat_replication`, `pg_stat_wal_receiver`.  
183. How to use `pg_stat_io` for disk I/O metrics.  
184. Explain `pg_stat_user_tables` and `pg_statio_user_tables`.  
185. How to alert on long-running queries via `pg_cron`.  
186. Explain autovacuum tuning with `vacuum_cost_delay`.  
187. How to track locking conflicts via `pg_locks`.  
188. Explain using `adminpack` for in-database file access.  
189. How to force a checkpoint manually.  
190. Explain logical and physical replication monitoring.  
191. How to inspect `pg_replication_slots` usage.  
192. Explain configuring `archive_command` for WAL archiving.  
193. How to automate failover testing via `repmgr`.  
194. Explain rolling upgrades with `pg_upgrade`.  
195. How to plan major version upgrades for minimal downtime.  
196. Explain `pg_checkpointer` and `bgwriter` processes.  
197. How to use `pg_isready` for readiness checks.  
198. Explain `pg_basebackup` options for parallel backups.  
199. How to run `pg_rewind` after divergent history.  
200. Explain tracing internals with `auto_explain.log_analyze`.

## 7. Microservices Integration (211–240)  
211. How to use PostgreSQL as a microservice datastore.  
212. Explain sidecar pattern for connection pooling in Kubernetes.  
213. How to use `postgres_fdw` for cross-service joins.  
214. Explain using `LISTEN`/`NOTIFY` for event bus.  
215. How to integrate with Debezium for CDC (change data capture).  
216. Explain transactional outbox pattern in Postgres.  
217. How to implement saga orchestration via messages in Postgres.  
218. Explain using `pg_notify` vs. external message brokers.  
219. How to optimize PostgreSQL for micro VM environments.  
220. Explain multi-tenant schemas vs. row-level tenancy.  
221. How to implement tenant isolation via RLS policies.  
222. Explain connection multiplexing via PgBouncer in microservices.  
223. How to handle distributed transactions via two-phase commit.  
224. Explain using XA transactions with Postgres connectors.  
225. How to integrate Postgres with Spring Boot microservices.  
226. Explain using Sequelize or Knex.js with Postgres in Node.js.  
227. How to integrate Postgres in Go microservices using `pgx`.  
228. Explain Phoenix-style HTTP+PubSub with Postgres triggers.  
229. How to use Postgres in serverless functions (AWS Lambda).  
230. Explain performance tuning for multi-tenant microservices.  
231. How to implement blue/green schema migrations.  
232. Explain shadow tables for zero-downtime schema changes.  
233. How to manage migrations with Flyway in CI/CD.  
234. Explain `pglogical` for zero-downtime logical replication.  
235. How to handle schema drift detection in microservices.  
236. Explain write amplification vs. read amplification in shared clusters.  
237. How to integrate with container orchestrators via StatefulSets.  
238. Explain using `initContainers` for bootstrapping Postgres.  
239. How to implement TTL via `pg_cron` for soft deletes.  
240. Explain service mesh integration (Istio mTLS) for secure Postgres access.


[1] https://www.crsinfosolutions.com/postgresql-interview-questions/
[2] https://www.tessell.com/blogs/postgresql-concepts-benefits-and-use-cases
[3] https://en.wikipedia.org/wiki/PostgreSQL
[4] https://www.datacamp.com/blog/top-postgresql-interview-questions-for-all-levels
[5] https://www.geeksforgeeks.org/postgresql/postgresql-tutorial/
[6] https://cto.ai/blog/postgresql-features-and-architecture/
[7] https://www.linkedin.com/pulse/postgresql-interview-questions-answers-qaisar-abbas-4hcrf
[8] https://www.postgresql.org/docs/7.1/advanced.html
[9] https://cloud.google.com/sql/docs/postgres/features
[10] https://www.geeksforgeeks.org/postgresql/postgresql-interview-questions/
[11] https://www.codecademy.com/learn/fscp-advanced-postgresql/modules/fscp-postgres-database-performance/cheatsheet
[12] https://www.quest.com/learn/what-is-postgresql.aspx
[13] https://www.youtube.com/watch?v=Y1ZU1NU7wT8
[14] https://bytescout.com/blog/postgresql-advanced-queries.html
[15] https://www.postgresql.org/about/featurematrix/
[16] https://www.hirist.tech/blog/top-25-postgresql-interview-questions-and-answers/
[17] https://www.squash.io/postgresql-tutorial-series-from-basics-to-advanced-concepts/
[18] https://www.postgresql.org/about/
[19] https://learnomate.org/postgresql-interview-questions-guide/
[20] https://www.postgresql.org/docs/current/tutorial-advanced.html