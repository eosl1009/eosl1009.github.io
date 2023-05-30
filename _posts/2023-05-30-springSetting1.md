---
layout: post
title: 스프링이란?
date: 2023-05-24
category: Spring
---


Spring MVC 프로젝트 만들기 
==========

new-other를 클릭한 후 spring을 검색, spring legecy 프로젝트 선택

![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/2056baed-4013-41ba-a08b-c5d8ec3d196f)

next 선택 후  spring MVC Project 선택.  프로젝트이름은 study4_1
![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/4fd83a59-140a-4b7e-8156-76f89ba9540b)


이 후 project setting 창에서는 패키지를 3단까지 쓰면 된다.  여기서 작성한 패키지에
HomeController가 위치하게 된다. 적당히 com.study.home으로 작성하자.

![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/595f900e-db1d-4471-9ddc-3bd45fb7a36c)
![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/14ccea71-289f-4118-8b6c-442e2f086093)

com.study.home에 HomeController가 있는 것을 확인할 수 있다.

프로젝트 세팅 
=========

1. 프로젝트 properties

spring legecy프로젝트는 기본적으로 빌드를 Maven으로 한다. Maven은 빌드된 파일을 배포할 때 jre가 아닌 jdk를

필요로 한다.  그래서 프로젝트 java build-path에는 jre가 아닌 jdk가 있어야 한다.

프로젝트 우클릭-properties- Java-Build-path에서 기존의 JRE System Library가 jre로 되어있다면 jdk로 바꿔주자.(1.8로)

![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/5e2bee6e-0476-44f9-a1e4-2b0255273a82)
![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/d5581890-b694-45bd-9e35-32543dff73c1)
![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/e486dba2-f50a-433e-8a6f-c3a7f6eedbc4)
![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/91463cf8-d761-4992-88d0-c36a826df945)

![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/2638a0e5-4675-4dc2-b5df-19aee239c609)

이번엔 Web Project settins에서 값을 study4로 바꾸자. 

프로젝트를 서버에 올릴 때의 기본 contextPath 값이다.
![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/84b1860e-69d8-41d8-bb71-1e9331828252)


마지막으론 properties-Project Facets에 가서  

Dynamic Web Module은 3.1,  Java는 1.8, Javascript는 1.0으로 하고 체크해주자.

그리고 오른쪽의 Runtimes에서 Apache Tomcat 9.0 체크하자. 

이상을 다 끝냈으면 Apply and Close를 누러 설정을 저장하자.
![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/ff2af3f8-dc00-45d7-b54f-1a32bd9c05f7)


2. pom.xml 

pom.xml은 메이븐설정으로 메이븐은 pom.xml에 명시 된 dependency가 현재 프로젝트 라이브러리에 있는지

확인한 후 없다면 mvn repository에서 해당 라이브러리를 다운받아 프로젝트 라이브러리에 추가한다.

다음은 기본 게시판을 만드는데 필요한  lib를 위해 pom.xml이다.  만들어진 프로젝트에 복사하자.

pom.xml


