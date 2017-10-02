# Production

* features included to monitor and manage your application when pushed to production
* choose to manage and monitoring using HTTP endpoints or JMX
* auditing, health and metrics gathering can also be applied automatically

## Actuator

* actuator exposes all @ConfigurationProperties beans
  * URL in a Web app: /configprops
  * equivalent JMX endpoint
* Http endpoints only available if Spring MVC-based application
  * NOK with Jersey \(unless Spring MVC is also there: [https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/\#howto-use-actuator-with-jersey\](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/#howto-use-actuator-with-jersey\)\)
* enabled via spring-boot-actuator
* actuator built-in endpoints \(custom ones can be added\): [https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/\#production-ready-endpoints](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/#production-ready-endpoints)

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
    * all properties: check CorsEndpointProperties: [https://github.com/spring-projects/spring-boot/tree/master/spring-boot-actuator/src/main/java/org/springframework/boot/actuate/autoconfigure/endpoint/infrastructure/CorsEndpointProperties.java](https://github.com/spring-projects/spring-boot/tree/master/spring-boot-actuator/src/main/java/org/springframework/boot/actuate/autoconfigure/endpoint/infrastructure/CorsEndpointProperties.java)

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

* customizing

  * endpoint paths: management.context-path

  * management server port: management.port=8081

  * custom TLS: server.ssl.key-store, server.ssl.key-password, management.port, management.ssl....

  * management address: management.address=...

  * disable HTTP endpoints: management.port=-1

* JMX

  * MBeans are exposed under the "org.springframework.boot" domain

  * customizing MBean names is possible

    * management.endpoints.jmx.unique-names=true \(always unique, avoids clashes\)

    * management.endpoints.jmx.domain=be.cool

  * disabling

    * endpoints.default.jmx.enabled=false

  * using Jolokia for JMX over HTTP

    * include a dependency to org.jolokia:jolokia-core

    * jolokia can then be accessed using /application/jolokia

    * customizing

      * management.jolokia.config.\*

      * management.jolokia.config.debug=true

    * to disable Spring Boot automatic support for Jolokia

      * management.jolokia.enabled=false

## Health endpoints

* exposed by the "health" endpoint
* if unauthenticated: simple status message
* if authenticated, details are provided: [https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/\#production-ready-health-access-restrictions](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/#production-ready-health-access-restrictions)
* health information is collected from all HealthIndicator beans \([https://github.com/spring-projects/spring-boot/tree/master/spring-boot-actuator/src/main/java/org/springframework/boot/actuate/health/HealthIndicator.java\](https://github.com/spring-projects/spring-boot/tree/master/spring-boot-actuator/src/main/java/org/springframework/boot/actuate/health/HealthIndicator.java\)\)
* possible to define custom ones in addition to the built-in ones
* the final system state is derived by the HealthAggregator \(sorts the statuses from each indicator\)

## Info endpoint

* exposes various information collected from all InfoContributor beans: [https://github.com/spring-projects/spring-boot/tree/master/spring-boot-actuator/src/main/java/org/springframework/boot/actuate/info/InfoContributor.java](https://github.com/spring-projects/spring-boot/tree/master/spring-boot-actuator/src/main/java/org/springframework/boot/actuate/info/InfoContributor.java)
* automatically configured ones
  * EnvironmentInfoContributor: expose any keys from the Environment under the info key
    * [https://github.com/spring-projects/spring-boot/tree/master/spring-boot-actuator/src/main/java/org/springframework/boot/actuate/info/EnvironmentInfoContributor.java](https://github.com/spring-projects/spring-boot/tree/master/spring-boot-actuator/src/main/java/org/springframework/boot/actuate/info/EnvironmentInfoContributor.java)
  * GitInfoContributor: expose git information if a git.properties file is available
    * [https://github.com/spring-projects/spring-boot/tree/master/spring-boot-actuator/src/main/java/org/springframework/boot/actuate/info/GitInfoContributor.java](https://github.com/spring-projects/spring-boot/tree/master/spring-boot-actuator/src/main/java/org/springframework/boot/actuate/info/GitInfoContributor.java)
  * BuildInfoContributor: expose build information if a META-INF/build-info.properties file is available
    * [https://github.com/spring-projects/spring-boot/tree/master/spring-boot-actuator/src/main/java/org/springframework/boot/actuate/info/BuildInfoContributor.java](https://github.com/spring-projects/spring-boot/tree/master/spring-boot-actuator/src/main/java/org/springframework/boot/actuate/info/BuildInfoContributor.java)
* customizable via properties
  * info.app.encoding=@project.build.sourceEncoding@
  * info.app.java.source=@java.version@
  * info.app.java.target=@java.version@
  * extract info from build: [https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/\#howto-automatic-expansion](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/#howto-automatic-expansion)
* custom ones can be created & registered

## Info endpoint: Git commit information

* publish information about the state of your git source code repository when the project was built
* if a GitProperties bean is available the git.branch, git.commit.id and git.commit.time properties will be exposed
  * that bean will be configured if a git.properties file is available at the root of the classpath: [https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/\#howto-git-info](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/#howto-git-info)

## Info endpoint: Build information

* the info endpoint can also publish information about the build if a BuildProperties bean is available
  * happens if a META-INF/build-info.properties file is on the classpath
* cfr [https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/\#howto-build-info](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/#howto-build-info)

## Info endpoint: Custom info

* register beans that implement the InfoContributor interface

## Loggers

* Spring Boot Actuator provides a way to view and configure the log levels at runtime
* POST a partial entity to the resource's URI { "configuredLevel": "DEBUG" }
  * TRACE, DEBUG, INFO, WARN, ERROR, FATAL, OFF, null \(no explicit config\)

## Metrics

* Spring Boot Actuator includes dependency management and auto-configuration for Micrometer \(an application metrics facade supporting numerous monitoring systems\): https://micrometer.io/
  * Atlas
  * Datadog
  * Ganglia
  * Graphite
  * Influx
  * Prometheus
* Micrometer provides a separate module for each monitoring system: https://micrometer.io/docs
* Spring MVC metrics
  * auto-configuration will enable instrumentation of requests handled by Spring MVC
  * when spring.metrics.web.server.auto-time-requests=true
    * for all requests, otherwise, enable by adding @Timed to a request-handling method
  * metrics are generated with the name http.server.requests
    * can be customized: spring.metrics.web.server.requests-metrics-name
  * tags
    * Request's method, request's URI, simple class name of any exception thrown, response's status
* WebFlux metrics
  * auto-configuration will enable instrumentation of all requests handled by WebFlux controllers
  * helper class: RouterFunctionMetrics provided that can be used to instrument applications using WebFlux's functional programming model
  * metrics are generated with the name http.server.requests
    * can be customized: spring.metrics.web.server.requests-metrics-name
* RestTemplate metrics
  * auto-configuration will customize the auto-configured RestTemplate.
  * MetricsRestTemplateCustomizer can be used to customize your own RestTemplate instances
  * metrics are generated with the name http.client.requests
    * customize via spring.metrics.web.client.requests-metrics-name
    * tags
      * Request's method, Request's URI \(templated if possible\), Response's status, Request URI's host

## Auditing

Spring Boot actuator has an audit framework that will publish events once Spring Security is in play

* for "authentication success", "failure" and "access denied" exceptions by default
* useful for reporting
* useful for a lock-out policy based on authentication failures \(ala fail-2-ban\)

Customize via own implementation of AbstractAuthenticationAuditListener and AbstractAuthorizationAuditListener.

Also possible to use audit services for your own business events

* inject the existing AuditEventRepository into your own components and use that directly
* or publish AuditApplicationEvent via the Spring ApplicationEventPublisher \(using ApplicationEventPublisherAware\)

## Tracing

Tracing is automatically enabled for all HTTP requests. You can view the trace Actuator endpoint and obtain basic information about the last 100 requests

Included by default:

* request headers
* response headers
* cookies
* errors
* time taken

Custom tracing

* inject a TraceRepository into your Spring beans
  * the add method accepts a single Map structure that will be converted to JSON and logged
* by default an InMemoryTraceRepository is used to store the last 100 events
  * can define your own instance of the InMemoryTraceRepository bean to expand the capacity
  * can also create another TraceRepository implementation

## Process monitoring

Spring Boot provides a set of classes to create files that are useufl for process monitoring:

* ApplicationPidFileWriter: creates a file containing the application PID \(by default in the application directory with the name application.pid\)
* EmbeddedServerPortFileWriter: generates an application.port file with the ports of the embedded server

This is disabled by default. To enable

* in META-INF/spring.factories, you can activate them
  * org.springframework.context.ApplicationListener=\
  * org.springframework.boot.system.ApplicationPidFileWriter,\
  * org.springframework.boot.actuate.system.EmbeddedServerPortFileWriter
* programmatically: invoke SpringApplication.addListeners\(...\) and passing the appropriate writer object
  * allows to customize the file name and path via the Writer constructor

## CloudFoundry support

Built-in support. Exposes /cloudfoundryapplication which provides an alternative secured route to all @Endpoint beans.

The extended support allows CloudFoundry management UIs to be augmented with Actuator information.

To disable: management.cloudfoundry.enabled=false







