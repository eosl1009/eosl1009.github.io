---
layout: post
title: Spring 세팅2
date: 2023-05-30
category: Spring
---


일단 context-*.xml을 살펴보기 전에 완성된 (또는 앞으로 완성할 )프로젝트 4_1의 파일 구성을 살펴보자. 


![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/34bc05fe-c6c1-4366-be2e-3a5062b9e458)



이런 파일구성하에서 원할하게 동작하도록 context-*.xml과 mvc-servlet.xml을 작성해야 할 것이다.

 

 

 

이전 글에서 spring 빈 설정은 다음과 같이 한다 했다.

ApplicationContext(ContextLoaderListener)설정파일은 context-*.xml 이다. 여기서 Service,Dao빈들을 등록해준다.

WebApplicationContext(DispatcherServlet)설정파일은 mvc-servlet.xml이다. 여기서 Controller 빈들을 등록해준다.

 

context-main.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xmlns:mybatis-spring="http://mybatis.org/schema/mybatis-spring"
	xmlns:p="http://www.springframework.org/schema/p"
	xmlns:util="http://www.springframework.org/schema/util"
	xmlns:c="http://www.springframework.org/schema/c"
	xsi:schemaLocation="http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.3.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd
		http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.3.xsd
		http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.3.xsd
		http://mybatis.org/schema/mybatis-spring http://mybatis.org/schema/mybatis-spring-1.2.xsd
		http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd">

	<!-- Root Context: defines shared resources visible to all other web components -->

	<!-- ContextLoaderListenr, 프로젝트 전체에서 공유되는 빈들 관리 Buisness, Persistence layer의 
		빈들을 가지고 있도록 설정 =>@Controller 제외하고 전부입니다. -->
	<context:component-scan base-package="com.study"
		use-default-filters="true">
		<context:exclude-filter type="annotation"
			expression="org.springframework.stereotype.Controller" />
	</context:component-scan>

	<!-- properties파일 읽은 프로퍼티 객체, DB말고도 여러가지 작성할 거라 main에 빈등록 -->
	<util:properties id="util"
		location="/WEB-INF/classes/spring/appconfig.properties" />


	<!-- ContextLoaderListenr 빈 중 DB관련 빈들 등록 -->
	<!-- pdf 8페이지 , commons.dbcp대신 commons.dbcp2, name=" driverClassName " 띄어쓰기 
		주의 -->
	<bean id="dataSource"
		class="org.apache.commons.dbcp2.BasicDataSource"
		destroy-method="close">
		<property name="driverClassName"
			value="#{util['jdbc.driverClassName']}" />
		<property name="url" value="#{util['jdbc.url']}" />
		<property name="username" value="#{util['jdbc.username']}" />
		<property name="password" value="#{util['jdbc.password']}" />
		<property name="defaultAutoCommit"
			value="#{util['jdbc.defaultAutoCommit']}" />


	</bean>
	<!-- SqlSession setup for MyBatis Database Layer -->
	<bean id="sqlSessionFactory"
		class="org.mybatis.spring.SqlSessionFactoryBean">
		<property name="dataSource" ref="dataSource" />
		<property name="configLocation"
			value="/WEB-INF/classes/mybatis/mybatis-config.xml" />
		<property name="mapperLocations"
			value="/WEB-INF/classes/mybatis/mapper/*.xml" />
	</bean>
	<mybatis-spring:scan base-package="com.study"
		annotation="org.apache.ibatis.annotations.Mapper" />

</beans>
```

여기에 이제 Service,Dao단의 빈들을  등록하려고한다. 사실 Service,Dao단 뿐만아니라 

Spring에서 제공하는 여러기능들에 관한 빈들은 대부분 context-*.xml에 등록한다.  

여기에 등록된 빈들은 나중에 프로젝트가 확장 돼 Servlet이 늘어나거나 다른 프레임워크가 적용이 되도 사용가능하다.

 

그래도 일단 지금 당장은 게시판을 만드는데 필요한 bean들을 살펴보자.

context-*.xml의 namespace
![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/65dc6cf8-1b89-418c-be9c-9e5e77a912f3)

우선 xml에서 여러 태그를 사용할 수 있도록 namespace에서 관련 내용들을 체크해줘야 한다.

나중을 위해서 모두 체크해줘도 상관없다.

# \<context:component-scan\>

먼저 \<context:component-scan\> 태그를 보자. 

```xml
	<!-- ContextLoaderListenr, 프로젝트 전체에서 공유되는 빈들 관리 Buisness, Persistence layer의 
		빈들을 가지고 있도록 설정 =>@Controller 제외하고 전부입니다. -->
	<context:component-scan base-package="com.study"
		use-default-filters="true">
		<context:exclude-filter type="annotation"
			expression="org.springframework.stereotype.Controller" />
	</context:component-scan>
