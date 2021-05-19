---
title: sb3) ReplyingKafkaTemplate 사용 방법 
author: cotchan 
date: 2021-05-20 02:24:21 +0800 
categories: [Spring-Boot3]
tags: [kafka] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ 계속해서 업데이트 할 예정입니다.

---

## 0. INTRO

+ **Producer Kafka와 Consumer Kafka는 별도의 스프링부트 프로젝트입니다.**

---

## 1. ReplyingKafkaTemplate 란?

---


## 2. Producer Kafka 만들기

+ **Producer Kafka는 Consumer Kafka에게 요청을 보내고 Consumer Kafka로부터 결과를 받는 역할을 합니다.**

---

## 2-1. ProducerKafkaConfig.java

+ 여기서 중요한 건 아래 세 가지입니다.
  + **Producer KafKa에서 `ReplyingKafkaTemplate을 빈으로 등록`합니다.**
  + **그리고 ReplyingKafkaTemplate에서 응답을 들을 `replyContainer 또한 빈으로 등록`합니다.**
  + 그리고 producer에 대한 config와 factory 셋팅과 consumer에 대한 config와 factory 셋팅을 모두 완료합니다.

```java
package com.github.prgrms.social.configure;

import org.springframework.kafka.support.serializer.JsonDeserializer;
import org.springframework.kafka.support.serializer.JsonSerializer;

/**
 * 1. Set Up Spring ReplyingKafkaTemplate
 */
@Configuration
public class ProducerKafkaConfig {

    @Value("${kafka.bootstrap-servers}")
    private String bootstrapServers;

    @Value("${kafka.topic.request-reply-topic}")
    private String requestReplyTopic;

    @Value("${kafka.consumer-group}")
    private String consumerGroup;

    // ReplyingKafkaTemplate
    @Bean
    public ReplyingKafkaTemplate<String, String, String> replyKafkaTemplate(ProducerFactory<String, String> pf,
                                                                            KafkaMessageListenerContainer<String, String> container) {
        return new ReplyingKafkaTemplate<>(pf, container);
    }

    // Listener Container to be set up in ReplyingKafkaTemplate
    @Bean
    public KafkaMessageListenerContainer<String, String> replyContainer(ConsumerFactory<String, String> cf) {
        ContainerProperties containerProperties = new ContainerProperties(requestReplyTopic);
        return new KafkaMessageListenerContainer<>(cf, containerProperties);
    }

    // ==================== Producer Region ====================
    // Default Producer Factory to be used in ReplyingKafkaTemplate
    @Bean
    public ProducerFactory<String,String> producerFactory() {
        return new DefaultKafkaProducerFactory<>(producerConfigs());
    }

    // Standard KafkaProducer settings - specifying brokerand serializer
    @Bean
    public Map<String, Object> producerConfigs() {
        Map<String, Object> props = new HashMap<>();
        props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, bootstrapServers);
        props.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class);
        props.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, JsonSerializer.class);
        return props;
    }
    // ==================== End Producer Region ====================

    // ==================== Consumer Region ====================
    // Default Consumer Factory
    @Bean
    public ConsumerFactory<String, String> consumerFactory() {
        return new DefaultKafkaConsumerFactory<>(consumerConfigs(),new StringDeserializer(),new JsonDeserializer<>(String.class));
    }

    @Bean
    public Map<String, Object> consumerConfigs() {
        Map<String, Object> props = new HashMap<>();

        props.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, bootstrapServers);
        props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class);
        props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, JsonDeserializer.class);
        props.put(ConsumerConfig.GROUP_ID_CONFIG, consumerGroup);
        return props;
    }
    // ==================== End Consumer Region ====================
}
```

---

## 3. ConSumer Kafka 만들기

+ **Consumer Kafka는 Producer Kafka가 보낸 요청을 받고, 그 응답을 돌려주는 역할을 합니다.**

---

## 3-1. ConsumeKafkaConfig

+ 일반적인 셋팅은 Kafka Listener와 같습니다.
  + consumerFactory
  + kafkaListenerContainerFactory

