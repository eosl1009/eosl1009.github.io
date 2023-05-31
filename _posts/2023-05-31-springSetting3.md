---
layout: post
title: Spring 세팅3
date: 2023-05-30
category: Spring
---


`WebApplicationContext(DispatcherServlet)설정파일은 mvc-servlet.xml이다. Controller단 관련 빈들을 등록해준다.`

즉, mvc-servlet에서는 @Controller를 스캔하도록 하면된다. 물론 @Controller뿐만아니라 Controller 단에서 사용하는

여러 빈들을 등록하면된다. 

근데 이런 빈들은 각각의 역할이 있기 때문에 단순히 빈 등록 방법만 하기보다는 이 DispatcherServlet이 어떤식으로

동작하는지를 알아야한다.  그 후 mvc-servlet.xml 내용을 살펴보자.

또 기본적으로 우리가 만드는 Controller는 다음과 같은 형태를 띈다.

```java
@Controller
public class FreeController {
@RequestMapping("/free/freeList.wow")
	public String freeList() {
		return "free/freeList";
	}
}
```


아래 SpringMVC동작원리는 지금 한번 보고 

나중에 sutdy3_1에 있던 코드를 Spring 적용하고 나서 다시 봐야한다.

# Spring MVC 동작 원리

![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/283e1e7a-0010-47e1-bc02-ddc39f987322)

web.xml에서  DispathcerServlet을 Servlet으로 등록했다. Spring 프로젝트에서는 이 DispatcherServlet이

FrontController 역할을 한다. 즉, 모든 요청을 이 DispatcherServlet이 처리한다. 

저렇게 동작원리에 대한 그림이 있지만, 모두 `DispatcherServlet의 doService메소드에서 실행되는 내용`인 것이다.

위의 1~7번까지의 과정을 실행 후  RequestDispatcher.forward() 까지 실행하면 비로소   jsp화면이 나온다.

 

1. DispatcherSevlet 은 모든 연결을 담당하며, 웹 브라우저에서 요청이 들어오면

2. 그 요청을 처리하기 위해 HandlerMapping 객체에게 컨트롤러 검색을 요청한다.

    HandlerMapping은 클라이언트의 요청 경로를 이용해서 이를 처리할 컨트롤러 객체를 찾아서

    DispatcherServlet 에 리턴한다.  

3.  DispatcherServlet 은 @Controller 어노테이션을 이용해서 구현한 컨트롤러 클래스와 기타 응답처리 클래스를

    처리하기 위해  HandlerAdapter 객체에게 요청 처리를 위임한다.

4. HandlerAdapter 객체는 컨트롤러의 알맞은 메소드를 호출해서 요청을 처리하고,

5. 컨트롤러는 여러가지 기능을 수행 한 후  어떤 결과 값을 리턴한다. 

6. HandlerAdapter는 컨트롤러로부터 받은 결과값이 어떤 타입이든지

  ModelAndView 객체에 담아서 DispatcherServlet에 리턴한다.

7. ModelAndView 객체를 반환 받은 DispatcherServlet 은 ViewResolver 객체를 이용해서 결과를 보여줄 뷰를 검색한다.

    ViewResolver 객체는 ModelAndView 객체에 담긴 뷰 이름을 이용해서 View 객체를 찾거나 생성해서 리턴한다.

8. DispatcherServlet 은 ViewResolver 가 리턴한 View 객체에게 응답 결과 생성을 요청한다.

9. JSP를 사용하는 경우, View 객체는 JSP를 실행함으로서 브라우저에게 전송할 응답 결과를 생성한다.(forward)

   ModelAndView 의 Model 객체에 담겨 있는 데이터가 응답 결과에 필요하면 Model 에서 데이터를 꺼내 JSP에서

  사용할 수 있다.




DispatcherServlet은 요청을 처리할 때 여러 객체들을 사용하는데 이 객체들은 SpringContainer(WebApplicationContext)에 등록된 빈이다.  DispatcherServlet은 SpringContainer를 서버가 켜질 때 만드는데,  web.xml에서 정의한 대로