```
 base-package를 보면  "com.study"라고 되어있다. com.study 하위 패키지를 전부 스캔해서 특정 @이 붙어있는

클래스들은 빈 등록을 하겠다는 의미이다. 이는 DI에서 설명한 내용이다.

<context:component-scan>이 스캔 후 모든 @을 등록하는 건 아니다.   

이 태그가 등록하는 @은  @Repostioy,@Component,@Service,@Controller 등이 있다. 

(이 @들은 전부 @Component가 있다. 그래서 Component scan이다)
![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/b94f959a-a7f5-4958-b0ed-3cdfee834509)
  
use-default-filters="true" 를 보자. 이는 위에서 말한 @들을 스캔하겠다는 의미이다. 

그리고  <context:execlude-filter>를 보면 특정 @을 제외할 수 있다.  expression을 보면 Controller를 제외하고 있다.

즉, 위에서 작성한 <context:component-scan>은  com.study 및 그 하위 패키지를 스캔하면서, @Controller을 제외한

@Repositroy,@Component,@Service이 붙어있는 클래스에 대해 빈 등록을 한다.

 

 

# \<util\: properties\>
\<util:properties id="util"  location="/WEB-INF/classes/spring/appconfig.properties"/\>

이 태그는 properties파일을 읽어서 그 정보를 가지고있는 빈을 등록해주는 태그이다.

appconfig.properties의 내용은 다음과 같다.

```
# appconfig.properties
# file encoding is UTF-8

# JDBC information
jdbc.driverClassName=oracle.jdbc.driver.OracleDriver
jdbc.url=jdbc:oracle:thin:@localhost:1521:xe
jdbc.username=jsp
jdbc.password=oracle
jdbc.defaultAutoCommit=true
jdbc.maxTotal=4
jdbc.minIdle=4
jdbc.validationQuery=SELECT 1 FROM dual
```

현재는 jdbc와 관련된 내용만 있지만 앞으로 여러가지 설정 내용이 추가 될 것이다.

 

 

 

DataSource와 Mybatis 관련빈, 그리고 \<mybatis-spring:scan\>

```xml
<bean id="dataSource"
		class="org.apache.commons.dbcp2.BasicDataSource"
		destroy-method="close">
		<property name="driverClassName"
			value="#{util['jdbc.driverClassName']}" />
		<property name="url"
			value="#{util['jdbc.url']}" />
		<property name="username" value="#{util['jdbc.username']}" />
		<property name="password" value="#{util['jdbc.password']}" />
		<property name="defaultAutoCommit" value="#{util['jdbc.defaultAutoCommit']}" />
		
		
	</bean>
	<!-- SqlSession setup for MyBatis Database Layer -->
	<bean id="sqlSessionFactory"
		class="org.mybatis.spring.SqlSessionFactoryBean">
		<property name="dataSource" ref="dataSource" />
		<property name="configLocation"
			value="/WEB-INF/classes/mybatis/mybatis-config.xml" />
		<property name="mapperLocations"
			value="/WEB-INF/classes/mybatis/mapper/*.xml" />
	</bean>
	<mybatis-spring:scan base-package="com.study" 
	 annotation="org.apache.ibatis.annotations.Mapper" />
