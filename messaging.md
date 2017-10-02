# Messaging

* JmsTemplate
* Spring AMQP
  * RabbitMQ --&gt; RabbitTemplate
* STOMP
* Spring WebSocket

## JMS

* automatic configuration for
  * ActiveMQ
* properties: spring.activemq.\*
* spring-boot-starter-activemq

Pooling

* add a dependency to org.apache.activemq:active-mq-pool
  * PooledConnectionFactory
  * * corresponding properties

Apache Artemis

* automatically configured by Spring Boot if present on the classpath

JMS

* JmsTemplate automatically configured
* @JmsListener beans are automatically registered
* JmsListenerContainerFactory created automatically
  * transactional by default
* IF JtaTransactionManager is present, it is automatically associated with the listener container
* @JmsListener\(destination="someQueue"\)
* check @EnableJms
* we can customize via DefaultJmsListenerContainerFactoryConfigurer
  * can be used to initialize the DefaultJmsListenerContainerFactory

AMQP

* RabbitMQ supported
* spring-boot-starter-amqp
* properties: spring.rabbitmq.\*
  * full list: RabbitProperties
* RabbitMessageTemplate, AmqpTemplate and AmqpAdmin are automatically configured and can be injected
* if a MessageConverter bean is declared, it is automatically associated with the AmqpTemplate

## Kafka

* properties: spring.kafka.\*
  * full list: KafkaProperties
* KafkaTemplate automatically configured
* @KafkaListener beans are automatically registered

## Mail

* JavaMailSender automatically created
* spring-boot-starter-mail
* properties: spring.mail.\*



