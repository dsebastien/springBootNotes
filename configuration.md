# Configuration

## General

* start with --debug to see all automatic configuration classes
  * those classes make the Spring Boot magic happen
* @Import to load additional configuration classes
* @EnableAutoConfiguration\(exclude =... .class\): to disable some automatic configuration classes
* SpringApplication app = new SpringApplication...
  * or SpringApplicationBuilder

## Externalized

### Possibilites & loading/override order:

* .properties or .yml
  * application.properties\|yml
* inject values via @Value
* ```
  @Value("${name}")
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

  * application-default.properties \(_default _active profile\)

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

## Profiles

* defining active profiles
  * through spring.profiles.active

## Banner

* Spring banner customizable
* banner.txt file or banner.location or code \(SpringApplication app = new ...; app.setBanner ...
* variables can be used within
  * can be defined through MANIFEST.MF \(e.g., application name with Maven filtering applied to the manifest\)