```xml
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
```

mvc-servlet.xml을  설정파일로 해서 SpringContainer를 만든다. 

즉 mvc-servlet.xml에 Handlermapping,HandlerAdapter, 컨트롤러(@Controller붙은 클래스), ViewResolver 등에 관한

빈 등록 태그가 있어야 한다는 얘기이다.

![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/b00eabb0-3ddc-44eb-992c-bf871e8eb5ba)

mvc-servlet.xml  (우선 namespace에서 관련 내용을 체크해줘야합니다. 근데 spring프로젝트만들면 기본으로 돼어있음)

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
	
	
	<!--1.annotation-driven태그 :   이 태그만 있으면 HandlerMapping을 비롯한 여러가지 mvc에 필요한 설정들이 자동으로 세팅 (기본값)
		annotation-driven태그 안에 내용을 하나하나 다 써도 됩니다. 80줄정도-->
	<annotation-driven />

	
	<!--2. resources 태그 :  mapping은 url요청,  location은 webapp에서의 위치 -->
	<!--  귀찮으면 폴더이름도 resources, mapping도 resources하면 됨  -->
	<resources mapping="/resources/**" location="/resources/" />

	<!--3. viewResolver 태그 :  ViewName을 찾는데 사용됨 -->
	<beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<beans:property name="prefix" value="/WEB-INF/views/" />
		<beans:property name="suffix" value=".jsp" />
	</beans:bean>
	
	<!--4. component-scan태그 : @Service, @DAO를 제외한   @Controller만 빈으로 등록한다.
    component-scan은 기본적으로  use-default-filter가 트루 일 때 
	@Controller,@Service,@Respoitory,@Component 등이 붙은 걸 빈으로 다 등록 	 (기본값도 true)
	DS은 Presentation,Controller단  빈만 등록 ->	@Controller만 가지도록
	-->
	<context:component-scan base-package="com.study"  use-default-filters="false">
		<context:include-filter type="annotation" 
		expression="org.springframework.stereotype.Controller"/>
	</context:component-scan>
	
	
	
	  
	
</beans:beans>


`WebApplicationContext(DispatcherServlet)설정파일은 mvc-servlet.xml이다. Controller단 관련 빈들을 등록해준다.`

 

이 글 맨위에 있던 내용인데, DispatcherServlet이 동작하는데 필요한 객체들에 관한 빈이 등록되면 된다.

(Service, Dao단의 빈들은 등록하지 않는다.)

 

위에서 부터 찬찬히 보자.

 

\<annotation-driven/\>

일단 전체적으로 살펴보면 ViewResolver와  @컨트롤러 빈에 관한 내용이 보이는데 

HandlerMapping,HandlerAdapter에 관한 빈은 보이지 않는다. 이 2개의 객체는 \<annotation-driven/\> 태그에 

정의 되어 있다. 물론 이 2개의 객체 말고도 여러가지 기능이 포함되어있다.

기본적으로 \<annotation-driven/\> 태그에서 HandlerMapping,HandlerAdapter 구현체로 등록되는 빈은

RequestMappingHandlerMapping, RequestmappingHandlerAdapter이다 .

또  SpringMVC 동작원리 8번에서 JSP로 포워딩하는 View의 구현체로 등록되는 빈은 InternalResourceView이다.

 

 

 

\<resources\>

Spring MVC 동작원리는 .jsp같은 동적콘텐츠에 해당하는 것이다.  일반적으로 정적컨텐츠인 css,js 등은

WebServer를 따로 설치해서 처리하는게 좋지만, WebServer를 따로 설치하지않았을 경우 

DispatcherServlet이 정적컨텐츠를 처리할 수도 있다.  이 정적컨텐츠를 처리하도록 하는 태그가

\<resources\>태그이다.  mapping과 location 속성이 있는데 별로 어렵지 않다.

mapping은 url요청에 대한 값을 적는다.

