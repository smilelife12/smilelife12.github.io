# 스프링 공부 내용 정리

## Servlet이란(<https://mangkyu.tistory.com/14)>

- 웹프로그래밍에서 클라이언트의 요청을 처리하고 클라이언트에게 전송하는 Servlet 클래스의 구현 규칙을 지킨 자바 프로그래밍 기술
- 특징으로
    1. 클라이언트 요청에 대해 동적으로 작동하는 웹 어플리케이션 컴포넌트
    2. html을 사용해 요청에 응답
    3. Java Thread를 이용해 동작
    4. MVC 패턴에서 Controller로 이용
    5. HTTP 프로토콜 서비스를 지원하는 javax.servlet.http.HttpServlet 클래스 상속
    6. html 변경시 Servlet 재컴파일하는 단점
- 동작 방식
    1. URL클릭하면 HTTP request 하는 servlet container 전송
    2. request를 전송받은 container는 HttpServleRequest, HttpServletResponse 두객체 생성
    3. web.xml에서 요청한 url분석해 어느 서블릿인지 찾음
    4. 해당 서블릿에서 service 메소드 호출해 post인지 get인지 구분하여 doGet(), doPost() 호출
    5. 해당 메소드가 동적페이지를 생성해 HttpServletResponse 객체에 응답 보냄
    6. 응답 종료후 HttpServletRequest, HttpServletResponse 소멸

## Servlet Container

- Servlet이 작동하기 위해 필요한 역할을 하는 부분
- 역할로는
    1. 웹서버와 통신 지원
    2. 서블릿 생명주기 관리(가비지 컬렉션 진행)
    3. 멀티 쓰레드 지원 및 관리
    4. 선언적인 보안관리

## Sevlet의 Listenr와 Filter(<https://galid1.tistory.com/524?category=783055>)

- Listener가 주요 변화 감지와 이벤트 이전에 특별 작업 처리 기능
- Filter의 경우 요청을 Servlet에 전달하기 전에 특별 처리를 하는 부분
    1. init: filter가 등록되 초기화 실행
    2. doFilter: servlet에 사용자 요청 들어왔을때 servlet 전달 전 실행
    3. destroy: servletcontainer 종료 전 filter 삭제될 때 호출

## Spring에서 IoC Container 사용

- maven을 이용해 의존성 추가한 후 web.xml에서 해당 내용을 사용하는 것을 선언한다.
- 이를 통해 linstener/ filter등을 활용할 수 있다.
- 또한, Servlet을 연결하여 주는 역할을 해서 이를 활용해 servlet 작동시킴
- 이 중 ContextLoaderLisnter는 ServletContext의 라이프 사이클에 맞춰 자동으로 ApplicationContext에 추가
- servlet을 생성하고 이를 bean으로 주입받아서 사용할 수 있다.

## Dispatcher Servlet

- 각각의 servlet을 각각 처리하는 것이아니라 하나의 FrontController Servlet을 만들고 이를 활용해 요청에 적절한 Contrlloer에 연결해 주는 것을 하는 역할

```html
<servlet>
    <servlet-name>name</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>name</servlet-name>
    <url-pattern>/name/*</url-pattern>
</servlet-mapping>
```

- 위처럼 선언을하며 해당 servlet을 이름을 설정해 지정하고, 이를 dsipatcherservlet으로 선정한 뒤에 해당 servlet을 사용하는 url-pattern을 선정해 줄 수 있음.
- 이 외에도 \<init-parm\> 태그를 이용해서 적절한 ApplicationContext Class도 지정가능하다.

## DispatcherServlet의 응답과정

- 모든 요청을 Handler로 요청을 보내 처리
    1. doService()
        - 모든 요청을 처음으로 HttpServletRequest, HttpServletResponse 객체로 받는 매개변수로 받는 해당 메소드를 호출
    2. doDispatch()
        - doService()를 통해 여기로 전달. 모든 요청을 각각에 알맞게 전달하는 역할
        - 이는 getHandler()에서 for문을 돌며 DispatcherServlet이 가진 HandlerMapping 객체들의 getHandler()메소드에 다시 전다랗여 처리하는 역할
- HandlerMapping은 사용자의 요청 정보를 이용해 실제 요청을 수행할 Handler를 찾는 역할
    1. Property
        - order: 여러개 등록된 Bean을 순서를 정해주기 위해 사용
        - defaultHandler: 요청한 url중 어떤 handler와도 매핑되지 않는 경우 실행할 HandlerMapping 지정
    2. Interceptor
        - Controller 호출 전, 후에 request, response, ModleAndView를 참조해 가공 및 처리를 하는 필터 역할

## ViewResolver, DispatcherServlet