```java
// Default Consumer Factory
@Bean
public ConsumerFactory<String, Model> consumerFactory() {
  return new DefaultKafkaConsumerFactory<>(consumerConfigs(),new StringDeserializer(),new JsonDeserializer<>(Model.class));
}

// Concurrent Listner container factory
@Bean
public KafkaListenerContainerFactory<ConcurrentMessageListenerContainer<String, Model>> kafkaListenerContainerFactory() {
  ConcurrentKafkaListenerContainerFactory<String, Model> factory = new ConcurrentKafkaListenerContainerFactory<>();
  factory.setConsumerFactory(consumerFactory());
  // NOTE - set up of reply template
  factory.setReplyTemplate(kafkaTemplate());
  return factory;
}
```

+ **하지만 추가적으로 `ReplyTemplate`을 가지고 있습니다.**
  + 그 이유는 ProducerKafka에게 응답을 돌려주려면 Consumer에서도 결과를 토픽으로 쏴줘야 하기 때문입니다.
  + **그러므로 결과적으로 `kafkaTemplate을 필드로 갖고 있습니다.`**
  + 그래서 Consumer Kafka 또한 producer config, factory 셋팅이 필요합니다.

```java
import org.springframework.kafka.support.serializer.JsonDeserializer;
import org.springframework.kafka.support.serializer.JsonSerializer;

/**
 * 2. Set Up Spring-Kafka Listener
 */
@Configuration
public class ConsumeKafkaConfig {

    @Value("${kafka.bootstrap-servers}")
    private String bootstrapServers;

    @Value("${kafka.consumer-group}")
    private String consumerGroup;

    //FIXME
    // Concurrent Listner container factory
    @Bean
    public KafkaListenerContainerFactory<ConcurrentMessageListenerContainer<String, String>> kafkaListenerContainerFactory() {
        ConcurrentKafkaListenerContainerFactory<String, String> factory = new ConcurrentKafkaListenerContainerFactory<>();
        factory.setConsumerFactory(consumerFactory());
        // NOTE - set up of reply template
        factory.setReplyTemplate(kafkaTemplate());
        return factory;
    }

    // Standard KafkaTemplate
    @Bean
    public KafkaTemplate<String, String> kafkaTemplate() {
        return new KafkaTemplate<>(producerFactory());
    }

    // ==================== Consumer Region ====================
    // Default Consumer Factory
    @Bean
    public ConsumerFactory<String, String> consumerFactory() {
        return new DefaultKafkaConsumerFactory<>(consumerConfigs(),
                new StringDeserializer(),
                new ErrorHandlingDeserializer(new JsonDeserializer(String.class)));
    }

    @Bean
    public Map<String, Object> consumerConfigs() {
        Map<String, Object> props = new HashMap<>();

        props.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, bootstrapServers);
        props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class);
        props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, JsonDeserializer.class);
        props.put(ConsumerConfig.GROUP_ID_CONFIG, consumerGroup);
        return props;
    }
    // ==================== Consumer Region ====================

    // ==================== Producer Region ====================
    // Default Producer Factory to be used in ReplyingKafkaTemplate
    @Bean
    public ProducerFactory<String,String> producerFactory() {
        return new DefaultKafkaProducerFactory<>(producerConfigs());
    }

    // Standard KafkaProducer settings - specifying brokerand serializer
    @Bean
    public Map<String, Object> producerConfigs() {
        Map<String, Object> props = new HashMap<>();
        props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, bootstrapServers);
        props.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class);
        props.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, JsonSerializer.class);
        return props;
    }
    // ==================== End Producer Region ====================
}
```


---

## 4. TOPIC Config

+ **Kafka에서 메시지를 보내려면 토픽이 반드시 있어야 합니다.**
+ **그러므로 `producer Kafka, consumer Kakfa가 공통으로 사용`할 Topic 설정클래스를 만듭니다.**

