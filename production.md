# Production

## Actuator

* actuator exposes all @ConfigurationProperties beans
  * URL in a Web app: /configprops
  * equivalent JMX endpoint

## Monitoring

* MBeanServer automatically configured
  * bean id: mbeanServer
  * exposes any beans with JMX annotations
    * @ManagedResource
    * @ManagedAttribute
    * @ManagedOperation
  * see @JmxAutoConfiguration



