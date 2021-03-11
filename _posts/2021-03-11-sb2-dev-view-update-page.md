---
title: sb2) 17. [화면개발] 게시글 수정 화면 만들기 
author: cotchan 
date: 2021-03-11 23:59:21 +0800 
categories: [Spring-Boot2, Spring-Boot2_DEV]
tags: [spring-boot2] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 게시글 수정 API 만들기

+ 예전에 이미 만들어두었습니다.

+ `PostsApiController`

```java
@RequiredArgsConstructor
@RestController
public class PostsApiController {

    private final PostsService postsService;

    //...

    @PutMapping("/api/v1/posts/{id}")
    public Long update(@PathVariable Long id, @RequestBody PostsUpdateRequestDto requestDto) {
        return postsService.update(id, requestDto);
    }

}
```

---

## 2. 게시글 수정 화면 만들기

+ `posts-update.mustache`
  + [posts-update.mustache 코드](https://gist.github.com/cotchan/b5f85008b1ff5c68e9cce2a823a2a251)

+ 게시글 수정 화면 머스테치 파일을 생성합니다.

---

## 3. index.js 수정(update function 추가)

+ `index.js`
  + **`btn-update` 버튼을 클릭하면 update 기능을 호출할 수 있도록 update function 추가**

```javascript
//index.js
var main = {
    init: function () {
        var _this = this;
        $('#btn-save').on('click', function () {
            _this.save();
        });

        //...

        $('#btn-update').on('click', function () {
            _this.update();
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
    },

    update: function () {
        var data = {
            title: $('#title').val(),
            content: $('#content').val()
        };

        var id = $('#id').val();

        $.ajax({
            type: 'PUT',
            url: '/api/v1/posts/' + id,
            dataType: 'json',
            contentType: 'application/json; charset=utf-8',
            data: JSON.stringify(data)
        }).done(function () {
            alert('글이 수정되었습니다.');
            window.location.href = '/';
        }).fail(function (error) {
            alert(JSON.stringify(error));
        });
    }

};

main.init();
```

---

## 4. 수정 페이지로 이동할 수 있도록 만들기

+ `index.mustache`
  + [index.mustache 코드](https://gist.github.com/cotchan/fe0272b6411989ab4b1c4942ab62fd8b)

+ 전체 목록에서 **수정 페이지로 이동할 수 있도록** 페이지 이동 기능을 추가합니다.

---

## 5. IndexController에 Update 처리 메소드 추가

+ 수정 화면을 연결할 Controller 코드 작업

```java
@RequiredArgsConstructor
@Controller
public class IndexController {

    //...
    
    @GetMapping("/posts/update/{id}")
    public String postsUpdate(@PathVariable Long id, Model model) {
        PostsResponseDto dto = postsService.findById(id);
        model.addAttribute("post", dto);
        return "posts-update";
    }
}
```

---

## 6. 완성된 화면

+ **게시글 수정 화면**

![Desktop View](/assets/img/post/spring-boot2/2021-03-12-update-page.png)

---

+ **수정 기능이 반영된 메인 화면**

![Desktop View](/assets/img/post/spring-boot2/2021-03-12-updated-main-view.png)




---

+ 출처
  + 이동욱, 『스프링 부트와 AWS로 혼자 구현하는 웹 서비스』, 프리렉(2019) 
