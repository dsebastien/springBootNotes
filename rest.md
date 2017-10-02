# Web Services

## REST

* RestTemplateBuilder: automatically configured
  * to easily create RestTemplate objects
  * ensures sensibleHttpMessageConverters are applied to the created RestTemplate objects
* RestTemplateCustomizer: automatically registered with the automatically configured builder
  * to apply common customizations to all the created RestTemplate objects

## SOAP

* auto-configuration for definind Endpoints
* spring-boot-starter-webservices
* SimpleWsdl11Definition
* SimpleXsdSchema beans can be automatically created for your WSDLs and XSDs
  * configure location via spring.webservices.wsdl-locations=classpath:/wsdl