location은 프로젝트에서 파일 위치를 적는다.

만약 다음과 같이 설정되어있다면
  
```xml
  <resources mapping="/url/*" location="/bootjs/" />

```

url 요청  : \<script src="\<%=request.getContextPath() %\>/url/bootstrap-3.3.2/js/bootstrap.js"\>\</script\>이고 

![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/5cc2ddf9-4e8f-48ae-8b8b-a0ccb04ee516)

와 같다.

하지만 실제 코드에서는 헷갈리지 않게 그냥 mapping과 location 모두 resouces라는 값으로 통일했다.

```xml
<resources mapping="/resources/*" location="/resources/" />
```

url 요청  : \<script src="\<%=request.getContextPath() %\>/resources/bootstrap-3.3.2/js/bootstrap.js"\>\</script\>이고

**\<bean  class= InternalResourceViewResolver\>

Spring 동작원리 7번부분에서 동작하는 빈이다.  preFix, Suffix를 보면 다음과 같이 되어있다.

prefix:  /WEB-INF/views/
suffix : .jsp
이렇게 되어있다. 만약 컨트롤러에서   "free/freeList"를  return 했다면

DispatcherServlet은 "/WEB-INF/views/free/freeList.jsp" 의 view 값을 얻는다. 

나중에 이 jsp로 forwarding 하게 돼서 해당 jsp 화면이 나오게된다.

 

 

** \<context:component:scan\>

이전글에서 설명했다. 단지 이전글에서는 ContextLoaderListenr가 @Controller를 제외한 @을 빈으로 등록하도록 했고

여기서 DispatcherServlet은 @Controller만 빈으로 등록하도록 했다.  

 

 
# DispatcherServlet 동작 코드로 살펴보기

localhost:8080/study4/free/freeList.wow로 요청했을 때 

```java
@Controller
public class FreeController {
@RequestMapping("/free/freeList.wow")
	public String freeList() {
		return "free/freeList";
	}
}
```
 
이 실행되고 freeList.jsp에 의한 화면이 나오는 과정까지의 DispatcherServlet 코드동작을 살펴보겠다.

 

 

 

DispatcherServlet의 doService메소드. 클라이언트로부터 요청이오면 실행되는 메소드



```java
@Override
	protected void doService(HttpServletRequest request, HttpServletResponse response) throws Exception {
        //생략
        try {
                    doDispatch(request, response);
        }
        //생략
	}
```
 

 doDispatch메소드. doSerivce에서 호출한 메소드로서 DispatcherServlet동작에 관한 코드는 이 메소드안에 있음.


```java
protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {
    HttpServletRequest processedRequest = request;
    HandlerExecutionChain mappedHandler = null;
    boolean multipartRequestParsed = false;
 
    WebAsyncManager asyncManager = WebAsyncUtils.getAsyncManager(request);
 
    try {
        ModelAndView mv = null;
        Exception dispatchException = null;
 
        try {
            // multipartResolver에 의해 resolve 처리 (request를 파싱하여 file이 있다면 set)
            processedRequest = checkMultipart(request);
            multipartRequestParsed = (processedRequest != request);
 
            // 1.현재 요청에 알맞은 핸들러를 가져온다.
            mappedHandler = getHandler(processedRequest);
            if (mappedHandler == null || mappedHandler.getHandler() == null) {
                noHandlerFound(processedRequest, response);
                return;
            }
 
            // 2.위에서 가져온 핸들러를 실행(invoke) 시킬 수 있는 핸들러 아답터를 가져온다. 
            HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());
 
            // last-modified header 수정. ( last-modified 헤더값으로 브라우저나 proxy 서버에서 캐시를 이용할 수 있음)
            String method = request.getMethod();
            boolean isGet = "GET".equals(method);
            if (isGet || "HEAD".equals(method)) {
                long lastModified = ha.getLastModified(request, mappedHandler.getHandler());
                if (logger.isDebugEnabled()) {
                    logger.debug("Last-Modified value for [" + getRequestUri(request) + "] is: " + lastModified);
                }
                if (new ServletWebRequest(request, response).checkNotModified(lastModified) && isGet) {
                    return;
                }
            }
 
            // 적용할 interceptor가 있다면, preHandle 적용
            if (!mappedHandler.applyPreHandle(processedRequest, response)) {
                return;
            }
 
            //3.실제 컨트롤러 핸들러(메소드) 실행
            mv = ha.handle(processedRequest, response, mappedHandler.getHandler());
 
            //비동기 처리라면, 종료.  WebAsyncManager에서 별도의 스레드가 처리하게 된다.
            if (asyncManager.isConcurrentHandlingStarted()) {
                return;
            }
 
            //View 에 대한 설정이 없다면, RequestToViewNameTranslator 전략 빈에 의해 view name을 설정한다.
            applyDefaultViewName(processedRequest, mv);
            //interceptor postHandle 실행
            mappedHandler.applyPostHandle(processedRequest, response, mv);
        }
        catch (Exception ex) {
        // 예외 처리 생략
        }
 
        //4.핸들러 실행 결과 ModelAndView를 적절한 response 형태로 처리한다.
        // viewResolver를 이용해 view를 선택하고, forwarding한다
        processDispatchResult(processedRequest, response, mappedHandler, mv, dispatchException);
    }
 
    //아래 예외처리 부분 생략
```


