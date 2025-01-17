---
layout: post
title: Spring 어노테이션
date: 2023-06-09
category: Spring
---


# 1. Annotation이란?


> JAVA에서 Annotation 이라는 기능이 있습니다. 사전상으로는 주석의 의미이지만 Java 에서는 주석 이상의 기능을 가지고 있습니다. Annotation은 자바 소스 코드에 추가하여 사용> 할 수 있는 메타데이터의 일종입니다. 소스코드에 추가하면 단순 주석의 기능을 하는 것이 아니라 특별한 기능을 사용할 수 있습니다.


nnotation은 클래스와 메서드에 추가하여 다양한 기능을 부여하는 역할을 합니다. Annotation을 활용하여 Spring Framework는 해당 클래스가 어떤 역할인지 정하기도 하고, Bean을 주입하기도 하며, 자동으로 getter나 setter를 생성하기도 합니다. 특별한 의미를 부여하거나 기능을 부여하는 등 다양한 역할을 수행할 수 있습니다.


이러한 Annotation을 통하여 코드량이 감소하고 유지보수하기 쉬우며, 생산성이 증가됩니다. **이번 포스팅에서는 **Spring에서 사용하는 많은 Annotation에 대하여 정리해보려고 합니다.**



# 2. Spring의 대표적인 Annotation과 역할


## @Component

개발자가 생성한 Class를 Spring의 Bean으로 등록할 때 사용하는 Annotation입니다. Spring은 해당 Annotation을 보고 Spring의 Bean으로 등록합니다.

```java
@Component(value="myman")
public class Man {
    public Man() {
        System.out.println("hi");
    }
}
```


## @ComponentScan

Spring Framework는 @Component, @Service, @Repository, @Controller, @Configuration 중 1개라도 등록된 클래스를 찾으면, Context에 bean으로 등록합니다. @ComponentScan Annotation이 있는 클래스의 하위 Bean을 등록 될 클래스들을 스캔하여 Bean으로 등록해줍니다.

## @Bean

@Bean Annotation은 개발자가 제어가 불가능한 외부 라이브러리와 같은 것들을 Bean으로 만들 때 사용합니다.

## @Controller
Spring에게 해당 Class가 Controller의 역할을 한다고 명시하기 위해 사용하는 Annotation입니다.


```java
@Controller                   // 이 Class는 Controller 역할을 합니다
@RequestMapping("/user")      // 이 Class는 /user로 들어오는 요청을 모두 처리합니다.
public class UserController {
    @RequestMapping(method = RequestMethod.GET)
    public String getUser(Model model) {
        //  GET method, /user 요청을 처리
    }
}
```


## @RequestHeader
Request의 header값을 가져올 수 있으며, 해당 Annotation을 쓴 메소드의 파라미터에 사용합니다.

```java
@Controller                   // 이 Class는 Controller 역할을 합니다
@RequestMapping("/user")      // 이 Class는 /user로 들어오는 요청을 모두 처리합니다.
public class UserController {
    @RequestMapping(method = RequestMethod.GET)
    public String getUser(@RequestHeader(value="Accept-Language") String acceptLanguage) {
        //  GET method, /user 요청을 처리
    }
}
```


## @RequestMapping

@RequestMapping(value=”“)와 같은 형태로 작성하며, 요청 들어온 URI의 요청과 Annotation value 값이 일치하면 해당 클래스나 메소드가 실행됩니다. Controller 객체 안의 메서드와 클래스에 적용 가능하며, 아래와 같이 사용합니다.

Class 단위에 사용하면 하위 메소드에 모두 적용됩니다.
메소드에 적용되면 해당 메소드에서 지정한 방식으로 URI를 처리합니다.

```java
@Controller                   // 이 Class는 Controller 역할을 합니다
@RequestMapping("/user")      // 이 Class는 /user로 들어오는 요청을 모두 처리합니다.
public class UserController {
    @RequestMapping(method = RequestMethod.GET)
    public String getUser(Model model) {
        //  GET method, /user 요청을 처리
    }
    @RequestMapping(method = RequestMethod.POST)
    public String addUser(Model model) {
        //  POST method, /user 요청을 처리
    }
    @RequestMapping(value = "/info", method = RequestMethod.GET)
    public String addUser(Model model) {
        //  GET method, /user/info 요청을 처리
    }
}
```


## @RequestParam

URL에 전달되는 파라미터를 메소드의 인자와 매칭시켜, 파라미터를 받아서 처리할 수 있는 Annotation으로 아래와 같이 사용합니다. Json 형식의 Body를 MessageConverter를 통해 Java 객체로 변환시킵니다.

```java
@Controller                   // 이 Class는 Controller 역할을 합니다
@RequestMapping("/user")      // 이 Class는 /user로 들어오는 요청을 모두 처리합니다.
public class UserController {
    @RequestMapping(method = RequestMethod.GET)
    public String getUser(@RequestParam String nickname, @RequestParam(name="old") String age {
        // GET method, /user 요청을 처리
        // https://naver.com?nickname=dog&old=10
        String sub = nickname + "_" + age;
        ...
    }
}
```


