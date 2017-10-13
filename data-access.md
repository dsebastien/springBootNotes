# Data access

## Entities

* @EntityScan: used by Spring Boot to detect entities \(e.g., JPA entities\)

## Embedded DB support

* H2, HSQL DB, Derby
* no need to provide any connection URLs, simply include a build dependency to the embedded database that you want to use
* the same in-memory database is reused across the whole test suite regardless of the number of application contexts that you use
  * to get a separate in-memory DB for each context: spring.datasource.generate-unique-name=true
* need dependency on Spring JDBC for an embedded db to be auto-configured
  * comes transitively with spring-boot-starter-data-jpa
* H2 web console
  * browser-based console that Spring Boot auto configures if
    * Web app
    * h2 on the classpath
    * Spring Boot dev tools used \(if not, add spring.h2.console.enabled=true\)

## Production

Uses database connection pooling of Tomcat by default or uses HikariCP if available

Configuration keys:

* spring.datasource.url
* ...username
* ...password
* ...driver-class-name \(often not needed, auto detection\)
* spring.datasource.tomcat.\*
* spring.datasource.hikari.\*

## JDBC

* JdbcTemplate and NamedParameterJdbcTemplate auto configured and can directly be injected

## JPA

* persistence.xml not needed
  * entities are scanned automatically via @EnableAutoConfiguration or @SpringBootApplication
    * finds all @Entity, @Embeddable and @MappedSuperclass
    * can be customized via @EntityScan
* Spring Data repositories
  * queries are automatically created based on method names
    * for complex scenarios, add @Query
* creating and dropping JPA databases
  * auto create for embedded databases
* configuration: spring.jpa.\* \(e.g, spring.jpa.hibernate.ddl-auto=create-drop
* spring.jpa.open-in-view=false
  * !
* built-in JOOQ support

## Redis

* auto configuration of the Lettuce client library
  * https://lettuce.io/
* spring-boot-starter-data-redis
* connecting: inject an auto configured
  * RedisConnectionFactory
  * StringRedisTemplate
  * RedisTemplate
  * IF commons-pool2 is on the classpath, you'll get a pooled connection factory by default

## MongoDB

* auto configuration of a MongoDbFactory
* tries to connect to mongodb://localhost/test by default
* spring.data.mongo.uri=mongodb://user:secret@mongo1.example.com:12345,mongo2.example.com:23456/testmongodb://user:secret@mongo1.example.com:12345,mongo2.example.com:23456/test

  * replica set

* a custom MongoDbFactory can be declared \(or Mongo bean to take complete control of establishing the MongoDB connection\)

* auto configured MongoTemplate

  * see MongoOperations for details

* Spring Data MongoDB repositories

  * queries are constructed for us automatically based on method names

* auto configured embedded mongo server

  * to use it, add a dependency to de.flapdoodle.embed:de.flapdoodle.embed

  * the port can be customized via spring.data.mongodb.port \(use '0' for a random port\)

  * the MongoClient created by MongoAutoConfiguration will be automatically configured to use the randomly allocated port

* if SLF4J is on the classpath, output produced by Mongo will be routed to a specific logger

## Neo4J

* ...

## SolR

* auto configuration for client library on top of what Spring Data SolR brings
  * spring-boot-starter-data-solr
* connecting: inject the auto configured SolrClient
  * connects to localhost:8983/solr by default
* creation of queries based on method names

## ElasticSearch

* auto configuration on top of Spring Data Elastic Search
* spring-boot-starter-data-elasticsearch
* connecting using Jest
  * if Jest is on the classpath a JestClient will be configured automatically
    * by default it connects to localhost:9200
  * properties
    * spring.elasticsearch.jest.uris=...
    * spring.elasticsearch.jest.read-timeout=...
    * spring.elasticsearch.jest.username=...
* auto configured ElasticSearchTemplate
  * by default, local in-memory server
  * home set to work directory
    * customize via spring.data.elasticsearch.properties.path.home=...
* auto construct queries in repositories based on method names

## Cassandra

* auto configured CassandraTemplate or Session
* spring-boot-starter-data-cassandra
* repositories support is limited: annotate finder methods with @Query

## CouchBase

* ...

## LDAP

* spring-boot-starter-ldap
* properties: LdapTemplate
* automatically configured embedded in-memory server
  * if "schema.ldif" on the classpath: used for initialization

## Distributed transactions

* with JTA: Atomikos or Bitronix
  * if on the classpath: automatically configured JtaTransactionManager
* @Transactional
* Spring Boot ensures that depends-on settings are applied to Spring beans

Atomikos

* spring-boot-starter-jta-atomikos
* Atomikos transaction logs are written to a "transaction-logs" folder in the application work directory \(app home dir\)
  * customize: spring.jta.log-dir
* properties
  * spring.jta.atomikos.properties
    * full list: AtomikosProperties

Bitronix

* spring-boot-starter-jta-bitronix

JMS

* if JTA is used, the primary JMS connection factory bean will be XA aware and participate in distributed transactions.

By default, we inject XaJmsConnectionFactory \(qualifier: xaJmsConnectionFactory\)

To use a non-XA ConnectionFactory for some cases:

```
@Autowired
@Qualifier("nonXaJmsConnectionFactory")
...
```

## Hazelcast

* automatically configured HazelcastInstance
* define a com.hazelcast.config.Config instance if needed
* can also use spring.hazelcast.config.\*



