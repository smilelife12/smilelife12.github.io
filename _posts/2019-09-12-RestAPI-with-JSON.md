---
layout: post
title:  "REST API와 JSON format"
date:   2019-09-12
excerpt: "RestAPI란? 그리고 디자인"
tag:
- Hyun
- REST API
- JSON
- Python
comments: true
---

## 2차 코딩테스트에 대한 대비
- - -

카카오 1차 테스트에 대한 합격 메일이 왔고, 추후 이루어질 2차 테스트는 어떤 문제가 주어지는 지에 대한 메일이 왔다. json format를 이용하며 REST API를 통해서 진행한다고 한다. 그렇기 때문에 미리 공부를 해야할 것 같다. json format의 경우 과거 웹개발을 간단히 진행할 때 본적이 있지만 RESP API는 처음 듣는 것이라서 둘다 공부를 시작해야 한다. 우선 RESP API에 대해 공부를 시작해야 겠다.


### REST
- - -
 우선 REST가 무엇를 인지 알아보자. [위키백과 - REST API](https://ko.wikipedia.org/wiki/REST)를 통해서 기본을 알아보자.

 **REST**
 - - -
 REST(Representational State Transfer)는 네트워크 아키텍처 원리의 모음이다. 자원을 정의하고 자원에 대한 주소를 지정하는 방법 전반을 의미하며, 간단하게 웹상의 자료를 HTTP위에서 SOAP이나 쿠키를 통한 세션 트래킹 같은 별도의 전송 계층 없이 전송을 위한 간단한 인터페이스를 말한다. REST의 원리를 따르는 시스템은 종종 RESTful이란 용어로 지칭된다.
 - - -
 - 역사
  - 2000년에 `로이 필딩(Roy Fileding)`이 논문에 정의하였고, 그는 1996년부터 99년까지 HTTP 1.0의 기존 디자인에 기반을 둔 1.1과 병행하여 REST 구조 스타일을 개발
- 원리
  - 6가지 제한 조건
    - 클라이언트/서버 구조 : 일관적인 인터페이스로 분리
    - 무상태(Stateless) : 각 요청 간 클라이언트의 콘텍스트가 서버에 저장되어서는 안 된다.
    - 캐시 처리 가능(Cacheable): WWW에서와 같이 클라이언트는 응답을 캐싱할 수 있어야 한다.
    - 계층화(Layered System) : 클라이언트는 보통 대상 서버에 직접 연결 되었는지, 중간서버로 되었는지 모른다.
    - Code on demand(Optional) : 자바 애플링시나 자바스크립트의 제공을 서버가 클라이언트가 실행시킬 수 있는 로직을 전송하여 기능 확장
    - 인터페이스 일관성 : 아키텍처를 단순화 시키고 작은 단위로 분리해 각 파트가 독립적으로 개선될 수 있도록 해준다.

- - -
이정도가 기본적인 REST의 정의이다. 내가 이해한 바로는 HTTP형식의 다른 버전으로 이해했고, 자료를 서버로 주고받기위해 사용하는 것으로 생각이 된다.

### REST API
- - -
추가적으로 이제 API에 접근을 해보기위해 구글링 하는 중 찾은 사이트로 [Toast meetup: REST API](https://meetup.toast.com/posts/92)를 기반으로 공부 해보려 한다.

#### REST API 구성
- - -
REST API는 쉽게 3가지로 구성되어 있다.
- 자원(RESOURCE) - URI
- 행위(Verb) - HTTP METHOD
- 표현(Representation)

#### REST API 디자인 가이드
- - -
REST API 설계 시 가장 중요한 항목은 다음의 2가지로 요약할 수 있다.

첫 번째, URI는 정보의 자원을 표현해야 한다.
두 번째, 자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE)로 표현된다
> #### URI???
url은 어디서 많이 들어봣는데 uri는 처음들어봤다.
URL은 Uniform Resource Locator, URI는 Uniform Resource Identifier 라고 한다.
즉, URL은 자원의 위치를 가르키는 값이고, URI의 경우는 자원의 식별자를 나타내는 것이다.
더 나아가 보면, URI는 Rewrite 등의 Apache, IIS, Tomcat등의 처리하는 자원 식별자라고 한다. 그리고 URL은 이 URI에 포함이 되는 것이다. 정확한 위치를 가르키는 URL이 그 값에 대한 식별자로 나타내는 URI안에 속하게 되는 것이다.

##### 1. REST API 중심 규칙
- - -
1) URI는 정보의 자원을 표현해야 한다.(리소스명은 동사보다는 명사를 사용)
 ```
 GET /members/delete/1
 ```
 과 같은 경우는 행위에 대한 표현이 들어갔기 때문에 잘못된 표현이다.
