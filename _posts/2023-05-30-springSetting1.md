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