## @RequestBody

Body에 전달되는 데이터를 메소드의 인자와 매칭시켜, 데이터를 받아서 처리할 수 있는 Annotation으로 아래와 같이 사용합니다. 클라이언트가 보내는 HTTP 요청 본문(JSON 및 XML 등)을 Java 오브젝트로 변환합니다. 아래와 같이 사용합니다.

클라이언트가 body에 json or xml 과 같은 형태로 형태로 값(주로 객체)를 전송하면, 해당 내용을 Java Object로 변환합니다.

```java
@Controller                   // 이 Class는 Controller 역할을 합니다
@RequestMapping("/user")      // 이 Class는 /user로 들어오는 요청을 모두 처리합니다.
public class UserController {
    @RequestMapping(method = RequestMethod.POST)
    public String addUser(@RequestBody User user) {
        //  POST method, /user 요청을 처리
        String sub_name = user.name;
        String sub_old = user.old;
    }
}
```


## @ModelAttribute

클라이언트가 전송하는 HTTP parameter, Body 내용을 Setter 함수를 통해 1:1로 객체에 데이터를 연결(바인딩)합니다. RequestBody와 다르게 HTTP Body 내용은 multipart/form-data 형태를 요구합니다. @RequestBody가 json을 받는 것과 달리 @ModenAttribute 의 경우에는 json을 받아 처리할 수 없습니다.

## @ResponseBody

@ResponseBody은 메소드에서 리턴되는 값이 View 로 출력되지 않고 HTTP Response Body에 직접 쓰여지게 됩니다. return 시에 json, xml과 같은 데이터를 return 합니다.

```java
@Controller                   // 이 Class는 Controller 역할을 합니다
@RequestMapping("/user")      // 이 Class는 /user로 들어오는 요청을 모두 처리합니다.
public class UserController {
    @RequestMapping(method = RequestMethod.GET)
    @ResponseBody
    public String getUser(@RequestParam String nickname, @RequestParam(name="old") String age {
        // GET method, /user 요청을 처리
        // https://naver.com?nickname=dog&old=10
        User user = new User();
        user.setName(nickname);
        user.setAge(age);
        return user;
    }
}
```


## @Autowired

Spring Framework에서 Bean 객체를 주입받기 위한 방법은 크게 아래의 3가지가 있습니다. Bean을 주입받기 위하여 @Autowired 를 사용합니다. Spring Framework가 Class를 보고 Type에 맞게(Type을 먼저 확인 후, 없으면 Name 확인) Bean을 주입합니다.

* @Autowired
* 생성자 (@AllArgsConstructor 사용)
* setter


## @GetMapping

RequestMapping(Method=RequestMethod.GET)과 똑같은 역할을 하며, 아래와 같이 사용합니다.

```java
@Controller                   // 이 Class는 Controller 역할을 합니다
@RequestMapping("/user")      // 이 Class는 /user로 들어오는 요청을 모두 처리합니다.
public class UserController {
    @GetMapping("/")
    public String getUser(Model model) {
        //  GET method, /user 요청을 처리
    }
    
    ////////////////////////////////////
    // 위와 아래 메소드는 동일하게 동작합니다. //
    ////////////////////////////////////

    @RequestMapping(method = RequestMethod.GET)
    public String getUser(Model model) {
        //  GET method, /user 요청을 처리
    }
}
```


## @PostMapping

RequestMapping(Method=RequestMethod.POST)과 똑같은 역할을 하며, 아래와 같이 사용합니다.

```java
@Controller                   // 이 Class는 Controller 역할을 합니다
@RequestMapping("/user")      // 이 Class는 /user로 들어오는 요청을 모두 처리합니다.
public class UserController {
    @RequestMapping(method = RequestMethod.POST)
    public String addUser(Model model) {
        //  POST method, /user 요청을 처리
    }

    ////////////////////////////////////
    // 위와 아래 메소드는 동일하게 동작합니다. //
    ////////////////////////////////////

    @PostMapping('/')
    public String addUser(Model model) {
        //  POST method, /user 요청을 처리
    }
}
```


## @SpringBootTest

Spring Boot Test에 필요한 의존성을 제공해줍니다.

```java
// DemoApplicationTests.java
package com.example.demo;

import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
class DemoApplicationTests {

	@Test
	void contextLoads() {

	}

}
```


## @Test

JUnit에서 테스트 할 대상을 표시합니다.


# 3. Lombok의 대표적인 Annotation과 역할



Lombok은 코드를 크게 줄여주어 가독성을 크게 높힐 수 있는 라이브러리입니다. 대표적인 Annotation은 아래와 같습니다.

## @Setter

Class 모든 필드의 Setter method를 생성해줍니다.

## @Getter

Class 모든 필드의 Getter method를 생성해줍니다.

## @AllArgsConstructor

Class 모든 필드 값을 파라미터로 받는 생성자를 추가합니다.

## @NoArgsConstructor

Class 기본 생성자를 자동으로 추가해줍니다.

## @ToString

Class 모든 필드의 toString method를 생성한다.




출처 : <https://melonicedlatte.com/2021/07/18/182600.html>
