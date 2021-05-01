---
title: sb3) IntelliJ로 Maven 프로젝트 가져오는 방법
author: cotchan 
date: 2021-05-02 00:00:21 +0800 
categories: [Spring-Boot3]
tags: [spring-setting] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처를 참고하여 작성한 글입니다.**

---

## Maven 프로젝트 시작하기

+ Create New Project 버튼을 클릭합니다.

![Desktop View](/assets/img/post/spring-boot3/intellij-maven-start-1.png)

---

+ 비어있는 프로젝트를 생성합니다.

![Desktop View](/assets/img/post/spring-boot3/intellij-maven-start-2.png)

---

+ 프로젝트 Root 디렉토리를 지정합니다. 디렉토리가 없는 경우 자동으로 생성됩니다.


![Desktop View](/assets/img/post/spring-boot3/intellij-maven-start-3.png)

---

+ 시스템 JDK 경로를 지정하고, 기본 컴파일 레벨을 8로 지정해줍니다.

![Desktop View](/assets/img/post/spring-boot3/intellij-maven-start-4.png)

---

+ 상단 메뉴에서 File > New > Module from Existing Source 를 선택합니다.

![Desktop View](/assets/img/post/spring-boot3/intellij-maven-start-5.png)

---

+ 프로젝트 Root 디렉토리에 GitHub에서 Clone한 프로젝트를 준비하고 선택합니다.

![Desktop View](/assets/img/post/spring-boot3/intellij-maven-start-6.png)


---

+ Import module from external model을 선택하고 Maven을 선택한 후 Next를 클릭합니다.

![Desktop View](/assets/img/post/spring-boot3/intellij-maven-start-7.png)

---

+ Source와 Documentation을 체크하면 오픈소스 소스코드와 문서를 자동으로 다운로드합니다. (선택)

![Desktop View](/assets/img/post/spring-boot3/intellij-maven-start-8.png)


---

+ Import하는 Maven 프로젝트의 groupId, ArtifactId, Version을 확인할 수 있습니다.

![Desktop View](/assets/img/post/spring-boot3/intellij-maven-start-9.png)


---

+ SocialServer 좌측의 초록색 삼각형 버튼을 클릭하면 프로젝트를 실행할 수 있습니다.
+ 프로젝트가 정상적으로 실행되면 브라우저에 `http://localhost:8080`을 입력 후 첫 페이지에 진입할 수 있습니다.

![Desktop View](/assets/img/post/spring-boot3/intellij-maven-start-10.png)

---

+ 출처
    + [[스터디/9기] 단순 CRUD는 그만! 웹 백엔드 시스템 구현(Spring Boot)](https://programmers.co.kr/learn/courses/11694) 