인터셉터나 비동기처리에 관한 내용은 냅두고 일단 어떻게 우리가 만든  @Controller 의 @RequestMapping이

실행되는지만 살펴보자. 또 최종적으로 jsp 화면이 나오는지 까지도 알아보자.

 

 

 

**1.mappedHandler=getHandler()의 getHandler  // 현재 요청에 알맞은 핸들러를 가져온다.

우리는 @Controller를 붙인 클래스를 사용하지만, DispatcherServlet(Spring) 입장에서는 이 요청을 처리하는게 

@Controller를 붙인 클래스여야만 하는 건 아니다. 

Controller 인터페이스를 구현한 클래스도, 또 다른 방법으로 만든 클래스도 처리할 수 있어야 한다.

이 모든것들을 통틀어서 핸들러라 한다.  근데 사실 우리는 @Controller붙여서 클래스만드는 방법만 사용하기때문에

`핸들러= @Contrller 붙인 클래스(컨트롤러,  MVC의 C가 아니다)`  라고 인식해도 좋다.

코드를 보자. 

DispatcherServlet.getHandler()

```java
protected HandlerExecutionChain getHandler(HttpServletRequest request) throws Exception {
		for (HandlerMapping hm : this.handlerMappings) {
			if (logger.isTraceEnabled()) {
				logger.trace("Testing handler map [" + hm + "] in DispatcherServlet with name '" + getServletName() + "'");
			}
			HandlerExecutionChain handler = hm.getHandler(request);
			if (handler != null) {
				return handler;
			}
		}
		return null;
	}
```

for문 자체가 중요한 건 아니다.  HandlerMapping의 구현체를 전부 반복하겠단 얘기다.  

특별히 따로 설정을 해주지 않는다면 기본적으로 반복문을 돌면서  handler !=null 이 아니게 되는 경우는

RequestMappingHandlerMapping 인 경우다. 

RequestMappingHanlderMapping 객체 타입의 hm.getHandler(request) 메소드를 보자.

getHandler(request)메소드는 ReuqestMappingHandlerMapping의 부모 추상클래스인 AbstractHandlerMapping에 

정의 되어있다. 코드를 보자. 

AbstractHandlerMapping.getHandler()

