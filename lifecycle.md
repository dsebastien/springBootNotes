## Automatic refresh/reload

* dedicated classloaders for parts that can be reloaded or not

* to exclude paths from causing refresh/reload: spring.devtools.restart.exclude=&lt;paths to exclude&gt;

* to add paths: spring.devtools.restart.additional-paths=&lt;...&gt;

# Events

* Events we can listen/react to
  * ApplicationStartingEvent
  * ApplicationEnvironmentPreparedEvent
  * ApplicationPreparedEvent
  * ApplicationReadyEvent
  * ApplicationFailedEvent
* To configure listeners
  * SpringApplication.addListeners\(...\)
  * SpringApplicationBuilder.listeners\(...\)
* to run code once the application has started, implement ApplicationRunner or CommandLineRunner
  * optionally with @Order if there are multiple runners

# Shutdown

* Exit code generator 
* ```
  System.exit(SpringApplication.exit(SpringApplication.run(ExitCodeApplication.class)))
  ```
* Where ExitCodeApplication.class has

* ```
  ...
  @Bean
  public ExitCodeGenerator() {
      public int getExitCode() { ... }
  }
  ```