```java
//KafkaTopicConfig.java

@Configuration
public class KafkaTopicConfig {

    @Value("${kafka.topic.request-topic}")
    String requestTopic;

    @Value("${kafka.topic.request-reply-topic}")
    String requestReplyTopic;

    @Bean
    public NewTopic requestTopic() {
        return TopicBuilder.name(requestTopic).build();
    }

    @Bean
    public NewTopic requestReplyTopic() {
        return TopicBuilder.name(requestReplyTopic).build();
    }
}
```

---

## 5. 프로듀서, 컨슈머 사용방법

---

## 5-1 Producer Kafka 사용 방법

+ 사용할 때는 아래와 같이 3가지 절차를 밟습니다.

1. **producer record 만들기**
  + **요청을 보내는 통로인 `requestTopic`과 실제 요청 데이터를 담을 `Model`이 record를 구성합니다.**
2. **set reply topic in header**
  + requestTopic과는 별개로 응답을 받을 reply topic을 셋팅합니다. 
3. **post in kafka topic**
  + **요청을 보낼 때는 `sendAndReceive(record)` 메소드를 통해 요청을 보내고 응답을 받습니다.**

```java
//Sample Code 1
@ResponseBody
@PostMapping(value="/sum",produces=MediaType.APPLICATION_JSON_VALUE,consumes=MediaType.APPLICATION_JSON_VALUE)
public Model sum(@RequestBody Model request) throws InterruptedException, ExecutionException {
  // create producer record
  ProducerRecord<String, Model> record = new ProducerRecord<String, Model>(requestTopic, request);
  // set reply topic in header
  record.headers().add(new RecordHeader(KafkaHeaders.REPLY_TOPIC, requestReplyTopic.getBytes()));
  // post in kafka topic
  RequestReplyFuture<String, Model, Model> sendAndReceive = kafkaTemplate.sendAndReceive(record);

  // confirm if producer produced successfully
```

---

+ `SubscribeController`에서 사용

```java
//Sample Code 2
@RestController
@RequestMapping("api")
@Api(tags = "Push 구독 APIs")
public class SubscribeController {

  ReplyingKafkaTemplate<String, String, String> kafkaTemplate;

  @Value("${kafka.topic.request-topic}")
  String requestTopic;

  @Value("${kafka.topic.request-reply-topic}")
  String requestReplyTopic;

  public SubscribeController(ReplyingKafkaTemplate kafkaTemplate) {
    this.kafkaTemplate = kafkaTemplate;
  }

  //FIXME
  //ApiResult<Subscription>
  @PostMapping(value = "/subscribe", produces=MediaType.APPLICATION_JSON_VALUE,consumes=MediaType.APPLICATION_JSON_VALUE)
  @ApiOperation(value = "Push 구독")
  public ApiResult<String> subscribe(
    @AuthenticationPrincipal JwtAuthentication authentication,
    @RequestBody Subscription subscription
  ) throws InterruptedException, ExecutionException {

    // 0. create subscription
    Subscription.SubscriptionBuilder subscriptionBuilder = new Subscription.SubscriptionBuilder(subscription);
    subscriptionBuilder.userId(authentication.id);
    Subscription requestSubscribe = subscriptionBuilder.build();

    ProducerRecord<String, String> record = new ProducerRecord<>(requestTopic, requestSubscribe.toString());
    // 2. set reply topic in header
    record.headers().add(new RecordHeader(KafkaHeaders.REPLY_TOPIC, requestReplyTopic.getBytes()));
    // 3. post in kafka topic
    RequestReplyFuture<String, String, String> sendAndReceive = kafkaTemplate.sendAndReceive(record);

    // 4. confirm if producer produced successfully
    SendResult<String, String> sendResult = sendAndReceive.getSendFuture().get();
    // 4-1. print all headers
    sendResult.getProducerRecord().headers().forEach(header -> System.out.println(header.key() + ":" + header.value().toString()));
    // 5. get consumer record
    ConsumerRecord<String, String> consumerRecord = sendAndReceive.get();

    // 6. return consumer value
    return OK(consumerRecord.value());
  }
}
```

