# Production

* features included to monitor and manage your application when pushed to production
* choose to manage and monitoring using HTTP endpoints or JMX
* auditing, health and metrics gathering can also be applied automatically

## Actuator

* actuator exposes all @ConfigurationProperties beans
  * URL in a Web app: /configprops
  * equivalent JMX endpoint
* Http endpoints only available if Spring MVC-based application
  * NOK with Jersey \(unless Spring MVC is also there: https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/\#howto-use-actuator-with-jersey\)
* enabled via spring-boot-actuator
* actuator built-in endpoints \(custom ones can be added\): https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/\#production-ready-endpoints
* 
## Monitoring

* MBeanServer automatically configured
  * bean id: mbeanServer
  * exposes any beans with JMX annotations
    * @ManagedResource
    * @ManagedAttribute
    * @ManagedOperation
  * see @JmxAutoConfiguration

## Securing endpoints

* by default, all HTTP endpoints are secured
  * only users with the ACTUATOR role may access them
  * enforced through the standard HttpServletRequest.isUserInRole
  * use management.security.roles to customize
  * disable via management.security.enabled=false
  * custom credentials to use when spring security is enabled \(default config\)
    * security.user.name
    * security.user.password
* CORS support
  * * management.endpoints.cors.allowed-origins=...
    * management.endpoints.cors.allowed-methods=...
    * all properties: check CorsEndpointProperties: https://github.com/spring-projects/spring-boot/tree/master/spring-boot-actuator/src/main/java/org/springframework/boot/actuate/autoconfigure/endpoint/infrastructure/CorsEndpointProperties.java

## Endpoints

* control which endpoints are enabled or not
  * endpoints.shutdown.enabled=true \#enables the shutdown endpoint
  * by default, all endpoints except for shutdown are enabled
    * to rather use opt-in, define endpoints.default.enabled=false
* rename endpoints
  * endpoints.beans.id=springbeans
  * .../application/springbeans instead of "beans"
* discovery for endpoints
  * a discovery page is added with links \(hateoas\): /application

* custom endpoints
  * @Bean annotated with @Endpoint
    * then any methods annotated with @ReadOperation or @WriteOperation will be exposed over JMX and also over HTTP in Web apps

## Health endpoints

* exposed by the "health" endpoint
* if unauthenticated: simple status message
* if authenticated, details are provided: https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/\#production-ready-health-access-restrictions
* health information is collected from all HealthIndicator beans \(https://github.com/spring-projects/spring-boot/tree/master/spring-boot-actuator/src/main/java/org/springframework/boot/actuate/health/HealthIndicator.java\)
* possible to define custom ones in addition to the built-in ones
* the final system state is derived by the HealthAggregator \(sorts the statuses from each indicator\)

## Info endpoint

* exposes various information collected from all InfoContributor beans: https://github.com/spring-projects/spring-boot/tree/master/spring-boot-actuator/src/main/java/org/springframework/boot/actuate/info/InfoContributor.java
* automatically configured ones
  * EnvironmentInfoContributor: expose any keys from the Environment under the info key
    * https://github.com/spring-projects/spring-boot/tree/master/spring-boot-actuator/src/main/java/org/springframework/boot/actuate/info/EnvironmentInfoContributor.java
  * GitInfoContributor: expose git information if a git.properties file is available
    * https://github.com/spring-projects/spring-boot/tree/master/spring-boot-actuator/src/main/java/org/springframework/boot/actuate/info/GitInfoContributor.java
  * BuildInfoContributor: expose build information if a META-INF/build-info.properties file is available
    * https://github.com/spring-projects/spring-boot/tree/master/spring-boot-actuator/src/main/java/org/springframework/boot/actuate/info/BuildInfoContributor.java
* customizable via properties
  * info.app.encoding=@project.build.sourceEncoding@
  * info.app.java.source=@java.version@
  * info.app.java.target=@java.version@
  * extract info from build: https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/\#howto-automatic-expansion
* custom ones can be created & registered

## Info endpoint: Git commit information

* publish information about the state of your git source code repository when the project was built
* if a GitProperties bean is available the git.branch, git.commit.id and git.commit.time properties will be exposed
  * that bean will be configured if a git.properties file is available at the root of the classpath: https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/\#howto-git-info

## Info endpoint: Build information

* the info endpoint can also publish information about the build if a BuildProperties bean is available
  * happens if a META-INF/build-info.properties file is on the classpath
* cfr https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/\#howto-build-info

## Info endpoint: Custom info

* register beans that implement the InfoContributor interface