- ViewResoler
  - 전체 경로를 쓰지않고 view이름으로부터 사용될 view 객체를 매핑하는 역할
  - servlet에서 bean을 활용해 등록하며 해당 값들의 property를 적용해 사용한다.
  - prefix를 통해서 해당 파일의 위치를 지정해준다.
  - suffix를 통해서 확장자를 지정하고 원하는 확장자 인식하도록 돕는다.
  - 이를 이용해서 return 받은 string 값과 조합하여 해당 구조를 찾아갈 수 있도록 한다.

## Factory Method Pattern

- 팩토리 메소드 패턴은 객체를 만들어내는 부분을 서브 클래스에 위임하는 패턴
- 객체를 만들어내는 공장(Factory 객체)을 만드는 패턴
- 틀을 만들어 놓고 여기에 확장해서 사용하는 패턴이라고 생각하면 될 거 같음 보통 extends를 이용

## AOP(<https://addio3305.tistory.com/86> + 토비's 스프링)

- AOP는 Aspect Orient Programming(관점 지향 프로그래밍)의 약자이다.
- 자바의 OOP라는 것을 더 OOP스럽게 사용하려는 의도로 만들었다.
- 객체를 재사용함으로써 반복되는 코드 양을 굉장히 많이 줄일 수 있었다.
- 여기서도 또 반복되는 부분이 생성되는데 로그, 권한 체크, 인증, 예외 처리가 반복되는 데 이를 해결할 수 있다.
- 기능을 비즈니스 로직과 공통 모듈로 구분해 개발자의 코드 밖에서 필요한 시점에 비즈니스 로직을 삽입해 실행한다.
- 즉, 다른 부분에서 각각 처리하던 부분을 하나로 통일된 부분을 통과하여 처리될 수 있도록 설정하는 것이다.

### 토비

- **DI 란 Dependency Injection으로 의존성 주입의 준말**
- 의존 관계를 분리하여 서비스의 인터페이스를 구현하고, 트랜잭션을 처리하는 부분을 구현해준다.
- 이를 통해 트랜잭션의 처리는 트랜잭션만, 서비스는 서비스만 관리할 수 있도록 사용
- 여기서 분리한 것에서 추가적으로 **데코레이터 패턴**을 적용해 더 분리해 준다.
  - 내용물은 동일하지만, 추가적인 데코를 하듯 부가적인 효과를 부여해 주는 것으로 작동한다.
  - 프록시가 여러개이므로 이를 순서를 정해 단계적으로 위임하는 구조 생성
- 데코레이터 패턴에 추가적으로 **프록시 패턴**도 적용해준다.
  - 타깃에 대한 접근 방법을 제어하려는 목적을 가진 것이다.
  - 타깃 오브젝트의 레퍼런스를 알고 클라이언트에게 이 레퍼런스를 넘길때 타깃 오브젝트를 만들어서 넘기는 것이 아닌 프록시를 넘기는 것이다.
  - 그래서 이를 통해 접근 제어가 가능해지는 부분이 생긴다.
  - 하지만, 이는 굉장히 번거롭다.
- 이를 적용할 때 번거로움을 피하기 위해 **다이내믹 프록시**를 사용한다.
  - java.lang.reflect에서 지원해주는 클래스
  - 주로 인터페이스 구현하고, 위임하는 코드를 작성하는 것이 번거롭다는 점을 해결하고, 인터페이스가 많아지고 다양해졌을 때와 타깃 인터페이스가 추가 변경시 함께 수정해야한다는 점도 개선가능한 부분이다.
  - 이를 리플렉션을 통해서 프록시를 만들어준다.
  - 예를 들어 `String name = "Spring";` 라는 코드를 작성하고 이를 `length()`를 불러오는 것이 아니라 `Method lengthMethod = String.class.getMethod("length");` 를 이용해서스트링이 가진 메소드 중 `"length"`라는 이름을 가진 메소드를 가져온다.

#### 포인트컷 표현식

- 정규식과 비슷한 일종의 표현식 언어이다.
- `AspectJExpressionPointcut`은 클래스와 메소드를 같이 사용할 수 있게 해준다.
- 지시자 중 가장 많이 사용되는 것은 `execution()`
  - `execution(\[접근제한자패턴\] 타입패턴 \[타입패턴.\] 이름패턴(타입패턴 | "..",...))` 형태
  - 표현식을 사용하는 과정에서 접근제한자, 클래스타입, 예외 패턴은 생략가능하다. 이에 따라 `execution(int minus(int,int))` 형태로 기존의 `public int springbook.learning....Target.minus(int, int) throws java.lang.RuntimeException` 을 줄일 수 있다. 더 나아가 `execution(* *(..))` 형태로 가장 간결하게 만들 수 있다.
  - 이를 활용해 추가적으로 빈을 설정할 때도 적용이 가능하다.