```java
@Override
	public final HandlerExecutionChain getHandler(HttpServletRequest request) throws Exception {
		Object handler = getHandlerInternal(request);
        //   @Controller붙은 빈들중에서 /free/freeList.wow 에 해당하는 메소드가 있나 검사.
		// 있으면 handlerMethod.createWithResolvedBean()를 통해 freeController 빈 객체 리턴
        //type은 object
        
        //생략, 없을 때 처리.
        
		// Bean name or resolved handler?,  진짜 빈이 있는지 검사.
		if (handler instanceof String) {
			String handlerName = (String) handler;
			handler = getApplicationContext().getBean(handlerName);
		}

		HandlerExecutionChain executionChain = getHandlerExecutionChain(handler, request);
       	//이 메소드에서는 interCeptor에 대한 처리를 한다.
        //큰 문제가 없다면  executionChain은 handler와 똑같이 freeController 빈 객체
        // tpye은 object에서 HandlerExecutionChain으로 변경 


        //생략  cors 에 대한 처리 
    
		return executionChain;
	}
  
  결국 @ReuqestMapping("/free/freeList.wow)가 있으므로   doDispatch메소드의 

mappedHandler는 우리가 @Controller를 붙인 freeController 빈 객체가 된다.

 

 

 

** 2. HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());

mappedHandler.getHandler()를 먼저 보자. mappedHandler는 HandlerExecutionChain 타입이다.

이걸 단순히 Object 타입으로 형변한 해주는 거라 생각하면 된다. 어쨋든 return 값은 freeController 빈 객체이다.

 

getHandlerAdapter를 보자.

DispatcherServlet.getHandlerAdapter()

```java
protected HandlerAdapter getHandlerAdapter(Object handler) throws ServletException {
		for (HandlerAdapter ha : this.handlerAdapters) {
			if (logger.isTraceEnabled()) {
				logger.trace("Testing handler adapter [" + ha + "]");
			}
			if (ha.supports(handler)) {
				return ha;
			}
		}
		throw new ServletException("No adapter for handler [" + handler +
				"]: The DispatcherServlet configuration needs to include a HandlerAdapter that supports this handler");
	}
```


역시 for문은 의미가 없다.  여기도 마찬가지로 특별히 설정을 안해주면 기본적으로 

RequestMappingHandlerAdapter 일 때  if(ha.supports(handler)){return ha; } 가 실행된다.

물론 return 되는 타입은 HandlerAdapter이다.

 

 

** 3. mv = ha.handle(processedRequest, response, mappedHandler.getHandler());

실제 컨트롤러(핸들러) 메소드가 실행된다 라고 했는데, 이 실제 컨트롤러 메소드가 바로

개발자가 작성한 @RequestMapping이 붙어있는 메소드이다. 

```java
@RequestMapping("/free/freeList.wow")
	public String freeList() {
		return "free/freeList";
}
```

우리가 작성한 freeList()메소드가 실행되는 부분까지 가기위해서 ha.handle()에서 메소드를 호출하고

또 다른 객체의 메소드를 호출하고를 반복한다.

 

 

우선 ha.handle(processedRequest, response, mappedHandler.getHandler()); 에서 

AbstractHandlerMethodAdapter.handle()

-> RequestMappingHandlerAdapter.handleInternal()

->RequestMappingHandlerAdapter.invokeHandlerMethod()    

->ServletInvocableHandlerMethod.invokeAndHandle()

->InvocableHandlerMethod.invokeForRequest()

->InvocableHandlerMethod.doInvoke()에서  java Reflection을 이용해 Invoke()를 한다.(개발자가 만든 메소드 실행)

그리고 메소드 실행(invoke())하고 return 받은 값을 가지고 modelAndView를  return 한다.

mv = ha.handle(processedRequest, response, mappedHandler.getHandler());에서는

개발자가만든 메소드가 실행되고  return받은 값 가지고 ModelAndView를 만든다.

즉,  HandlerAdapter가 하는 일은 개발자가 만든 메소드 호출,  그 메소드의 return Type이 뭐든

ModelAndView로 만들어서 return 해주는 역할을 한다.

이 때 각 메소드를 실행할 때 마다 여러 필요한 파라미터들을 넘긴다.

여기서 잠깐 RequestMappingHandlerAdapter.invokeHandlerMethod()    를 보자

```java
protected ModelAndView invokeHandlerMethod(HttpServletRequest request,
			HttpServletResponse response, HandlerMethod handlerMethod) throws Exception {

		   //생략 

			invocableMethod.invokeAndHandle(webRequest, mavContainer);
			
            //생략

			return getModelAndView(mavContainer, modelFactory, webRequest);
		}
		//생략
	}
