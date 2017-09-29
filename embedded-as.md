# Embedded AS

## Defaults

* runs on port 8080 by default
* Servlets, Filters, Servlet \*Listener that are Spring beans are registered automatically with the embedded container
  * useful if you want to refer to a value from application.properties\|yml during configuration
  * this behavior is enabled via @ServletComponentScan
* If only a single servlet, it will be mapped to /, otherwise to the bean name as prefix
* filters will map to /\*

## Servlet Context Initialization

ServletContainerInitializer and WebApplicationInitializer are not executed by default; it's a design choice.

To perform servlet context initialization, you need to register a bean that implements ServletContextInitializer; the onStartup method provides access to the ServletContext and can be used as adapter to an existing WebApplicationInitializer

## Customization

* application.properties\|yml settings
  * server.port
  * server.address
  * server.session.persistence
  * server.session.timeout
  * server.error.path
  * server.tomcat....
  * server.undertow....
  * ...
* Complete list of properties in ServerProperties class
* Can be customized programmatically by implementing EmbeddedServletContainerCustomizer