### AOP를 구현하기 위한 과정 정리

- 트랜잭션 서비스 추상화
  - 트랜잭션 경계설정과 비즈니스 로직을 담은 코드에 넣으면서 특정 트랜잭션에 종속되는 것을 막아준다.
  - 이를 트랜잭션 적용을 따로 남겨놓고, 서비스 추상화 기법을 적용해 구체적인 구현 내용을 담은 의존 오브젝트는 런타임 시 다이내믹하게 연결하는 DI를 활용했다.
  - 결과적으로 인터페이스와 DI를 통해서 무엇을 하는 지 남기고, 그것을 어떻게 하는 지 분리하며, 어떻게 하는 지는 비즈니스 로직 코드에 영향을 주지 않게 되었다.
- 프록시와 데코레이터 패턴
  - 트랜잭션이 대부분 비즈니스 로직을 담은 메소드에 필요하며, 경계설정 담당하는 코드 특성 때문에 단순 추상화와 메소드 추출로 불가능 했다.
  - 클라이언트가 인터페이스와 DI를 통해 접근하도록 설계하고, 데코레이터 패턴을 적용해 비즈니스 로직 클래스에 영향을 주지 않았다.
  - 클라이언트와 비즈니스 로직을 담은 타깃 클래스사이에 트랜잭션이 존재하면서, 클라이언트가 프록시 역할을 하는 트랜잭션 데코레이터를 거치게 된다.
- 다이내믹 프록시와 프록시 팩토리 빈
  - 비즈니스 로직 인터페이스의 모든 메소드마다 트랜잭션 기능을 부여하는 코드를 넣어 프록시 클래스 만드는 것이 불편하였다.
  - 이를 프록시 클래스가 없어도 런타임시 만들어주는 JDK 다이내믹 프록시 기능을 사용하게 되었다.
  - 여기에 프록시 기술을 추상화한 프록시 팩토리 빈을 이용해 다이내믹 프록시 생성방법에 DI를 도입했다.
  - 이를 통해 부가기능을 담은 어드바이스와 부가기능 선정 알고리즘을 담은 포인트 컷이 프록시에서 분리되어 공유하며 사용된다.
- 자동 프록시 생성 방법과 포인트컷
  - 빈마다 일일이 프록스 팩토리 빈을 설정이 불편하였다.
  - 지정하지 않고 자동으로 선정할 수 있도록, 클래스를 선정하는 기능을 담은 포인트컷을 사용했다.
- 부가기능의 모듈화
  - 지금까지 사용한 기술을 토대로 트랜잭션 경계설정이라는 기능을 하나의 독립적인 모듈로 생성했다.
  - 각각의 기술들이 나누어져서 설정되어 있기 때문에 트랜잭션 경계설정 또한 모듈화 시켜 사용할 수 있게 된 것이다.

### AOP: 애스펙트 지향 프로그래밍

- 결국 다양한 곳에서 사양되던 부가기능들을 핵심기능만 모듈화 시켜서 꺼내고 자기가 필요한 방식에 따라서 사용할 수 있도록 만드는 것이 AOP이다.
- 그래서 2차원적인 구조에서 3차원적인 구조로 다양한 측면에서 바라봤을 때 다이내믹하게 해당 모듈들을 사용하는 것을 말하는 것이다.

#### AOP 용어

- 타겟 : 부가기능을 부여할 대상
- 어드바이스 : 타겟에게 제공할 부가기능을 담은 모듈
- 조인 포인트 : 어드바이스가 적용될 수 있는 위치
- 포인트컷 : 어드바이스를 적용할 조인 포인트를 선별하는 작업이나 기능을 정의한 모듈, 일반적으로 실행을 의미하는 execution으로 시작
- 프록시 : 클라이언트와 타겟 사이에 투명하게 존재해 DI를 통해 타겟 대신에 클라이언트에 주입되는 대상
- 어드바이저 : 포인트컷과 어드바이스를 하나씩 갖고 있는 오브젝트, 부가기능(어드바이스)을 어디(포인트컷)에 전달할 것인가를 알고 있는 가장 기본이 되는 모듈
- 애스팩트 : 한 개 또는 그 이상의 포인트컷과 어드바이스의 조합으로 만들어져서 보통 싱글톤 형태로 존재한다.

- - -

## Web Application Structure(<https://gmlwjd9405.github.io/2018/10/29/web-application-structure.html>)

### 웹서비스 기본 설정 구조

1. src
    - 개발자가 작성한 servlet 코드 저장
