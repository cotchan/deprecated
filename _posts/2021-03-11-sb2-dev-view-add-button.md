---
title: sb2) 15. [화면개발] 글 등록 버튼 / 글 등록 페이지 / 등록 액션 추가하기
author: cotchan 
date: 2021-03-11 21:32:21 +0800 
categories: [Spring-Boot2, Spring-Boot2_DEV]
tags: [spring-boot2] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. index.mustache에 글 등록 버튼 추가

+ **`<a>` 태그를 이용해 글 등록 페이지로 이동하는 글 등록 버튼을 생성**
+ 이동할 페이지의 주소는 `/posts/save`입니다.

```html
<!-- >layout/header -->

    <h1>스프링 부트로 시작하는 웹 서비스</h1>
    <div class="col-md-12">
        <div class="row">
            <div class="col-md-6">
                <a href="/posts/save" role="button" class="btn btn-primary">글 등록</a>
            </div>
        </div>
    </div>

<!-- >layout/footer -->
```

---

![Desktop View](/assets/img/post/spring-boot2/2021-03-11-add-button.png)

---

## 2. 버튼 클릭시 글 등록 페이지로 가도록 라우팅 처리

+ `IndexController`

```java
package com.cotchan.review.springboot.web;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class IndexController {

    //...

    @GetMapping("/posts/save")
    public String postsSave() {
        return "posts-save";
    }
}
```

---

## 3. 글 등록 화면 생성

+ `posts-save.mustache`

```html
<!-- >layout/header -->

<h1>게시글 등록</h1>

<div class="col-md-12">
    <div class="col-md-4">
        <form>
            <div class="form-group">
                <label for="title"> 제목 </label>
                <input type="text" class="form-control" id="title" placeholder="제목을 입력하세요">
            </div>

            <div class="form-group">
                <label for="author"> 작성자 </label>
                <input type="text" class="form-control" id="author" placeholder="작성자를 입력하세요">
            </div>

            <div class="form-group">
                <label for="content"> 내용 </label>
                <textarea class="form-control" id="content" placeholder="내용을 입력하세요"></textarea>
            </div>
        </form>

        <a href="/" role="button" class="btn btn-secondary">취소</a>
        <button type="button" class="btn btn-primary" id="btn-save">등록</button>
    </div>
</div>

<!-- >layout/footer -->
```

---

![Desktop View](/assets/img/post/spring-boot2/2021-03-11-posts-save-page.png)

---

## 4. 글 등록 화면에 '등록' 버튼 기능 구현(index.js)

+ **`API를 호출하는 JS가 필요합니다.`**
  + 아직 게시글 등록 화면에 등록 버튼은 기능이 없습니다. API를 호출하는 JS가 없기 때문입니다.

1. `src/main/resources`에 **`static/js/app`** 디렉토리를 생성합니다.
2. 위 디렉토리에 `index.js`를 생성합니다.

```javascript
//index.js
var main = {
    init: function () {
        var _this = this;
        $('#btn-save').on('click', function () {
            _this.save();
        });
    },

    save: function () {
        var data = {
            title: $('#title').val(),
            author: $('#author').val(),
            content: $('#content').val()
        };

        $.ajax({
            type: 'POST',
            url: '/api/v1/posts',
            dataType: 'json',
            contentType: 'application/json; charset=utf-8',
            data: JSON.stringify(data)
        }).done(function () {
            alert('글이 등록되었습니다.');
            window.location.href='/';
        }).fail(function (error) {
            alert(JSON.stringify(error));
        });
    }

};

main.init();
```

---

## 5. index.js를 머스테치 파일이 사용하도록 셋팅

+ `footer.mustache`에 코드 추가
  + index.js 호출 코드를 보면 **절대 경로**(/)로 바로 시작합니다.
  + **스프링 부트는 기본적으로 `src/main/resources/static`에 위치한 JS, CSS, 이미지 등 정적 파일들은 URL에서 `/`로 설정됩니다.**

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js">
</script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js">
</script>


<!--index.js 추가-->
<script src="/js/app/index.js"></script>
</body>
</html>
```

---

## 6. 테스트

+ 브라우저에서 직접 테스트를 해봅니다.
+ `localhost:8080/h2-console`에 접속해서 실제로 DB에 데이터가 등록되었는지도 확인해봅니다.

![Desktop View](/assets/img/post/spring-boot2/2021-03-11-db-result.png)


---

+ 출처
  + 이동욱, 『스프링 부트와 AWS로 혼자 구현하는 웹 서비스』, 프리렉(2019) 
