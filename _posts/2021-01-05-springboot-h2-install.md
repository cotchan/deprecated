---
title: Spring-Boot) h2 DB 설치 방법
author: cotchan 
date: 2021-01-05 04:45:21 +0800 
categories: [Spring-Boot, Spring-Boot_DB]
tags: [spring-boot] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 다운로드 & 압축풀기

+ [다운로드 페이지](http://h2database.com/html/main.html)
    + `All Platforms` 권장
    + 참고로 java로 실행하므로 java가 설치되어 있어야 합니다.

+ 압축을 풀면 `h2 폴더`가 생깁니다.

---

## 2. 권한주기

+ h2.sh 파일 실행을 위한 권한을 줍니다.

```terminal
$ cd bin
$ chmod 755 h2.sh
```

---

## 3. bin 폴더 내 h2.sh 실행(mac OS 기준)

```terminal
$ ./h2.sh
```

---

## 4. JDBC URL 수정

+ 웹페이지 화면의 JDBC URL을 수정합니다.
    + `jdbc:h2:~/jpashop`
    + DB 파일을 생성할 경로를 지정해주는 것입니다.

---

## 5. *.mv.db 파일 생성

+ `~` path에서 살펴보면 `jpashop.mv.db` 파일이 생성된 것을 볼 수 있습니다. 

```terminal
$ ls -al
```

---

## 6. 연결 끊기

+ 웹페이지 화면의 11시 방향에 있는 `연결 끊기` 클릭

---

## 7. JDBC URL 수정 2

+ 이제부터는 다시 JDBC URL을 수정한 후 접근해줍니다. 
    + `jdbc:h2:tcp://localhost/~/jpashop`


---

+ 출처
	+ [실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1/dashboard)