2. Libraries
    - servlet이나 jsp에서 추가로 사용하는 라이브러리나 드라이버
    - jar로 압축한 파일이어야 함
3. WebContent
    - Deploy 시 디렉터리 전체가 .war로 묶여 보내짐
    - WEB-INF
        - lib : 추가한 모든 라이브러리 드라이버 저장
        - classes : 작성한 JavaServlet 파일이 .class로 모두 저장
        - web.xml : SUN에 정해놓은 규칙에 맞게 작성하며 WAS에 대해 모두 작성방법이 동일
    - html 파일

### web.xml 기본설정

- 역할
    - Deploy할 때 Servlet의 정보를 설정
    - 브라우저가 Java Servlet에 접근할 때 필요한 정보를 알려줄 Servlet 호출

### web.xml 구체적인 설정 내용

1. DispatcherServlet
    - Spring Container 생성 -> 이는 Controller lifecycle 관리
    - 클라이언트의 요청을 처음 받는 클래스
    - 클라이언트 요청 Handler로 전송
    - 추가 사항
        - Handler Mapping : 어떤 url인지 설정
        - ViewResolver : prefix와 suffix 설정
    - 예시
        - \<servlet-name\> 에 설정한 이름 + -servlet.xml 파일 생성 그리고 web.xml과 같은 위치에 존재해야 contextLoader가 읽음
        - 이경우는 \<init-param>으로 이름 설정하지 않아도 자동으로 로드 

```html
<servlet>
  <servlet-name>exServlet</servlet-name>
  <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>

  <init-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>/WEB-INF/exServlet-servlet.xml</param-value>
  </init-param>
  <load-on-startup>1</load-on-startup>
</servlet>

<servlet-mapping>
  <servlet-name>exServlet</servlet-name>
  <url-pattern>/ex</url-pattern>
</servlet-mapping>
```

2. ContextLoaderListener
    - Controller가 공유하는 Bean을 포함하는 Spring Container 생성
        - 공유하는 Bean : Dao, DataSource, Service
        - 각 Bean에 대한 설정 파일 따로 생성
            - service-context.xml : service 관련
            - dao-context.xml : dao 관련
            - applicationContext.xml : datasource관련, properties등록, SessionFactory, TransactionManager 등
            - security-context.xml : security관련, BCryptPasswordEncoder 등
3. encodingFilter
    - 인코딩을 UTF-8로 설정하여 필터링 하겠다고 설정, 다른 방식으로도 가능


## web.xml 안의 태그들

### display-name

- gui툴이 웹어플리케이션 이름 설정하는데 사용하는 이름
- 속한 프로젝트의 이름을 적는 부분
- Wrapsody Service로 명칭

### session-config/ session-timeout

- 세션 유지 시간 지정하는 부분으로 120초로 설정되어 있음

### filter

- 프로젝트에서 사용할 필터 클래스를 등록하는 부분
- 필터 안의 태그
    1. filter-name
        - 필터 이름 설정, 닉네임부분
    2. filter-class
        - 필터 클래스에 대한 이름 적는 것으로 패키지 이름까지 전체 등록
    3. init-param
        - 해당 필터클래스가 갖는 매개변수들 적는 부분
        1. param-name
            - 매개 변수 명
        2. param-value
            - 값

### filter-mapping

- 필터 클래스를 지정하고 해당 필터 클래스가 실행되는 시점을 등록하는 것
- \<dispatcher\>를 통해서 dispatcher 타입 설정 가능
  - REQUEST : 클라이언트 요청인 경우 필터 적용
  - FORWARD : foward()를 통해서 제어 흐름이 이동한 경우 필터를 적용
  - INCLUDE : include()를 통해서 포함되는 경우에 필터 적용
  - ERROR : 예외 발생 시 예외 처리 페이지를 요청하는 경우 필터 적용

### listener

- web.xml에서 객체를 참조하기 위해 사용되는 부분
- ContextLoaderListener 클래스를 DispatcherServlet 클래스의 로드보다 먼저 동작해 비즈니스 로직층을 정의한 스프링 설정 파일을 로드

### servlet

- servlet은 filter와 설정방식이 비슷하며 servlet을 설정하기 위해 사용

### context-param

- 사용자가 직접 컨트롤하는 xml파일을 지정해주는 역할
- 이름과 해당위치를 설정해 줄 수 있음


## welcome-file-list

- 시작화면을 설정해주는 부분
- html과 jsp 설정이 가능

## mybatis로 여러 종류 변수 전달

- 여러종류의 변수 전달시 `parameterType = 'map'`으로 해놓고 각각을 순서대로 `#{param1}`,`#{param2}` ... 이렇게 적용하여 가져온다.