```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/maven-v4_0_0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.study</groupId>
	<artifactId>home</artifactId>
	<name>study4_1</name>
	<packaging>war</packaging>
	<version>1.0.0-BUILD-SNAPSHOT</version>
	<properties>
		<java-version>1.8</java-version>
		<org.springframework-version>4.3.28.RELEASE</org.springframework-version>
		<org.aspectj-version>1.9.6</org.aspectj-version>
		<org.slf4j-version>1.7.30</org.slf4j-version>
	</properties>
	<dependencies>
		<!-- Spring -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>${org.springframework-version}</version>
			<exclusions>
				<!-- Exclude Commons Logging in favor of SLF4j -->
				<exclusion>
					<groupId>commons-logging</groupId>
					<artifactId>commons-logging</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>

		<!-- spring ORM : 스프링에서 DB관련 설정하는데 필요한 것들 -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-orm</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>

		<!-- AspectJ -->
		<dependency>
			<groupId>org.aspectj</groupId>
			<artifactId>aspectjrt</artifactId>
			<version>${org.aspectj-version}</version>
		</dependency>
		<!-- aop할려면 jrt+ weaver tools 필요한데 아직 안하니까.. -->
		<dependency>
			<groupId>org.aspectj</groupId>
			<artifactId>aspectjweaver</artifactId>
			<version>1.9.7</version>
		</dependency>


		<!-- Logging -->
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-api</artifactId>
			<version>${org.slf4j-version}</version>
		</dependency>
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>jcl-over-slf4j</artifactId>
			<version>${org.slf4j-version}</version>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-log4j12</artifactId>
			<version>${org.slf4j-version}</version>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>log4j</groupId>
			<artifactId>log4j</artifactId>
			<version>1.2.15</version>
			<exclusions>
				<exclusion>
					<groupId>javax.mail</groupId>
					<artifactId>mail</artifactId>
				</exclusion>
				<exclusion>
					<groupId>javax.jms</groupId>
					<artifactId>jms</artifactId>
				</exclusion>
				<exclusion>
					<groupId>com.sun.jdmk</groupId>
					<artifactId>jmxtools</artifactId>
				</exclusion>
				<exclusion>
					<groupId>com.sun.jmx</groupId>
					<artifactId>jmxri</artifactId>
				</exclusion>
			</exclusions>
			<scope>runtime</scope>
		</dependency>






		<!-- @Inject -->
		<dependency>
			<groupId>javax.inject</groupId>
			<artifactId>javax.inject</artifactId>
			<version>1</version>
		</dependency>

		<!-- Servlet -->
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>javax.servlet-api</artifactId>
			<version>3.1.0</version>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>javax.servlet.jsp</groupId>
			<artifactId>javax.servlet.jsp-api</artifactId>
			<version>2.3.3</version>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jstl</artifactId>
			<version>1.2</version>
		</dependency>


		<!-- Apache 및 사용자가 추가한 거 -->
		<!-- https://mvnrepository.com/artifact/commons-beanutils/commons-beanutils -->
		<dependency>
			<groupId>commons-beanutils</groupId>
			<artifactId>commons-beanutils</artifactId>
			<version>1.9.4</version>
		</dependency>
		<!-- https://mvnrepository.com/artifact/org.apache.commons/commons-collections4 -->
		<dependency>
			<groupId>org.apache.commons</groupId>
			<artifactId>commons-collections4</artifactId>
			<version>4.4</version>
		</dependency>
		<!-- https://mvnrepository.com/artifact/org.apache.commons/commons-dbcp2 -->
		<dependency>
			<groupId>org.apache.commons</groupId>
			<artifactId>commons-dbcp2</artifactId>
			<version>2.7.0</version>
		</dependency>

		<!-- https://mvnrepository.com/artifact/org.apache.commons/commons-lang3 -->
		<dependency>
			<groupId>org.apache.commons</groupId>
			<artifactId>commons-lang3</artifactId>
			<version>3.11</version>
		</dependency>
		<!-- https://mvnrepository.com/artifact/commons-logging/commons-logging -->
		<dependency>
			<groupId>commons-logging</groupId>
			<artifactId>commons-logging</artifactId>
			<version>1.2</version>
		</dependency>
		<!-- https://mvnrepository.com/artifact/org.apache.commons/commons-pool2 -->
		<dependency>
			<groupId>org.apache.commons</groupId>
			<artifactId>commons-pool2</artifactId>
			<version>2.8.1</version>
		</dependency>

		<!-- jstl은 위에 받았으니까 패스 -->

		<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis</artifactId>
			<version>3.5.6</version>
		</dependency>

		<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis-spring</artifactId>
			<version>2.0.6</version>
		</dependency>






		<!-- https://mvnrepository.com/artifact/com.oracle.database.jdbc/ojdbc8 -->
		<dependency>
			<groupId>com.oracle.database.jdbc</groupId>
			<artifactId>ojdbc8</artifactId>
			<version>19.6.0.0</version>
		</dependency>

		<!-- https://mvnrepository.com/artifact/com.oracle.database.nls/orai18n -->
		<dependency>
			<groupId>com.oracle.database.nls</groupId>
			<artifactId>orai18n</artifactId>
			<version>19.6.0.0</version>
		</dependency>



		<!-- Test -->
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.7</version>
			<scope>test</scope>
		</dependency>
	</dependencies>
	<build>
		<plugins>
			<plugin>
				<artifactId>maven-eclipse-plugin</artifactId>
				<version>2.9</version>
				<configuration>
					<additionalProjectnatures>
						<projectnature>org.springframework.ide.eclipse.core.springnature</projectnature>
					</additionalProjectnatures>
					<additionalBuildcommands>
						<buildcommand>org.springframework.ide.eclipse.core.springbuilder</buildcommand>
					</additionalBuildcommands>
					<downloadSources>true</downloadSources>
					<downloadJavadocs>true</downloadJavadocs>
				</configuration>
			</plugin>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>3.8.1</version>
				<configuration>
					<source>${java-version}</source>
					<target>${java-version}</target>
					<compilerArgument>-Xlint:all</compilerArgument>
					<showWarnings>true</showWarnings>
					<showDeprecation>true</showDeprecation>
				</configuration>
			</plugin>
			<plugin>
				<groupId>org.codehaus.mojo</groupId>
				<artifactId>exec-maven-plugin</artifactId>
				<version>1.6.0</version>
				<configuration>
					<mainClass>org.test.int1.Main</mainClass>
				</configuration>
			</plugin>
		</plugins>
	</build>
</project>
```


