# Data access

## Entities

* @EntityScan: used by Spring Boot to detect entities \(e.g., JPA entities\)
  * 

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





## 



