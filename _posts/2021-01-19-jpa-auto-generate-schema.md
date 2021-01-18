---
title: JPA) 데이터베이스 스키마 자동 생성 방법
author: cotchan 
date: 2021-01-19 05:20:21 +0800 
categories: [JPA, JPA_Basic]
tags: [jpa] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. DB 스키마 자동 생성

+ **JPA에서는 애플리케이션 로딩(실행) 시점에 DB 테이블을 생성하는 기능**도 지원해줍니다.

+ 테이블 중심 => 객체 중심 가능

+ 데이터베이스 방언을 활용해서 데이터베이스에 맞는 적절한 DDL을 생성해줍니다.

+ 이렇게 **생성된 DDL은 꼭 개발 장비에서만 사용해야 합니다.** 
  + 운영환경에서는 절대 사용해서는 안됩니다.

+ 생성된 DDL은 운영 서버에서는 사용하지 않거나, 적절히 다듬은 후에 사용합니다.

---

## 2. DB 스키마 자동 생성 - 속성

---

## 2-1. hibernate.hbm2ddl.auto

```xml
<!-- persistence.xml -->
<property name="hibernate.hbm2ddl.auto" value="create" />
<property name="hibernate.hbm2ddl.auto" value="create-drop" />
<property name="hibernate.hbm2ddl.auto" value="update" />
<property name="hibernate.hbm2ddl.auto" value="validate" />
<property name="hibernate.hbm2ddl.auto" value="none" />
```

![Desktop View](/assets/img/post/jpa/2021-01-19-jpa-generate-schema-01.png)

+ UPDATE 옵션의 경우 
  + 추가하는 것만 가능, 지우는 것에는 해당되지 않습니다.
  + TABLE 변경을 원하는데, drop은 하고 싶지 않은 경우에 사용합니다. (ALTER)

---

## 3. DB 스키마 자동 생성 - 주의점

+ **운영 장비에는 절대 `create`, `create-drop`, `update`를 사용하면 안됩니다.**

+ 개발 초기 단계에는 `create` 또는 `update`를 사용해서 자신의 개발장비에서 사용하면 됩니다.

+ 테스트 서버(여러 사람이 사용)는 `update` 또는 `validate`를 권장합니다.
  + 여기서는 create를 사용하면 안됩니다.
  + create를 사용하면 이전의 데이터가 모두 날아갑니다.
  + 그러나 가능하다면 그냥 안 쓰는 것을 권장합니다.

+ 스테이징과 운영 서버는 `validate` 또는 `none`을 권장합니다.
  + 그러나 가능하다면 그냥 안 쓰는 것을 권장합니다.

+ **결론은 본인의 로컬 PC에서만 자유롭게 사용하는 것을 권장합니다.**

---


## 4. DDL 생성 기능 추가

+ 제약 조건 추가 가능
+ 유니크 제약 초건 추가 가능

![Desktop View](/assets/img/post/jpa/2021-01-19-jpa-generate-schema-02.png)


---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
