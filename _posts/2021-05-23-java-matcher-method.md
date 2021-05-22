---
title: Java) 정규 표현식 Matcher 메서드 (feat. URL에서 {userId} 추출 유연하게 하는 법)
author: cotchan
date: 2021-05-23 02:18:00 +0800
categories: [Java]
tags: [java]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. Matcher.group(int group)

+ 아래 코드에서 `matcher.group(1)`은 어느 부분을 의미하는 것일까요?
  + **그건 바로 정규표현식에서 `(\\d+)` 부분을 의미합니다.**

```java
  @Bean
  public ConnectionBasedVoter connectionBasedVoter() {
    final String regex = "^/api/user/(\\d+)/post/.*$";
    final Pattern pattern = Pattern.compile(regex);
    RequestMatcher requiresAuthorizationRequestMatcher = new RegexRequestMatcher(pattern.pattern(), null);
    return new ConnectionBasedVoter(
      requiresAuthorizationRequestMatcher,
      (String url) -> {
        /* url에서 targetId를 추출하기 위해 정규식 처리 */
        Matcher matcher = pattern.matcher(url);
        long id = matcher.matches() ? toLong(matcher.group(1), -1) : -1;
        return Id.of(User.class, id);
      }
    );
  }
```

---


---


---

+ 출처
  + [[JAVA] 정규표현식, Matcher 메서드 사용방법과 그룹 개념 이해](https://enterkey.tistory.com/353)
