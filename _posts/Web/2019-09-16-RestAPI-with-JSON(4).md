---
layout: post
title:  "REST API와 JSON format(4)"
date:   2019-09-16
excerpt: "Rest API에서 http request 처리"
tag:
- Hyun
- REST API
- JSON
- Python
comments: true
---

## REST API와 HTTPS requests 처리
- - -
공부를 위해 구글링중 (**API Integration in Python – Part 1**)[https://realpython.com/api-integration-in-python] 를 찾게 되었다. HTTPS request에 대한 처리를 하는 부분으로 보여 시작을 해보려한다.

### How to Make Friends and Influence APIs
- - -
HTTPS request에 의해 여러 요청들이 처리가 된다. 그럼 이 HTTPS requests들을 코드에서 어떻게 사용할까. SDK나 라이브러리를 사용한다. 그래서 파이썬 코드를 RESTful API를 이용하는 것을 해보자.

### Talking REST
- - -
먼저 REST에 대해 알아보자.
### Appendix: REST in a nutshell
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
```

- POST /tasks/
  - 새로운 to-do item을 만들어준다 POST에 JSON에 "summary"와 "description" 필드를 만들어서 전달해 준다. 성공시 201코드를 리턴해 준다.

- DELETE /tasks/<item_id>
  - item을 진행 했다는 표시를 한다. 이후 GET /tasks/를 보면 보이지 않을 것이다.

- PUT /tasks/<item_id>/
  - 이미 존재하는 값을 수정한다. body에는 아까 POST처럼 "summary"와 "description"을 전달한다.


이를 이제 파이썬의 requests 라이브러리를 이용하여 진행한다.