---

## 5-2. Consumer 사용 방법  

+ **Consumer Kafka를 사용할 때는 그냥 사용하고자 하는 `Function에 아래 2개의 어노테이션을 달아주면 끝`입니다.**
  + 생성자 주입 등 Function에서 사용하기 위한 별도의 절차는 없습니다.

+ **`@KafkaListener`**
  + @KafkaListener에는 Producer Kafka가 요청을 보낼 때 사용하는 Topic을 등록합니다.
  + 즉, 이 예시에서는 Producer가 requestTopic을 통해서 요청을 보내므로 requestTopic을 등록합니다. 

+ **`@SendTo`**
  + **이 어노테이션은 Function의 리턴값과 연관있습니다. 이 어노테이션은 `Reply Topic으로 Model을 리턴합니다.`**
  + 즉, Reply Topic을 통해 돌려주고자 하는 응답값을 리턴으로 줍니다. 

```
@KafkaListener(topics = "${kafka.topic.request-topic}")
@SendTo
public Model listen(Model request) throws InterruptedException {

  int sum = request.getFirstNumber() + request.getSecondNumber();
  request.setAdditionalProperty("sum", sum);
  return request;
}
```

---

+ **중요한 점은 Consumer Kafka를 사용하는 클래스에서는 `필드에 Kafka 관련 의존을 주입받지 않습니다.`**
+ **사용하려는 함수에 `2개 어노테이션`만 달아주고 `함수의 Parameter와 리턴값을 맞춰주면` 알아서 Reply Topic으로 함수 결과값을 Producer Kafka에게 쏴줍니다.**
+ **그러면 Consumer Kafka로부터 받은 응답은 `sendAndReceive의 결과값으로 셋팅됩니다.`**

```java
//NotificationService.java

  @KafkaListener(topics = "${kafka.topic.request-topic}")
  @SendTo
  public String subscribe(String subscriptionString) {

    try {
      Subscription subscription = new Subscription(subscriptionString);
      subscriptionRepository.save(subscription);
      return "SUCCESS";
    } catch (Exception e) {
      log.warn("Subscribe insert fail {}", e.getMessage(), e);
      return "FAIL";
    }
  }
```

---

## 6. 부록 application.yaml

---

## 6-1. ProducerKafka

```
spring:
  application:
    name: social_server_api
  kafka:
    consumer:
      group-id: myGroup
server:
  port: 8080
kafka:
  bootstrap-servers: localhost:9092
  consumer-group: producer-group
  topic:
    request-topic: v1.event.subscription.request
    request-reply-topic: v1.event.subscription.response
```

---

## 6-2. ConsumerKafka

```
spring:
  application:
    name: social_server_api
  kafka:
    consumer:
      group-id: myGroup
server:
  port: 8080
kafka:
  bootstrap-servers: localhost:9092
  consumer-group: consumer-group
  topic:
    request-topic: v1.event.subscription.request
    request-reply-topic: v1.event.subscription.response
```

---

## 7. 컴포넌트별 사용 Config 파일

+ **`ProducerKafka`**
  + TopicConfig.java
  + ProducerKafkaConfig.java

+ **`ConsumerKafka`**
  + TopicConfig.java
  + ConsumeKafkaConfig.java

---

+ 출처
    + [[스터디/9기] 단순 CRUD는 그만! 웹 백엔드 시스템 구현(Spring Boot)](https://programmers.co.kr/learn/courses/11694) 
    + [Synchronous Kafka: Using Spring Request-Reply](https://dzone.com/articles/synchronous-kafka-using-spring-request-reply-1)
    + [Apache Kafka request-reply semantics implementation: ReplyingKafkaTemplate](https://shuaibabdulla40.medium.com/apache-kafka-request-reply-semantics-implementation-replyingkafkatemplate-5bf64958268c)
    + [Using Kafka with Spring Boot](https://reflectoring.io/spring-boot-kafka/)
