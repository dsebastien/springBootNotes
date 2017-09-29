# Logging

## Support & recommendation

* Logback is recommended and selected by default for application code
* Spring boot uses commons logging internally
* default configurations provided for JUL, log4j2 and logback
  * console output with optional file output

## Enable traces

To enable traces: run with --trace or trace=true in application.properties

## File output

To enable file output, define logging.file \(file\) or logging.path \(dir\) in application.properties

* if logging.path is defined and logging.file isn't then the file will be called _spring.log_

File rotation is set to every 10MB by default

## Custom log configuration

Can be put at the root of the classpath

File name:

* recommended: logback-spring.xml or log4j2-spring.xml
  * allows Spring Boot to get more control
  * allows Spring Boot to add/register the logback extensions

## Variables in logging config

* to use variables in logback configuration, use Spring Boot's syntax instead of logback's
  * delimiter ":" instead of ":-"

## Profile-specific logging config

Use &lt;springProfile&gt; in the logging configuration file to specify for which profile\(s\) the configuration block applies:

```
<springProfile name="foo,bar">
    <!-- configuration here applied for foo and bar profiles -->
</springProfile>
```

## Use Environment properties in logging config

Use &lt;springProperty&gt; in the config:

```
<springProperty> <!-- allows to get values from Spring's Environment
...
<springProperty scope="context" name="host" source="app.server.host" defaultValues="http://localhost">
...
```

## 



