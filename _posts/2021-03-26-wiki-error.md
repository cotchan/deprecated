---
title: Wiki) 겼었던 Error 모음 
author: cotchan
date: 2021-03-26 11:32:00 +0800
categories: [Wiki, Wiki_Error]
tags: [error]   
---

+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. SVN

+ **문제**
  + `.a` (정적 라이브러리) 파일이 svn 원격 저장소에 포함되지 않았던 현상**

+ 원인
  + svn의 default .ignore 설정에 `.a` 파일이 포함되어 있기 때문
  + [svn *a 라이브러리 파일 import 하는 방법](https://gods2000.tistory.com/entry/svn-a-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC-%ED%8C%8C%EC%9D%BC-import-%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95)  

+ 해결방법
  + 나의 경우는 `.subverion` 디렉토리가 없었으므로 .subverion 설정값을 바꿔줄 필요는 없었음
  + Cornerstone => Preferecne => Subversion => GLOBAL IGNORES 설정에서 **`.a` 파일을 ignore 옵션에서 제외**하여 해결







---

+ 출처
  + []()
