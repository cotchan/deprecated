---
title: dart) 빌더 패턴 사용 방법 
author: cotchan
date: 2021-04-08 22:00:00 +0800
categories: [Dart]
tags: [dart]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. 사용 예시1

---

## 1-1. pizza.dart

```dart
/// pizza.dart

class Pizza {
  final String sauce;
  final List<String> toppings;
  final bool hasExtraCheese;

  Pizza._builder(PizzaBuilder builder) : 
    sauce = builder.sauce,
    toppings = builder.toppings,
    hasExtraCheese = builder.hasExtraCheese;
}

class PizzaBuilder {
  static const String neededTopping = 'cheese';

  final String sauce;

  PizzaBuilder(this.sauce);

  List<String> toppings;
  bool hasExtraCheese;

  void setToppings(List<String> toppings) {
    if (!toppings.contains(neededTopping)) {
      throw 'Really, without $neededTopping? :(';
    }

    this.toppings = toppings;
  }

  Pizza build() {
    return Pizza._builder(this);
  }
}
```

---

## 1-2. test.dart

```dart
/// test.dart
print('___PIZZA BBQ___');

Pizza pizza = (
  PizzaBuilder('bbq')
    ..setToppings(['tomato', 'cheese', 'chicken'])
    ..hasExtraCheese = true
  ).build();

print(pizza.sauce);           // bbq
print(pizza.toppings);        // [onion, cheese, chicken]
print(pizza.hasExtraCheese);  // true
```

---

## 2. 사용 예시2

---

## 2-1. LoginViewDto

```dart
/**
 * 로그인 전용 DTO
 * LoginView에서 입력받는 데이터 DTO Class
 */
class LoginViewDto {

  String userId;
  String password;

  bool isHttps;
  String url;
  int port;

  LoginViewDto(this.userId, this.password, this.isHttps, this.url, [this.port]);

  LoginRequestDto toLoginRequestDto() {
    return (LoginRequestDtoBuilder(this.isHttps, this.url, this.port)
              ..setHost()
              ..setUserId = this.userId
              ..setPassword = this.password
            ).build();
  }
}
```

---

## 2-2. LoginRequestDto

```dart
/**
 * 로그인 전용 DTO
 * 서버에 요청할 때 필요한 request 데이터 DTO Class
 */
class LoginRequestDto {

  final String host;
  final String userId;
  final String password;

  LoginRequestDto._builder(LoginRequestDtoBuilder builder) :
      host = builder.host,
      userId = builder.userId,
      password = builder.password;
}
```

---

## 2-3. LoginRequestDtoBuilder

```dart
class LoginRequestDtoBuilder {

  //LoginViewDto
  String url;
  bool isHttps;
  int port;

  //LoginRequestDto
  String _host;
  String _userId;
  String _password;

  /**
   * Constructor
   */
  LoginRequestDtoBuilder(this.isHttps, this.url, [this.port]);

  /**
   * setter // getter
   */
  void setHost() {
    StringBuffer stringBuffer = new StringBuffer();
    this._host = stringBuffer.toString();
  }

  set setUserId(String userId) {
    this._userId = userId;
  }

  set setPassword(String password) {
    this._password = password;
  }

  String get userId => _userId;
  String get host => _host;
  String get password => _password;

  /**
   * build() method
   */
  LoginRequestDto build() {
    return LoginRequestDto._builder(this);
  }
}
```

---

## 2-4. test.dart

```dart
void main() {

  test('LoginRequestDtoBuilder 생성 테스트', () {

    String expectedHost = 'https://www.naver.com';

    /**
     * param: ID, Password, isHttps, url, port
     */
    LoginViewDto loginViewDto = LoginViewDto('username', 'userpassword', true, 'www.naver.com');
    LoginRequestDto loginRequestDto = (LoginRequestDtoBuilder(loginViewDto.isHttps, loginViewDto.url)
      ..setHost()
      ..setUserId = loginViewDto.userId
      ..setPassword = loginViewDto.password
    ).build();

    expect(loginViewDto.userId, equals(loginRequestDto.userId));
    expect(loginViewDto.password, equals(loginRequestDto.password));
    expect(expectedHost, equals(loginRequestDto.host));
  });
}
```

---

+ 출처
  + [The Builder Pattern in Dart](https://dev.to/inakiarroyo/the-builder-pattern-in-dart-efg)

