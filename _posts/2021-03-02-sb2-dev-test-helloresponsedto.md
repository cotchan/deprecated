---
title: sb2) 4. helloResponseDto 테스트 코드 작성(Lombok 검증)  
author: cotchan 
date: 2021-03-02 22:00:21 +0800 
categories: [Spring-Boot2, Spring-Boot2_DEV]
tags: [spring-boot2] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 롬복 설치

+ 롬복 설치는 아래 포스팅을 참고하면 됩니다.

---

## 2. gradle version 다운 그레이드 

+ **이 테스트 그레들4 기준으로 통과합니다.**
+ 아래 코드를 실행해서 그레들 다운 그레이드를 진행해줍니다. 

```terminal
$ ./gradlew wrapper --gradle-version 4.10.2
```

---

## 3. HelloResponseDto 코드 작성

```java
import lombok.Getter;
import lombok.RequiredArgsConstructor;

@Getter
@RequiredArgsConstructor
public class HelloResponseDto {

    private final String name;
    private final int amount;
}
``` 

---

## 4. HelloResponseDto 테스트 코드

```java
import org.junit.Test;

import static org.assertj.core.api.Assertions.assertThat;


public class HelloResponseDtoTest {

    @Test
    public void 롬복_기능_테스트() {
        //given
        String name = "test";
        int amount = 1000;

        //when
        HelloResponseDto dto = new HelloResponseDto(name, amount);

        //then
        assertThat(dto.getName()).isEqualTo(name);
        assertThat(dto.getAmount()).isEqualTo(amount);
    }
}
```

---

+ 출처
  + 이동욱, 『스프링 부트와 AWS로 혼자 구현하는 웹 서비스』, 프리렉(2019) 
