---
layout: post
title:  "REST API와 JSON format(2)"
date:   2019-09-16
excerpt: "카카오 코딩테스트 대비"
tag:
- Hyun
- REST API
- JSON
- Python
comments: true
---

## 2차 코딩테스트에 대한 대비
- - -
REST API 공부 이어서...

### REST API
- - -
[Toast meetup: REST API](https://meetup.toast.com/posts/92)를 기반 공부이어서 하기

#### REST API 디자인 가이드
- -

##### 1. REST API 중심 규칙
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


##### 2. URI 설계시 주의할 점
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

##### 3. 리소스 간의 관계를 표현하는 방법
- - -
REST 리소스 간 연관 관계가 있는 경우 다음과 같은 표현방법 사용
```
/리소스명/리소스 id/관계가 있는 다른 리소스명

ex) GET : /users/{userid}/devices (일반적으로 소유 'has'의 관계를 표현할 때)
```

만약 관계명이 복잡하다면 이를 서브 리소스에 명시적으로 표현하는 방법이 있다. 예를 들어 사용자가 '좋아하는' 디바이스 목록을 표현해야 ㅏㄹ 경우 다음과 같이 사용
```
GET : /users/{userid}/likes/devices (관계명이 애매하거나 구체적 표현이 필요할 때)
```

##### 4. 자원을 표현하는 Collection과 Document
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

#### HTTP 응답 상태 코드
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

### JSON?
- - -
JSON(JavaScript Object Notation)은 속성-값 쌍 또는 "키-값 쌍"으로 이루어진 데이터 오브젝트를 전달하기 위해 인간이 읽 을 수 있는 텍스틑를 사용하는 개방형 표준 포맷이다. 인터넷에서 자료를 주고 받을 때는 그 자료를 표현하는 방법으로 알려져 있다. 자료 종류 제한 없고, 변수 표현하는 데 적합하다. 다양한 프로그래밍 언어에서 쉽게 이용할 수 있다.

기본 자료형으로 수, 문자열, 참/거짓, 배열, 객체, null 이 존재한다.

배열은 대괄호를 사용한다. 객체는 중괄호를 이용하며, 각 쌍들을 쉼표(\,)로 구분한다. 순서는 의미없다.
출처는 [위키백과](https://ko.wikipedia.org/wiki/JSON)

### JSON 파일 읽고 파싱하여 사용하기
- - -
>**우선 파싱이란**
 언어학에서 parsing은 구문 분석이라고도 하고, 문장을 그 것을 이루고 있는 구성 성분으로 분해하고 그들 사이의 위계관게를 분석하여 문장의 구조를 결정하는 것을 말한다.
 컴퓨터 과학에서 파싱은 일련의 문자열을 의미있는 토큰으로 분해하고 이들로 이루어진 파스트리(parse tree)를 만드는 과정을 말한다.
 파서는 인터프리터나 컴파일러의 구성요소 가운데 하나로, 입력 토큰에 내재된 자료구조를 빌드하고 문법을 검사한다.
 출처 [위키백과](https://ko.wikipedia.org/wiki/%EA%B5%AC%EB%AC%B8_%EB%B6%84%EC%84%9D)

 그리고 이제 Python에서 json 포맷을 읽고 사용해 보자.

 처음 참조를 한 사이트는 [w3schools.com](https://www.w3schools.com/python/python_json.asp)을 통해서 시작해 보았다. 우선 json 모듈을 설치해야 하므로 cmd 창에 `conda install -c jmcmurray json` 를 입력하여 설치해 주자.

여기서 json string을 파싱하여 사용하기 위해서 `json.loads()`를 사용한다. 그러면 dictionary 변수 형태로 나타난다.

예시를 보면,
```python
import json

# some JSON:
x =  '{ "name":"John", "age":30, "city":"New York"}'

# parse x:
y = json.loads(x)

# the result is a Python dictionary:
print(y["age"])
```
가 있다. x를 보면 string형태로 JSON포맷을 만들어 나타냈다. 그리고 y에서 `json.loads()`를 이용해파일을 받아왔다.

그리고 나서, 디버깅을 한 후의 모습을 보면
![파일 형식]({{site.url}}/assets/img/posting0916/img1.png)

x는 string 형태이고, y는 dictionary 형태로 저장된것을 볼수 있다.

그럼 이제 파일에 대해서 출력을 해보자. 저번에 했던, `with - as`구문을 사용해 보자.

사용전에 `json.loads()`는 뒤에 붙은 `s` string을 의미하여 string 변수를 가져와 파싱하는 것이고 `json.load()`의 경우는 파이썬 객체를 기반으로 한다. 그러므로 파일 자체를 읽어올 때는 `json.load()`를 이용한다.

```python
import json

with open('ex.json') as data_json:
    y = json.load(data_json)
```
과 같이 json 파일의 내용을 불러와서 y에 dictionary형태로 저장해 줄 수 있다.

이를 반대로 json형태로 변환하는 방법은 'json.dumps()/dump()'를 이용해 주면된다.

여기서는 `sort_keys`와 `indent`를 이용해 더 깔끔하게 만들어 줄 수 있다. `sort_keys`를 통해서는 `sort_keys=True`를 하면 sorting을 해준다. 그리고 `indent`를 이용하면 자동으로 들여쓰기를 해준다.
예를 들어,
```python
import json
print(json.dumps({'4' :5, '6': 7}, sort_keys=True, indent=4))
```
를 실행하면
```
{
    "4": 5,
    "6": 7
}
```
처럼 출력이 된다.
- - -

생각보다 json 파일을 가져오는 것 자체는 어렵지 않다. 이제 이를 이용해서 REST api에 적용하는 것이 문제로 보인다. http method를 이용해서 하는 것이기 때문에 이부분에 대해서 더 공부해야한다.

### REST API와 HTTPS requests 처리
- - -
공부를 위해 구글링중 (**API Integration in Python – Part 1**)[https://realpython.com/api-integration-in-python/#appendix-rest-in-a-nutshell] 를 찾게 되었다. HTTPS request에 대한 처리를 하는 부분으로 보여 시작을 해보려한다.

#### How to Make Friends and Influence APIs
- - -
HTTPS request에 의해 여러 요청들이 처리가 된다. 그럼 이 HTTPS requests들을 코드에서 어떻게 사용할까. SDK나 라이브러리를 사용한다. 그래서 파이썬 코드를 RESTful API를 이용하는 것을 해보자.

#### Talking REST
- - -
먼저 REST에 대해 알아보자.
#### Appendix: REST in a nutshell
REST는 web API이다. API는 HTTP와 상호작용하는 것을 말하고 특정 URL을 통해 데이터를 주고받는다.

HTTP에는 다양한 methods를 갖고 있다. GET과 POST가 가장 많이 사용되며 이는 페이지에 로드 하거나 형식을 받아온다. REST에서는 이를 이용해 다른 액션을 취하게 할 수 있다.

GET은 보통 객체나 이미 있는 레코드에 대해 정보를 얻는다. 하지만, 결정적으로 GET은 조작을 할 수 없다. 예를 들어 HTTP GET to the URL/tasks/ 같은 것을 통해 값을 받아오면 JSON object형태로 리턴을 해주게 된다.
```json
[
  { "id": 3643, "summary": "Wash car" },
  { "id": 3697, "summary": "Visit gym" }
]
```

반대로, POST를 이용하면 무언가 만들어 줄수 있다. 어떤 값을 만들 때 HTTP POST to /tasks/ 같은 것을 해준다. 여기서 GET과 POST는 동사고 URL은 명사같다.

POST를 통해서 값을 만들어주게 될때 보통 JSON을 통해서 보내준다. 여기서 POST to /tasks/ 는 특정 필드를 포함시켜야한다. 예를들어,
```json
{
  "summary": "Get milk",
  "description": "Need to get a half gallon of organic 2% milk."
}
```
와 같은 방식으로 전달한다.

이를 진행하고 나면, 유용한 정보를 갖는 2차원의 값을 전달 받는다.

처음은 status code를 받게된다. 200, 404, 302 같은 경우는 긍정적인 숫자이다. 이 코드의 의미는 검색하면 간단히 찾을 수 있다.

다른 것은 response body를 받아올 수 있다. GET을 이용하면 HTML은 response body를 리턴해준다. API에선 이 값은 비어 있을 수도 있다.
- - -

다시 돌아와서 Talking REST부분을 진행하자.

다양한 포맷을 이용해 데이터를 주고 받을 수 있는데 RESTful API와 JSON format을 이용할 것이다.

예시로는 to-do list API를 사용한다고 생각하자. 여기서 이제 HTTP methods들의 예시를 보자.

- GET /tasks/
  - to-do list의 item들을 리턴해 줄 것이다. 예시로,
```json
{
    "id": "<item_id>",
    "summary": "<one-line summary>"
}
```

- GET /tasks/<item_id>/
  - 특정 to-do 항목에 대해 사용가능한 모든 정보를 가져온다. 예시로,
```json
{
    "id": "<item_id>",
    "summary": "<one-line summary>",
    "description" : "<free-form text field>"
}