```

DataSource는 DB에 실제로 연결하는 빈이다. 그렇기 때문에 Driver, url, username,password 등이 property로 있다.

그리고 이 값들을 직접 입력해도 되지만 appconfig.properties에 있는 값을 읽어서  세팅하도록 했다.

또, SqlSessionFactoryBean에 경우  실제 연결할 DB에 대한정보(dataSource),  mybatis 설정파일, mybatis mapper파일에

대한 정보를 가지고 있으면 된다.

여기까지 설정방법에 대한 자세한 내용은 http://mybatis.org/spring/ko/factorybean.html를 참고하자.

 

마지막으로 \<mybatis-spring:scan\>은  com.study 및 그 하위 패키지를 스캔하면서

\@Mapper가 붙은 인터페이스의  객체를 빈 등록한다.

 

# 결과
ApplicationContext는 \@Repostiory, \@Component, \@Service, \@Mapper가 붙은 빈을 가지고 있게 된다.

(사실 study4_1에서 \@Repostirory,\@Component는 사용x,    실제론 \@Service, \@Mapper만 빈을 가지고 있는다)
![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/e793435f-3baf-4c14-b175-2fc04dff8343)

※ 참고. context-\*.xml 파일 나누기

이렇게 @Controller을 제외한 대부분의 빈은 ApplicationContext에 등록한다.

이 때문에 지금까지는 context-main.xml에  \<bean\>태그나 scan을 써서 빈을 등록하도록 했다.

하지만 여러가지 기능을 추가하면서 여러 빈들을 추가해야하는 상황이 오면 context-main.xml이 길어지게 될 것이다. 

이를 해결하려면  빈을 등록할 때 빈이 하는 기능에 따라 따로 context-000.xml을 만들어서 그 파일에 빈을 등록한다.

예를 들어 다음과 같이 context-datasource.xml을 새로 만들자.
![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/270ce419-fdfc-4f8c-a471-a6dc461f55ba)

이 때 datasource에는 DB관련 내용 빈들을 작성해주자.  그리고 context-main.xml에서는 그 내용들을 지워주자.

context-main.xml
  
```xml
  <?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xmlns:mybatis-spring="http://mybatis.org/schema/mybatis-spring"
	xmlns:p="http://www.springframework.org/schema/p"
	xmlns:util="http://www.springframework.org/schema/util"
	xmlns:c="http://www.springframework.org/schema/c"
	xsi:schemaLocation="http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.3.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd
		http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.3.xsd
		http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.3.xsd
		http://mybatis.org/schema/mybatis-spring http://mybatis.org/schema/mybatis-spring-1.2.xsd
		http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd">

	<!-- Root Context: defines shared resources visible to all other web components -->

	<!-- ContextLoaderListenr, 프로젝트 전체에서 공유되는 빈들 관리 Buisness, Persistence layer의 
		빈들을 가지고 있도록 설정 =>@Controller 제외하고 전부입니다. -->
	<context:component-scan base-package="com.study"
		use-default-filters="true">
		<context:exclude-filter type="annotation"
			expression="org.springframework.stereotype.Controller" />
	</context:component-scan>
	
	<!-- properties파일 읽은 프로퍼티 객체, DB말고도 여러가지 작성할 거라 main에 빈등록 -->
	<util:properties id="util" 
	location="/WEB-INF/classes/spring/appconfig.properties"/>
	

</beans>
```
  
(util은 appconfig를 읽어서 그 내용을 가지고있는 빈인데,  appconfig는 JDBC 관련 내용이 있어서 DB관련 빈처럼보인다.

그러나 나중에 appconfig에 JDBC 뿐 아니라 파일업로드 및 여러가지 내용이 추가돼서 main에 등록하도록 한다)

context-datasource.xml
  
```xml
  <?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:mybatis-spring="http://mybatis.org/schema/mybatis-spring"
	xsi:schemaLocation="http://mybatis.org/schema/mybatis-spring http://mybatis.org/schema/mybatis-spring-1.2.xsd
		http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
<!-- ContextLoaderListenr 빈 중 DB관련 빈들 등록 -->
<!-- pdf 8페이지 , commons.dbcp대신 commons.dbcp2, 
	name=" driverClassName  " 띄어쓰기 주의 -->
	<bean id="dataSource"
		class="org.apache.commons.dbcp2.BasicDataSource"
		destroy-method="close">
		<property name="driverClassName"
			value="#{util['jdbc.driverClassName']}" />
		<property name="url"
			value="#{util['jdbc.url']}" />
		<property name="username" value="#{util['jdbc.username']}" />
		<property name="password" value="#{util['jdbc.password']}" />
		<property name="defaultAutoCommit" value="#{util['jdbc.defaultAutoCommit']}" />
		
		
	</bean>
	<!-- SqlSession setup for MyBatis Database Layer -->
	<bean id="sqlSessionFactory"
		class="org.mybatis.spring.SqlSessionFactoryBean">
		<property name="dataSource" ref="dataSource" />
		<property name="configLocation"
			value="/WEB-INF/classes/mybatis/mybatis-config.xml" />
		<property name="mapperLocations"
			value="/WEB-INF/classes/mybatis/mapper/*.xml" />
	</bean>
	<mybatis-spring:scan base-package="com.study" 
	 annotation="org.apache.ibatis.annotations.Mapper" />	

</beans>
```
  
이런식으로 빈의 하는 역할에 따라 파일을 분리할 수 있다. 이렇게 파일을 분리해도

web.xml에서 
  
```xml
  <context-param>
	<param-name>contextConfigLocation</param-name>
	<param-value>/WEB-INF/classes/spring/context-*.xml</param-value>
 </context-param>
```
  
처럼 설정했기 때문에  context-main.xml이든 context-datasource.xml이든  여기서 작성된 빈들은 모두

ApplicationContext에 등록하게 된다.  즉, 변한건없다. 단지 파일을 나눴을 뿐이다.