```

위의 과정은 invocableMethod.invokeAndHandle() 의 과정이고  마지막 return getModelAndView()를 보자.

```java
private ModelAndView getModelAndView(ModelAndViewContainer mavContainer,
	   ModelFactory modelFactory, NativeWebRequest webRequest) throws Exception {
		//생략    
		ModelMap model = mavContainer.getModel();
		ModelAndView mav = new ModelAndView(mavContainer.getViewName(), model, mavContainer.getStatus());
        //생략
        return mav;
}
```


ModelAndView를 만들기 전에 이미 여러과정을 거쳐서 데이터가 담긴 Model과 view이름 등을 가지고

ModelAndView를 만들고 return한다. 여기까지 했을때에 비로소
MVC동작원리 6번과정이 끝이다.

 

 

예시에서는 freeList() 메소드 실행하고 "free/freeList" 라는 String을 return 받는다.   

그리고 String을 ModelAndView에 담는다. 사용자가 freeList에서 model에 담은 데이터도 ModelAndView에 담긴다.

 

 

 

 

 

** 4.processDispatchResult(processedRequest, response, mappedHandler, mv, dispatchException);

viewResolver를 이용해 view를 선택하고, forwarding한다

DispatcherServlet.processDispatcherResult()

-> DispatcherServlet.render()

 

DispatcherServlet.render()

```java
protected void render(ModelAndView mv, HttpServletRequest request, HttpServletResponse response) throws Exception {

        //생략

        View view;
        // We need to resolve the view name.
        view = resolveViewName(mv.getViewName(), mv.getModelInternal(), locale, request);


        //생략

        view.render(mv.getModelInternal(), request, response);

        //생략
	}
```

view= resolveViewName 에서는 `viewResolver(구현체 기본 InternalResourceView)를 이용해 prefix,sufficx를 붙여서 

/WEB-INF/views/free/freeList.jsp에 해당하는 View를 return` 해준다.

더보기
 

그리고 그 view를 이용해 render메소드를 호출한다.   (view를 상속받은 AbstractView.render()가 실행된다)

->AbstractView.render()

->InternalResourceView.renderMergedOutputModel() 가 실행된다.

 

InternalResourceView.renderMergedOutputModel() 코드를 보자.

```java
@Override
	protected void renderMergedOutputModel(
			Map<String, Object> model, HttpServletRequest request, HttpServletResponse response) throws Exception {

		// Expose the model object as request attributes.
		exposeModelAsRequestAttributes(model, request);

		// Expose helpers as request attributes, if any.
		exposeHelpers(request);

		// Determine the path for the request dispatcher.
		String dispatcherPath = prepareForRendering(request, response);

		// Obtain a RequestDispatcher for the target resource (typically a JSP).
		RequestDispatcher rd = getRequestDispatcher(request, dispatcherPath);
		if (rd == null) {
			throw new ServletException("Could not get RequestDispatcher for [" + getUrl() +
					"]: Check that the corresponding file exists within your web application archive!");
		}

		// If already included or response already committed, perform include, else forward.
		if (useInclude(request, response)) {
			response.setContentType(getContentType());
			if (logger.isDebugEnabled()) {
				logger.debug("Including resource [" + getUrl() + "] in InternalResourceView '" + getBeanName() + "'");
			}
			rd.include(request, response);
		}

		else {
			// Note: The forwarded resource is supposed to determine the content type itself.
			if (logger.isDebugEnabled()) {
				logger.debug("Forwarding to resource [" + getUrl() + "] in InternalResourceView '" + getBeanName() + "'");
			}
			rd.forward(request, response);
		}
	}
