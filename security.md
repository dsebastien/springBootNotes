# Security

## Spring Security

### Default configuration

* auto configuration if Spring security is on the classpath
* basic authentication is enabled by default
  * AuthenticationManager: single default user 'user' with a random password \(printed on the console at startup\)
    * password can be provided via security.user.password
* to enable method-level security, add @EnableGlobalMethodSecurity
* all properties listed in SecurityProperties
* basic out-of-the-box features
  * an AuthenticationManager with in-memory store and a single user
  * ignored \(insecure\) paths for common static resource locations \(/css, /js, /images, /webjars, /favicon.ico\)** **
  * HTTP basic security for all other endpoints
  * Security events published to Spring's ApplicationEventPublisher
  * Common low-level features \(HSTS, XSS, CSRF, caching\) provided by Spring Security are on by default

### Customization

* the default security is implemented in SecurityAutoConfiguration
  * 
* add a bean with @EnableWebSecurity
  * this does not disable the authentication manager configuration or Actuator's security\)
  * customize it using external properties and beans of typeWebSecurityConfigurerAdapter
* to also switch off the authentication manager configuration, add a bean of type AuthenticationManager or configure the global AuthenticationManager by autowiring an AuthenticationManagerBuilder into a method in one of your @Configuration classes

## Actuator

* if Actuator is in use \(see Security\), then
  * the management endpoints are secure even if the application endpoints are insecure
  * security events are transformed into AuditEvent instances and published to the AuditEventRepository
  * the default user will have the ACTUATOR role as well as the USER role
* to configure
  * management.security.\* properties
  * ...

## OAuth 2

If spring-security-oauth2 is on the classpath, Spring Boot helps to setup an OAuth 2 Authorization and/or Resource Server

Authorization server

* @EnableAuthorizationServer and provide
  * security.oauth2.client.client-id
  * security.oauth2.client.client-secret
  * with those, the client will be registered for you in an in-memory repository
* curl client:secret@localhost:8080/oauth/token -d grant\_type=password -d username=user -d password=foo
  * basic OAuth credentials for /token are the client-id and client-secret
  * user credentials are those default of Spring Security with Spring Boot
* to configure yourself
  * @Bean of type AuthorizationServerConfigurer

Resource server

* @EnableResourceServer and provide
  * security.oauth2.resource.user-info-uri \(e.g., /me\)
  * security.oauth2.resource.token-info-uri \(e.g., /check\_token\)
  * OR
  * security.oauth2.resource.jwt.key-value
    * verification key \(symmetric secret or PEM encoded RSA pub key\)

SSO

* OAuth 2 client: fetch user details from provider
  * convert details to an Authentication token from Spring Security
  * supported via user-info-uri
* @EnableOAuth2Sso



