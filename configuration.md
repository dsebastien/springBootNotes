# Configuration

## General

* start with --debug to see all automatic configuration classes
  * those classes make the Spring Boot magic happen
* @Import to load additional configuration classes
* @EnableAutoConfiguration\(exclude =... .class\): to disable some automatic configuration classes
* SpringApplication app = new SpringApplication...
  * or SpringApplicationBuilder

## Externalized

### Possibilites, injection & loading/override order:

* .properties or .yml
  * application.properties\|yml
* inject values via @Value
* ```
  @Value("${name}") // no relaxed binding here, the names MUST match
  private String name;
  ```

  for tests

  * @TestPropertySource

  * @SpringBootTest\#properties

* arguments

  * inject ApplicationArguments bean
  * CommandLinePropertySource takes care of catching those

* inline json providing properties, passed through an environment variable or system property:

* ```
  SPRING_APPLICATION_JSON
  ...
  java -Dspring.application.json='{"name": "Seb" }'
  java -jar ... --spring.application.json ...
  ```

  RandomValuePropertySource

* ```
  my.secret=${random.uuid}
  ```

  profile specific configuration file

  * application-{profile}.properties\|yml

  * application-default.properties \(\_default \_active profile\)

* @PropertySource on an @Configuration class

### Using placeholders in properties

Possible to use previously defined keys:

```
app.name=My app
app.foo=${app.name} is a foo app!
```

### YAML config

* YamlPropertiesFactoryBean
* YamlMapFactoryBean

Example with nesting:

```
environments:
    dev:
        url: ...
        name: ...
    prod:
        url: ...
        name: ...
    ...
...

Ends up:
environments.dev.url=...
...
```

Example with multiple profiles:

```
server:
    url: ...localhost
...
spring:
    profiles: development
server:
    url: ...dev
...
spring:
    profiles: production
server:
    url: ...pro
...
```

To extract a subset of an application.yml file:

```
@Component
@ConfigurationProperties(prefix="foo")
public class FooProperties { ... }
...
application.yml:

foo:
    url: ...
    ...
...
```

With the above, those properties can then easily be injected:

```
@Service
public class MyService {
    @Autowired
    public MyService(FooProperties ...){ ... }
}
```

## @ConfigurationProperties

* can be used to annotate a class
* can be used to annotate an @Bean method
  * useful to bind properties to third-party components outside of your control
* Spring Boot uses relaxed rules for binding Environment properties to @ConfigurationProperties, no need to have an exact match
  * person.firstName
  * person.first-name: recommended in .properties and .yml
  * person.first\_name: recommended in .properties and .yml
  * PERSON\_FIRSTNAME: recommended when using system environment variables
* properties will be coerced to the right type when bound to the @ConfigurationProperties
  * can be customized via a ConversionService bean \(bean id: conversionService\) 
  * can be customized via custom property editors or custom converters
  * required early, so shouldn't have too many dependencies
* @ConfigurationProperties will be validated if annotated with Spring's @Validated. JSR-303 \(javax.validation\) constraints can then be used, as long as an implementation is on the classpath
  * to validate nested properties, use @Valid on the nested property to trigger its validation
  * possible to write custom validator: ConfigurationPropertiesValidator

## @Value &lt;-&gt; @ConfigurationProperties

* @Value
  * no relaxed binding
  * no metadata support
  * SpEL evaluation supported
* @ConfigurationProperties
  * relaxed binding
  * metadata support
  * no SpEL evaluation

## POJO for configuration

Spring recommends to regroup configuration keys in a POJO with @ConfigurationProperties:

* centralized
* visible
* easy to validate
* easy to inject
* ...

## Profiles

Active profiles can be defined via

* config file \(e.g., application.properties\|yml\)
  * spring.profiles.active=dev,bla
* command line switch: --spring.profiles.active=dev,bla
  * overrides value if defined in application.properties\|yml
* programmatically
  * SpringApplication.setAdditionalProfiles\(...\)
  * ConfigurableEnvironment

## Banner

* Spring banner customizable
* banner.txt file or banner.location or code \(SpringApplication app = new ...; app.setBanner ...
* variables can be used within
  * can be defined through MANIFEST.MF \(e.g., application name with Maven filtering applied to the manifest\)

## Properties list

https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/\#appendix



