---
layout: post
title:  "REST API와 JSON format(2)"
date:   2019-09-16
excerpt: "Rest API이어서, JSON이란?, Rest API에서 http request 처리"
tag:
- Hyun
- REST API
- JSON
- Python
comments: true
---

# 2차 코딩테스트에 대한 대비
- - -
REST API 공부 이어서...

## REST API
- - -
[Toast meetup: REST API](https://meetup.toast.com/posts/92)를 기반 공부이어서 하기

### REST API 디자인 가이드
- - -

### 1. REST API 중심 규칙
- - -
1) URI는 정보의 자원을 표현해야 한다.(리소스명은 동사보다는 명사를 사용)
 ```
 GET /members/delete/1
 ```
 과 같은 경우는 행위에 대한 표현이 들어갔기 때문에 잘못된 표현이다.

2) 자원에 대한 행위는 HTTP method(GET, POST, PUT, DELETE 등)로 표현
위의 잘못된 URI를 HTTP Method를 통해 수정해보면
```
DELETE /members/1
```
으로 수정할 수 있겠습니다.
회원정보를 가져올 때는 GET, 회원 추가시의 행위를 표현하고자 할 때는 POST METHOD를 사용하여 표현합니다.

**회원 정보를 가져오는 URI**
```
GET /members/show/1 (o)
GET /members/1      (x)
```

**회원을 추가할 때**
```
GET /mebers/insert/2  (x) - GET 메서드는 리소스 생성에 맞지 않습니다.
POST /mebers/2        (o)
```

>**[참고] HTTP METHOD의 알맞은 역할**  
POST    : POST를 통해 해당 URI를 요청하면 리소스를 생성합니다.
GET     : GET를 통해 해당 리소스를 조회합니다 리소스를 조회하고 해당 도큐먼트에 대한 자세한 정보를 가져온다.
PUT     : PUT를 통해 해당 리소스를 수정합니다.
DELETE  : DELETE를 통해 리소스를 삭제합니다.


### 2. URI 설계시 주의할 점
- - -
1) 슬래시 구분자(/)는 계층관계를 나타내는 데 사용
```
http://restapi.example.com/houses/apartments
http://restapi.example.com/animals/mammals/whales
```

2) URI 마지막 문자로 슬래시(/)를 포함하지 않는다.
URI가 다르다는 것은 리소스가 다르고, 리소스가 다르면 URI가 다르다는 것이며 혼동을 주지 않기 위해 마지막에는 슬래시 사용하지 않는다.
```
http://restapi.example.com/houses/apartments/ (X)
http://restapi.example.com/houses/apartments  (0)
```

3) 하이픈(-)은 URI 가독성을 높이는데 사용
URI가 너무 길면 가독성이 떨어져서 하이픈을 사용할 수 있음

4) 밑줄(\_)은 URI에 사용하지 않는다.
밑줄은 보기 어렵고 문자에 가려지기 때문에 하이픈을 사용

5) URI 경로에는 소문자가 적합하다.
RFC 3986(URI 문법 형식)은 URI 스키마와 호스트를 제외하고는 대소문자를 구별하도록 규정

6) 파일 확장자는 URI에 포함시키지 않는다.
```
http://restapi.example.com/members/soccer/345/photo.jpg (X)
```
메시지 바디 내용의 포맷을 나타내기 위해서 URI 안에 포함하지 않음, Accept header를 사용하자

```
GET / members/soccer/345/photo HTTP/1.1 Host: restapi.example.com Accept: image/jpg
```

### 3. 리소스 간의 관계를 표현하는 방법
- - -
REST 리소스 간 연관 관계가 있는 경우 다음과 같은 표현방법 사용
```
/리소스명/리소스 id/관계가 있는 다른 리소스명

ex) GET : /users/{userid}/devices (일반적으로 소유 'has'의 관계를 표현할 때)
```

만약 관계명이 복잡하다면 이를 서브 리소스에 명시적으로 표현하는 방법이 있다. 예를 들어 사용자가 '좋아하는' 디바이스 목록을 표현해야 할 경우 다음과 같이 사용
```
GET : /users/{userid}/likes/devices (관계명이 애매하거나 구체적 표현이 필요할 때)
```

### 4. 자원을 표현하는 Collection과 Document
- - -
DOCUMENT는 단순히 문서로 이해하거나 한 객체라고 이해하면 된다. collection은 문서들의 집합, 객체들의 집합이라 이해하면 된다. 컬렉션과 도큐먼트는 리소스라고 표현할 수 있으며 URI에 표현.
```
http:// restapi.example.com/sports/soccer
```
위의 예시는 sports라는 컬렉션과 soccer라는 도큐먼트로 표현되고 있다.
```
http:// restapi.example.com/sports/soccer/players/13
```
sports, players 컬렉션과 soccer, 13(13번인 선수)를 의미하는 도큐먼트로 URI가 이루어지게 된다. 주요한 점은 컬렉션은 복수로 사용된다. 그래서 컬렉션은 복수, 도큐먼트는 단수로 하면 쉽게 uri설계할 수 있다.

## HTTP 응답 상태 코드
- - -
응답에 대해 어떻게 표현되는지 응답의 상태코드 값을 명확히 돌려주는 것은 생각보다 중요하다.
```
200 : 클라이언트의 요청을 정상적으로 수행함
201 : 클라이언트가 어떠한 리소스 생성 요청, 해당 리소스가 성공적으로 생성(POST를 통한 리소스 생성 작업 시)

400 : 클라이언트의 요청이 부적절 할 경우 사용
401 : 클라이언트가 인증되지 않은 상태에서 보호된 리소스 요청시 사용 ( 로그인 하지 않은 유저가 로그인 했을 때, 요청 가능한 리소스 요청 시 )
403 : 유저 인증상태와 관계 없이 응답하고 싶지 않은 리소스를 클라이언트가 요청했을 때 사용 (403보다는 404를 사용할 것 권고, 403 자체가 리소스 존재함을 의미)
405 : 클라이언트가 요청한 리소스에서 사용 불가능한 Method를 이용했을 경우 사용

301 : 클라이언트가 요청한 리소스에 대한 URI가 변경 되었을때 사용(응답 시 Location header에 변경된 URI 적어야 함)
500 : 서버에 문제가 있을 경우 사용
```
- - -

이를 통해서 기본적인 REST API에 대해서 알아봤다. 이제 이를 이용하기 위한 json format의 파싱에 대해서 공부를 시작해 보자.
