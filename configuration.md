# Configuration

## General

* start with --debug to see all automatic configuration classes
  * those classes make the Spring Boot magic happen
* @Import to load additional configuration classes
* @EnableAutoConfiguration\(exclude =... .class\): to disable some automatic configuration classes
* SpringApplication app = new SpringApplication...
  * or SpringApplicationBuilder
* ## Automatic refresh/reload
* dedicated classloaders

* to exclude paths from causing refresh/reload: spring.devtools.restart.exclude=&lt;paths to exclude&gt;
* to add paths: spring.devtools.restart.additional-paths=&lt;...&gt;

## Banner

* Spring banner customizable
* banner.txt file or banner.location or code \(SpringApplication app = new ...; app.setBanner ...
* variables can be used within
  * can be defined through MANIFEST.MF \(e.g., application name with Maven filtering applied to the manifest\)