3. web.xml

Project Facet에서 Dynamic Web module을 3.1로 바꿔줬기 때문에  web.xml의 DTD도 3.1로 바꿔준다. 

Spring 프로젝트가 처리하는 과정에서의 encoding을  전부 UTF-8로 해줄거다.

또한 스프링에서 제공하는 파일들의 설정파일 위치를 /WEB-INF/classes/spring 폴더로 지정하자.

그리고 spring 설정파일의 이름을 바꿀 것이다.  이를 위해 web.xml을 다음과 같이 작성하자.(복사하자)

 

※DTD는 https://araikuma.tistory.com/769 참고.   

web.xml


```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://xmlns.jcp.org/xml/ns/javaee"
	xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
	version="3.1">

	<filter>
		<filter-name>encoding</filter-name>
		<filter-class>org.springframework.web.filter.CharacterEncodingFilter
		</filter-class>
		<init-param>
			<param-name>encoding</param-name>
			<param-value>UTF-8</param-value>
		</init-param>
		<init-param>
			<param-name>forceEncoding</param-name>
			<param-value>true</param-value>
		</init-param>
	</filter>
	<filter-mapping>
		<filter-name>encoding</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>


	<!-- The definition of the Root Spring Container shared by all Servlets 
		and Filters -->
	<!-- 전체 컨테이너 파라미터 -->
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>/WEB-INF/classes/spring/context-*.xml</param-value>
	</context-param>

	<!-- Creates the Spring Container shared by all Servlets and Filters -->
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener
		</listener-class>
	</listener>

	<!-- Processes application requests -->
	<!-- init-param은 이 Servlet안에서만 쓰이는 파라미터 -->
	<servlet>
		<servlet-name>appServlet</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet
		</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>/WEB-INF/classes/spring/mvc-servlet.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>

	<servlet-mapping>
		<servlet-name>appServlet</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>


</web-app>
```

web.xml에서 작성한대로 스프링 설정파일을 위치시켜야 한다.
![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/a7fb81f7-9777-44f0-89c5-a8f9bf2a3c08)

프로젝트 생성 시 처음 생성되는 spring설정파일들이다.   

servlet-context.xml ===>  mvc-servlet.xml로, 

root-context.xml ===> context-main.xml로 변경해준다.
![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/39298058-6dff-4829-960a-a447e65a1c71)

이후 java Resource -  src/main/resources 우클릭 new other  후 folder를 생성해준다. 폴더의 이름은 spring.
![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/ef0a438d-91ea-4dfd-832c-ee52fae840f9)
![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/0b146b38-8233-4c62-8185-7f6525c815f0)
![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/37864523-a229-4757-b641-dc5d21877928)


이 폴더에다가 아까 이름을 변경한 mvc-servlet.xml, context-main.xml을 이동시킨다.

WEB-INF/spring 폴더는 이제 필요없으므로 삭제한다. 

web.xml에서  mvc-servlet.xml(DispatcherServlet 설정파일 .기존 servlet-context.xml)

,context-main.xml(ConteLoaderListner 설정파일 .  기존 root-context.xml)의

이름과 위치를 변경했는데, 거기에 맞춰 파일을 위치시킨 것이다.
![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/e5d3d4ca-3ca1-4bb5-956d-c02a1bcb78b6)

