# Testing

* two modules: spring-boot-test and spring-boot-test-autoconfigure
  * on top: spring-boot-starter-test
    * JUnit, AssertJ, Hamcrest, Mockito, JSONAssert, JsonPath
* @SpringBootTest: alternative to @ContextConfiguration
* use "webEnvironment" property of @SpringBootTest to control how tests will run
  * MOCK: use with @AutoConfigureMockMvc
  * RANDOM\_PORT: embedded servlet container \(i.e., real servlet environment\)
    * easily inject the actual port in the tests via @LocalServerPort
    * @TestRestTemplate also knows the actual port and will resolve relative links to the running server
  * DEFINED\_PORT: idem \(default port: 8080\)
  * NONE: loads an application context using SpringApplication, but no servlet environment is provided
* if a test is @Transactional, it will rollback the transaction at the end of each test method by default
  * DOES NOT WORK with RANDOM or DEFINED port because in that case HTTP client and server will run in separate threads, thus separate transactions
* don't forget @RunWith\(SpringRunner.class\)

Mocking and spying:

* @MockBean: define a Mockito bean inside your application context
  * can be used to add new beans or replace a single bean definition
  * can be used on test classes, on fields within tests or on @Configuration classes and fields
  * when used on a field, the created mock will also be injected
* mocks are automatically reset after each test
  * automatically enabled if using @SpringBootTest
    * if different context: enable via @TestExecutionListeners\(MockitoTestExecutionListener.class\)
* use @SpyBean to wrap any existing bean with a Mockito spy

Automatically configured tests

* to test slices of the application, spring-boot-test-autoconfigure includes a number of annotations that can be used to configure the "slides". Each of them works similarly, providing a @...Test annotation that loads the ApplicationContext and one or more @AutoConfigure... annotations that can be used to customize auto-configuration settings
  * each slice loads a very restricted set of auto-configuration classes; can exclude some via the excludeAutoConfiguration attribute, or use @ImportAutoConfiguration\#exclude
* also possible to use @AutoConfigure... annotations with the standard @SpringBootTest annotation \(useful to have auto-configured test beans but no "slicing"\)
* auto-configured JSON tests
  * test JSON &lt;-&gt; object serialization/deserialization
  * @JsonTest: auto configures ObjectMapper, any @JsonComponent and any Jackson modules
    * also configures Gson if on the classpath
  * to customize: @AutoConfigureJsonTesters
  * Utils
    * AssertJHelpers
    * JacksonTester
    * GsonTester
    * BasicJsonTester
    * ...
* auto-configured Spring Mvc tests
  * ...
* auto-configured Spring Data JPA tests
  * @DataJpaTest
    * default: configures an in-memory embedded database
    * scans for @Entity classes
    * configures Spring Data JPA repositories
    * regular @Component beans are NOT loaded in the application contexts
  * DataJpaTest are transactional and rollback at the end of each test
    * transaction management can be disabled for a test or for the whole class
      * @Transactional\(propagation=Propagation.NOT\_SUPPORTED\)
  * can inject a TestEntityManager
  * can also use @AutoConfigureTestEntityManager
  * also, a JdbcTemplate is available
* auto-configured JDBC tests
  * @JdbcTest
    * also embedded DB and JdbcTemplate
* auto-configured jOOQ tests
  * ...
* auto-configured Data MongoDB tests
  * @DataMongoTest
    * in-memory MongoDB
    * MongoTemplate
    * scan for @Document classes
    * configures Spring Data MongoDB repositories
* auto-configured Data Neo4j tests
  * ...
* auto-configured Data Redis tests
  * @DataRedisTest
    * scan for @RedisHash classes
    * configures Spring Data Redis repositories
    * regular @Component beans will not be loaded in the ApplicationContext
* auto-configured Data LDAP tests
  * @DataLdapTest
    * configures an in-memory embedded LDAP \(if available\)
    * LdapTemplate scan for @Entry classes
    * configures Spring Data LDAP repositories
    * regular @Component beans will not be loaded in the ApplicationContext
* auto-configured REST clients
  * @RestClientTest
    * automatically configures Jackson and GSON support
    * RestTemplateBuilder
    * support for MockRestServiceServer
* auto-configured Spring REST Docs tests
  * ...
* auto-configured Spring REST Docs tests with Mock MVC
  * ...
* auto-configured Spring Rest Docs tests with REST Assured
  * ...

Spock support:

* add a dependency on Spock's spock-spring module
  * integrates Spring's test framework into Spock \(cfr: http://spockframework.org/spock/docs/1.1/modules.html\)

Test utilities

* ConfigFileApplicationContextInitializer
  * can be applied to tests to load application.properties files
  * useful when other features of @SpringBootTest are not useful
* EnvironmentTestUtils
  * add properties to ConfigurableEnvironment or ConfigurableApplicationContext
* OutputCapture
  * JUnit rule to capture System.out and System.err
* TestRestTemplate
  * useful in integration tests
  * not throwing for server-side errors
  * with HttpClient
    * redirects not followed to be able to check response location
    * cookies ignored to keep a stateless template



