---
title: sb2) 14. [화면개발] 레이아웃 적용(부트스트랩, jQuery)
author: cotchan 
date: 2021-03-11 19:58:21 +0800 
categories: [Spring-Boot2, Spring-Boot2_DEV]
tags: [spring-boot2] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. layout 디렉토리 추가

+ **`/src/main/resources/templates`** 디렉토리에 layout 디렉토리 추가

---

## 2. 레이아웃 파일에 공통 코드 추가

## 2-1. header.mustache

+ 경로: **`/src/main/resources/templates/layout`**

```html
<!DOCTYPE HTML>
<html>
<head>
    <title>스프링 부트 웹 서비스</title>
    <meta http-equiv="Content-Type" content="text/html"; charset="UTF-8" />

    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css">
</head>
<body>
```

---

## 2-2. footer.mustache

+ 경로: **`/src/main/resources/templates/layout`**

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js">
</script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js">
</script>

</body>
</html>
```

---

## 3. 레이아웃 적용(index.mustache 수정)

+ 경로: **`/src/main/resources/templates`**

+ **`{{>}}`**는 현재 머스테치 파일(`index.mustache`)을 기준으로 다른 파일을 가져옵니다.

```

{{>layout/header}}

<h1>스프링 부트로 시작하는 웹 서비스</h1>

{{>layout/footer}}

``` 


---

+ 출처
  + 이동욱, 『스프링 부트와 AWS로 혼자 구현하는 웹 서비스』, 프리렉(2019) 