4.   mvc-servlet.xml ,  context-main.xml


mvc-servlet.xml

com.study의 하위패키지를 스캔. @Controller만 찾아서 빈등록하도록 설정,그 외에는 기본값. 

아래 내용을 복사하자. 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans:beans xmlns="http://www.springframework.org/schema/mvc"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:beans="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd
		http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd
		http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.3.xsd">

	<!-- DispatcherServlet Context: defines this servlet's request-processing infrastructure -->
	
	<!-- Enables the Spring MVC @Controller programming model -->
	<!-- 이 태그만 있으면 HandlerMapping을 비롯한 여러가지 mvc에 필요한 설정들이 자동으로 세팅 (기본값)
		annotation-driven태그 안에 내용을 하나하나 다 써도 됩니다. 80줄정도-->
	<annotation-driven />

	<!-- Handles HTTP GET requests for /resources/** by efficiently serving up static resources in the ${webappRoot}/resources directory -->
	<!-- mapping은 url요청,  location은 webapp에서의 위치 -->
	<!--  귀찮으면 폴더이름도 resources, mapping도 resources하면 됨  -->
	<resources mapping="/resources/**" location="/resources/" />

	<!-- Resolves views selected for rendering by @Controllers to .jsp resources in the /WEB-INF/views directory -->
	<beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<beans:property name="prefix" value="/WEB-INF/views/" />
		<beans:property name="suffix" value=".jsp" />
	</beans:bean>
	
	<!-- component-scan은 기본적으로  use-default-filter가 트루 일 때 
	@Controller,@Service,@Respoitory,@Component 등이 붙은 걸 빈으로 다 등록 	 (기본값도 true)
	DS은 Presentation,Controller단  빈만 등록 ->	@Controller만 가지도록 
	  
	-->
	<context:component-scan base-package="com.study"  use-default-filters="false">
		<context:include-filter type="annotation" 
		expression="org.springframework.stereotype.Controller"/>
	</context:component-scan>
</beans:beans>
```

context-main.xml

com.study의 하위패키즈를 스캔해서 @Controller을 제외한 몇 가지 @을 빈으로 등록하자.

또 mybatis 관련 설정도 하자. 이 때 mybatis 설정 값을 직접 입력하지 않고 properties를 이용해

입력하도록 properties에 관한 빈도 추가하자.

아래내용을 복사한다.

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


mybatis 설정값에 관한 appconfig.properties를 만들자.

spring 폴더 우클릭-new-other file 선택 후 이름을 appconfig.properties라고 짓자.
![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/23c58c13-c77b-48df-bf8d-cf08f58e88c3)
![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/45ba92e0-b83e-467c-8598-8ca03cbb46e5)

appconifg를 다음과 같이 작성하자.

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

5. mybatis 파일들

context-main.xml에서 기입한 mybatis 설정대로 mybatis 관련 파일을 만들면 된다.

src/main/resource폴더밑에 mybatis폴더를 만들고 

우클릭 new-other-xml File을 선택한다.  파일이름은 mybatis-config.xml로 한다.
![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/e6f0e263-e765-44f3-a97a-0663b2ae64cf)

mybatis-config.xml 파일 내용은 다음과 같다.

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
<settings>
  <setting name="mapUnderscoreToCamelCase" value="true"/> 
</settings>

</configuration>
```


이번엔 mybatis 폴더 안에 또 mapper라는 폴더를 만들자. 

(이클립스에 따라 폴더안에 폴더가 mybatis.mapper 형태로 보일수도, 

mybatis/mapper 형태로 보일수도 있다. 상관없다. 중요한건 mybatis폴더안에 mapper 폴더를 만드는 거다.)

그리고 그 mapper 폴더 안에  study3_1에서 만들었던 freeBoard.xml, member.xml등을 복사해 넣어주자.
![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/538d79e4-bc5a-4ad7-bc0e-234ece1b1478)


일단 이렇게 하면 설정은 끝났다. DB 및 mvc에 필요한 기본 설정은 다 되었다. 

이제 우리가 할  건 study3_1에서 만들었던 게시판을 spring방식대로 작성하는 것이다. 

그 전에 spring 프로젝트에 관한 전반적인 설명과,  spring mvc에 대한 설명, DB연결방법 등에 대해 설명하고

그 부분이 설정파일 코드에서 어떻게 적용되었는지 살펴보자.



Spring 프로젝트 설정파일들
=========

# 1.pom.xml

pom.xml이 뭔지 알기 위해선 빌드, 빌드 툴(maven)을 먼저 알아야 한다.

 

먼저 빌드란  소스코드를 실행할 수 있는 소프트웨어 형태로 변환하는 과정으로  프로젝트를 위해 작성한

Java코드 및 여러 자원들(.xml, .jar, .properties)을 JVM이나 톰캣같은 WAS가 인식할 수 있는 구조로

패키징 하는 과정 및 결과물이다. 
또 단순히 컴파일해주는 작업 뿐만 아니라, 테스팅, 검사, 배포까지 일련의 작업들을 통틀어 빌드라고 한다.

빌드 단계 중에 컴파일이 포함되어 있다.

 

이런 빌드 과정을 하나하나 직접 개발자가 하기는 상당히 귀찮다.

그래서 빌드 과정을 도와주는 도구들이 있는데 이를 빌드 툴이라 한다.

이러한 빌드 툴중에 spring이 채택한 것이 Maven이다. (spring boot에서는 gradle)

 

Maven은 빌드 툴인데 빌드 하는 기준이 필요할 것이다. 아무렇게나 빌드하지는 않을 것이다. 

Maven은 미리 작성된 xml 파일을 사용하여 필요한 라이브러리를 다운로드하거나, 생성, 프로젝트 빌드 하는 등의

작업을 자동화한다.  이 미리 작성된 xml이 바로 pom.xml이다.

 

 

pom.xml에 여러태그가 있지만 사실 중요한건 <dependencies> 태그의  하위태그인 <dependency>태그이다.

이 <dependency>태그가 하나의 lib라고 생각하면 된다.

Spring 프로젝트에서 Maven은 다음과 같이 동작한다.

pom.xml에 변경사항이 있을 때 마다 pom.xml을 분석한다. 
(Maven은 모든 코드를 분석하겠지만 우리는 dependency태그만 관심있게 보자)
현재 프로제트의 Java Build Path의 Maven Dependencies 라이브러리에  <dependecy> 태그들에서 
지정한 라이브러리가 있는지 확인한다.
없으면 MVN repository에서 해당 jar파일을 다운받아 Maven Dependencies라이브러리에 자동으로 추가한다.
이 때 <dependency>에서 정의한 jar파일 뿐만 아니라 관련  jar파일까지 알아서 다 추가해준다.
즉 우리는 jar파일을 찾은 후 다운받은다음에 프로젝트 라이브러리에 jar파일을 추가 할 필요없이,

pom.xml에 .jar파일의 <dependency>태그만 추가해주면 된다. 

위의 pom.xml은 스프링 mvc 프로젝트에 필요한 기초 lib를 전부 추가한 파일들이다.

dependency를 전부 하나하나 알 필요는 없고, 앞으로 어떤 기능을 배우면서 lib가 필요하다면 

dependency를 추가해주기만 하면 된다.

 

 

※  개발 도중 소스코드 변경사항이 적용이 안될 때 ,  또는 pom.xml을 변경했는데도 lib가 추가가 안될 때
	
1.프로젝트 클린
	
![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/75052edc-d048-4188-b5ad-b3ffe830832c)

해당 프로젝트 컴파일을 다시한다.   

class파일들 지우고 다시 컴파일 및 빌드함. 빌드까지 하기 때문에 파일이 사라지고 다시 생성된다.

범위는 target폴더안에 일부 폴더들이다. m2e-wtp,classes,test-classes폴더들.

대부분의 경우 여기서 해결된다.    

 

------여기서 잠깐.  Clean 밑에 Build Automatically 설정 되어있다는걸 주의-------------

 

 

 

2.  프로젝트 우클릭-Run As- Maven clean , Maven Install
![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/d8bcc480-2839-4a7b-b71b-f8ca631622ba)


Maven clean의 경우 target폴더 전체 지움.  

(위의 Build Automatically에 의해서 지우자마자 Build 돼서 target폴더 생성됨, 체크해재하고 

Maven clean하면 target폴더 사라지는거 확인가능)

 

Maven Install은 target폴더가 없으면  target폴더( 및 그 안의 하위 파일들) 생성, war파일 생성

target폴더(및 그 안의 하위 파일들)가 있으면 war파일만 재생성.

 

즉, 어쨋든 1,2번 방법 둘다 프로젝트의 파일들을 재빌드 한다고 보면 된다. 

(Build Automatically는 체크해놓자. 안 해놓으면 여러분들이 소스코드 변경한 사항들이 자동으로 빌드가 안된다.)

 

이 빌드 관련 적용이 안되는 문제들은 pom.xml뿐만 아니라 프로젝트 전체 소스코드에 관한 문제이지만

어쨋든 pom.xml이나 web.xml처럼 xml파일이 빨간색x 표시가 날 때는 에러를 찾기가 무척어렵다.(대부분 오타1개)

애초에 xml은  반드시 자동완성이나 복붙으로만 작성하도록하자.  

 
	
	
# 2.Web.xml

```xml
	?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://xmlns.jcp.org/xml/ns/javaee"
	xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
	version="3.1">

	<filter>
		<filter-name>encoding</filter-name>
		<filter-class>org.springframework.web.filter.CharacterEncodingFilter
		</filter-class>
		<init-param>
			<param-name>encoding</param-name>
			<param-value>UTF-8</param-value>
		</init-param>
		<init-param>
			<param-name>forceEncoding</param-name>
			<param-value>true</param-value>
		</init-param>
	</filter>
	<filter-mapping>
		<filter-name>encoding</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>


	<!-- The definition of the Root Spring Container shared by all Servlets 
		and Filters -->
	<!-- 전체 컨테이너 파라미터 -->
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>/WEB-INF/classes/spring/context-*.xml</param-value>
	</context-param>

	<!-- Creates the Spring Container shared by all Servlets and Filters -->
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener
		</listener-class>
	</listener>

	<!-- Processes application requests -->
	<!-- init-param은 이 Servlet안에서만 쓰이는 파라미터 -->
	<servlet>
		<servlet-name>appServlet</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet
		</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>/WEB-INF/classes/spring/mvc-servlet.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>

	<servlet-mapping>
		<servlet-name>appServlet</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>


</web-app>
```
	
web.xml의 태그를 보면  크게 3군데로 나눌 수 있다.

<filter>,<filter-mapping>
<context-param>, <listener>
<servlet> , <servlet-mapping>
 

1. filter, filter-mapping

<filter>태그와 그 하위태그를 살펴보면 encoding에 관한 내용이라는 걸 알 수있다. 

 filter-mapping의 url-pattern이   '/*' 이다. 모든 요청에 대해서 해당 필터가 적용된다.

즉 우리가 web.xml에서 설정한 내용은 모든 요청에 대해서 utf-8로 인코딩하겠다는 것이다.

 

 

 

 

2. <context-param>, <listener>

기본적으로 web.xml에서  <context-param>이 먼저 있지만,   <listener> 태그를 먼저 알아야 한다.

먼저  자바에서 Listener란 특정 이벤트가 발생할 때 실행되는 클래스나 인터페이스이다. 

우리는 톰캣을 이용해서  동적 웹 프로젝트를 만들고 있기 때문에 

web.xml의 <lister>태그는 서버가 켜질 때 실행되는 클래스를 정의한다. 

ContextLoaderLister는 ServletContextLitenr를 상속했다.

ServletContextListener은 서버가 켜지고 꺼질 때의 이벤트를 제공한다

즉 서버가 켜질 때 org.springframework.web.context.ContextLoaderListener (스프링에서 제공하는)

클래스가  실행된다.   그리고 ContextLoaderLister는 ApplicationContext를 만든다. 

그렇다. 빈 등록과 관리를 해주는 바로 그 Context이다. <context-param>의 값들을 보자.
	
```xml
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>/WEB-INF/classes/spring/context-*.xml</param-value>
	</context-param>
```
	
여기에 보면  contextConfigLocation은  ContextLoaderLisnter에서 내부적으로 사용하는 이름이라 바뀌면 안된다.

/WEB-INF/classes/spring/context-*.xml은 파일 위치이다. 

즉 ApplicationContext는 context-*.xml을 읽어서 빈을 등록한다.

 

 

 

3.<servlet>, <servlet-mapping>

<servlet-class>에 보면 DispatcherServlet이 있다. 이는 스프링에서 제공해주는 Servlet이다 .

DispatcherServlet이 하는 역할은 너무 많기 때문에 여기서 다 설명하기 어렵우니 빈 관련해서만 일단 설명한다.

DispatcherServlet은  WebApplicationContext를 만든다. 

<init-param>태그를 보자.
	
```xml
	<init-param>
	<param-name>contextConfigLocation</param-name>
	<param-value>/WEB-INF/classes/spring/mvc-servlet.xml</param-value>
</init-param>
```
	
<param-value> 태그에 보면 mvc-servlet.xml이 있다.   

WebApplicationContext은 이 mvc-servlet.xml을 읽어서 빈을 등록한다.

 

이제 알 수 있을 것이다. 서버가 켜질 때 

ContextLoaderListenr에 의해 ApplicationContext가  생성되고  이 Context는 context-*.xml을 읽어서 빈을 관리한다.

DispatcherServlet에 의해 WebApplicationContext가  생성되고  이 Context는 mvc-servlet.xml을 읽어서 빈을 관리한다.

이 때 중요한 건 ApplicationContext에는 Service,Dao 단의 빈을 등록하고, 

WebApplicationContext에는 Controller단의 빈을 등록해야 한다.

여기서 ApplicationContext에 등록된 빈은

WebApplicaionContext에 등록된 빈에 접근이 불가능하고, 그 반대는 가능하다.
	
![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/ffb35093-3797-4756-82ab-fe5420283028)
	
※ 여기서 DispatcherServlet이 2개인건 단순히 빈의 참조 가능 관계를 표현하기 위해서이다. 

저렇게 DispatcherServlet이 2개일려면 web.xml에서 <servlet> 태그에 DispatcherServlet을 등록해주고 

<url-pattern>을 나누면 된다.   하지만 실제로 DispatcherServlet은 FrontController 역할을 하기 때문에

대부분의 경우 DispatcherServlet은 1개만 둔다.

 

 

이렇게 빈의 영역을 나누는 이유는 물론 유지보수 및 확장의 이점 때문인데 이를 다 설명하긴 힘들다.

간단히 말하면 서버에는 DispatcherServlet만 있는게 아니라 JspServlet, 기타 프레임워크 등이 있을 수 있다.

이 때 JspServlet,프레임워크 등에서도 ApplicationContext에서 등록된 빈(Service,Dao)은 사용이 가능하다. 

WebApplicationContext에 등록된 빈(Controller)은 사용불가능하다. 

자세한건 여기를 참조.  https://gunju-ko.github.io/toby-spring/2019/03/25/IoC-%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88%EC%99%80-DI.html

물론 우리는 DispatcherServlet 1개로 모든요청을 다 처리할 것 이다. 

# 3.context-*.xml,   mvc-servlet.xml

위에서 설명한 대로 빈을 나눠서 등록하도록 설정하면 됩니다.
![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/9b961863-8346-4afe-8166-51b5d7a92563)

보통 서버 한개에 프로젝트가 이렇게 여러개 있을 수 있다. 이 경우 그림은 다음과 같다.
![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/e8699b34-809f-4705-9ef1-53c5b85afd59)


이렇게 url요청을 localhost:8080 (tomcat)/study4(contextpath)로 시작해야 우리가 만든 spring project에 접근할 수 있다

위 그림에서 study4_1 프로젝트의 context 구성에서

Service,Dao 관련빈은 Application Context에,

Controller단 관련 빈은 WebApplcationContext에 있다. 

이렇게 설정했을 때 빈들의 참조 관계가 이 그림이 되는것이다.
![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/8cb4a06d-3d3b-4b64-b0c3-33c8a22a66d4)
	
위 그림들에서처럼 빈들 관련 설정을 각각 해주면 된다. web.xml에서 

ApplicationContext(ContextLoaderListener)설정파일은 context-*.xml 이다. 여기서 Service,Dao빈들을 등록해준다.

WebApplicationContext(DispatcherServlet)설정파일은 mvc-servlet.xml이다. 여기서 Controller 빈들을 등록해준다.

실제로 위의 context-*.xml, mvc-servlet.xml은 이렇게 빈들을 등록하였다. 

다음글에서 context-*.xml과 mvc-servlet.xml에 등록된 빈에 대해서 간단히 설명하겠다
	
	
출처: https://brilliantdevelop.tistory.com/88
