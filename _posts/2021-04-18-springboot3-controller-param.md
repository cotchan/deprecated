---
title: sb3) @RequestParam, @PathVariable 
author: cotchan 
date: 2021-04-18 03:43:21 +0800 
categories: []
tags: [spring-boot3] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ 아래 출처 글을 참고하여 작성하였습니다.

---

## 1. @PathVariable

```java
@PostMapping("delete/{idx}")
@ResponseBody
public JsonResultVo postDeleteFactory(@PathVariable("idx") int factoryIdx) {
	return factoryService.deleteFacotryData(factoryIdx);
}
```


---

```java
@RestController
@RequestMapping("/api/users")
public class UsersApiController {

    private final UsersService usersService;

    @Autowired
    public UsersApiController(UsersService usersService) {
        this.usersService = usersService;
    }

    @GetMapping(value = "")
    public Result findAll() {
    }

    @GetMapping(value = "/{id}")
    public UsersResponseDto findById(@PathVariable("id") Long userId) {
    }

    @PostMapping("/join")
    public UsersJoinResponseDto save(@RequestBody UsersJoinRequestDto requestDto) {
    }

}
```

---

## 2. @RequestParam

+ `/read?no=1`와 같이 url이 전달될 때 `no` 파라메터를 받아오게 됩니다.

```java
@GetMapping("read")
public ModelAndView getFactoryRead(@RequestParam("no") int factroyId, SearchCriteria criteria) 
{
  //...    
}
```


---

+ 출처
  + [Spring에서 @RequestParam과 @PathVariable](https://elfinlas.github.io/2018/02/18/spring-parameter/)