```

정상적으로 잘 실행됐다면 String dispatcherPath = prepareForRendering(request, response); 에서 

dispatcherPath의 값은 "/WEB-INF/views/free/freeList.jsp" 이다

그리고 RequestDispatcher rd = getRequestDispatcher(request, dispatcherPath); 가 있다.

마지막에 rd.forward(request,response)를 이용해 실제  /WEB-INF/views/free/freeList.jsp의 jsp로 forwarding한다.

 

 

 

 

 


DispatcherServlet의 부모 추상클래스이자 HttpServlet의 자식인 FrameworkServlet의 메소드를 보자.

HttpServlet의 doGet,doPost 등의 메소드를 재정의 하면서 모두 processRequest 메소드를 실행한다.

```java
@Override
	protected final void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {

		processRequest(request, response);
	}

	/**
	 * Delegate POST requests to {@link #processRequest}.
	 * @see #doService
	 */
	@Override
	protected final void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {

		processRequest(request, response);
	}
    
    
    //그외 doPut,doDelete,doOptions, service들도 processRequest를 호출한다.
```

processRequest 메소드.  doService 메소드를 호출한다.

```java
protected final void processRequest(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
            //기타기능
         doService(request, response);
           //이하 생략
           }
``` 

doService메소드 .  

```java
/**
	 * Subclasses must implement this method to do the work of request handling,
	 * receiving a centralized callback for GET, POST, PUT and DELETE.
	 * <p>The contract is essentially the same as that for the commonly overridden
	 * {@code doGet} or {@code doPost} methods of HttpServlet.
	 * <p>This class intercepts calls to ensure that exception handling and
	 * event publication takes place.
	 * @param request current HTTP request
	 * @param response current HTTP response
	 * @throws Exception in case of any kind of processing failure
	 * @see javax.servlet.http.HttpServlet#doGet
	 * @see javax.servlet.http.HttpServlet#doPost
	 */
	protected abstract void doService(HttpServletRequest request, HttpServletResponse response)
			throws Exception;
```

추상메소드로 선언되어있다. 설명을 읽어보면 'GET,POST,PUT,DELETE  어떤 방식의 Http요청이든 

자식메소드에서 재정의된 doService가 처리한다' 는 내용이다. 원래 HttpServlet에서는 GET방식이면 doGet,

POST방식이면 doPost 메소드가 실행됐지만,  DispatcherServlet에서는 어떤방식의 요청이든 요청을 받으면

doService메소드가  실행된다.

 

 

 

※ 여기서 잠깐,  따로 설정하지 않았을 때 기본적으로 

HandlerMapping의 구현체는 RequestMappingHandlerMapping,

HandlerAdapter의 구현체는 RequestMappingHandlerAdapter라 했다. 

mvc-servlet.xml의 <annotation-driven />  태그를 사용했다. 이 말이 따로 설정하지 않았다는 말이다.

RequestMappingHandlerMapping, RequestMappingHandlerAdapter 대신 다른 구현체를 사용하고 싶다면

<annotation-driven/> 태그대신 직접 HandlerMapping. HandelerAdapter 의 구현체를 빈 등록해주면 된다.

물론 <annotation-driven/> 에는 이 2개의 관한 설정만 있지는 않다. 그 외에도 여러가지 설정을 기본적으로

해준다. 

참고로 기본 설정 값은 Spring jar파일을 따운받을 때 DispatcherServlet.properties파일안에 명시돼어있다.

![image](https://github.com/eosl1009/eosl1009.github.io/assets/49154210/3dcef046-4c8f-45b9-8a2d-ceadd6df8566)

DispatcherServlet.properties

```java
#DispatcherServlet.properties
...

org.springframework.web.servlet.HandlerMapping=org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping,\
    org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping

org.springframework.web.servlet.HandlerAdapter=org.springframework.web.servlet.mvc.HttpRequestHandlerAdapter,\
    org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter,\
    org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter

org.springframework.web.servlet.ViewResolver=org.springframework.web.servlet.view.InternalResourceViewResolver
...

```
