---
title: sb3) class is not in the trusted packages 에러
author: cotchan 
date: 2021-06-01 22:40:21 +0800 
categories: [Spring-Boot3]
tags: [kafka] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 해결방법

+ **JsonDeserializer에 `addTrustedPackages() 메서드를 수정`하면 됩니다.**

```java
@Bean
public ConsumerFactory<String, Model> consumerFactory() {
    JsonDeserializer<Model> deserializer = new JsonDeserializer<>(Model.class);
    deserializer.addTrustedPackages("*");
    deserializer.setUseTypeMapperForKey(true);

    //...
    Map<String, Object> props = new HashMap<>();
    props.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, bootstrapServers);
    props.put(ConsumerConfig.GROUP_ID_CONFIG, groupId);
    props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class);
    props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, deserializer);
    return new DefaultKafkaConsumerFactory<>(
        props,
        new StringDeserializer(),
        deserializer);
}
```

---

+ 출처
  + [[Kafka] class is not in the trusted packages 에러](https://sup2is.tistory.com/102)
