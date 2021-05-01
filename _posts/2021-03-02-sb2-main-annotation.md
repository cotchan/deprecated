---
title: sb2) 주요 @어노테이션@
author: cotchan 
date: 2021-03-03 00:25:21 +0800 
categories: [Spring-Boot2, Spring-Boot2_Main]
tags: [dev_annotation] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **계속 업데이트할 예정입니다.**

---

## @Autowired

+ 스프링이 관리하는 빈(Bean)을 주입받습니다.

---

## @Builder

+ 해당 클래스의 빌더 패턴 클래스 생성
+ 생성자 상단에 선언 시 생성자에 포함된 필드만 빌더에 포함됩니다.

---

## @Column

+ 테이블의 컬럼을 나타내며 굳이 선언하지 않더라도 해당 클래스의 필드는 모두 컬럼이 됩니다.
+ 사용하는 이유는, 기본값 외에 추가로 변경이 필요한 옵션이 있으면 사용합니다.
  + 문자열의 경우 VARCHAR(255)가 기본값인데, 사이즈를 500으로 늘리고 싶은 경우
  + 타입을 TEXT로 변경하고 싶은 경우 등에 사용합니다.

---

## @Entity

+ 테이블과 링크될 클래스임을 나타냅니다.
+ 기본값으로 클래스의 카멜케이스 이름을 언더스코어 네이밍(_)으로 테이블 이름을 매핑합니다.
  + SalesManager.java => sales_manager table

---

## @GeneratedValue

+ PK 생성 규칙을 나타냅니다.
+ 스프링 부트 2.0에서는 GenerationType.IDENTITY 옵션을 추가해야만 auto_increment가 됩니다.

---

## @GetMapping

+ HTTP Method `Get` 요청을 받을 수 있는 API를 만들어 줍니다.
+ 예전: `@RequestMapping(method = RequestMethod.GET)`

---

## @Getter

+ 선언된 모든 필드의 get 메소드를 생성해 줍니다.

---

## @Id

+ 해당 테이블의 PK 필드를 나타냅니다.

---

## @PathVariable

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

## @RequestParam

+ `/read?no=1`와 같이 url이 전달될 때 `no` 파라메터를 받아오게 됩니다.

```java
@GetMapping("read")
public ModelAndView getFactoryRead(@RequestParam("no") int factroyId, SearchCriteria criteria) 
{
  //...    
}
```

---

+ **외부에서 API로 넘긴 파라미터를 가져오는 어노테이션입니다.**
+ 여기서는 외부에서 name (@RequestParam("name")) 이란 이름으로 넘긴 파라미터를 메소드 파라미터 name(String name)에 저장하게 됩니다.

```java
@RestController
public class HelloController {

    @GetMapping("/hello/dto")
    public HelloResponseDto helloDto(@RequestParam("name") String name,
                                     @RequestParam("amount") int amount) {
        return new HelloResponseDto(name, amount);
    }
}
```

---

## @RequiredArgsConstructor

+ 선언된 모든 `final` 필드가 포함된 생성자를 생성해 줍니다.
+ **final이 없는 필드는 생성자에 포함되지 않습니다.**

---

## @RestController

+ **JSON을 반환하는 컨트롤러를 만들어줍니다.**
+ 예전에는 `@ResponseBody`를 각 메소드마다 선언했던 것을 한 번에 사용할 수 있게 해준다고 보면 됩니다.

---

## @SpringBootApplication

+ 이 어노테이션을 사용하면
+ **스프링 부트의 자동 설정, 스프링 Bean 읽기와 생성이 모두 자동으로 설정됩니다.**
+ **또한 이 어노테이션이 있는 위치부터 설정을 읽어나갑니다.**
  + 그러므로 `항상 프로젝트 최상단에 위치`해야만 합니다.

---

## @Transactional

+ 스프링에서 어노테이션으로 **트랜잭션 처리를 지원하는 방식입니다.**
+ 클래스, 메서드 위에 `@Transactional`이 추가되면, 이 클래스에 트랜잭션 기능이 적용된 프록시 객체가 생성됩니다.
+ 이 프록시 객체는 `@Transactional`이 포함된 메소드가 호출 될 경우 트랜잭션을 시작하고, 정상 여부에 따라 `Commit` 또는 `Rollback` 합니다.



---

+ 출처
  + 이동욱, 『스프링 부트와 AWS로 혼자 구현하는 웹 서비스』, 프리렉(2019) 
  + [[Spring] Transactional 정리 및 예제](https://goddaehee.tistory.com/167)
