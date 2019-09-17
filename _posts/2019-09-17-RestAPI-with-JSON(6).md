---
layout: post
title:  "REST API와 JSON format(6)"
date:   2019-09-17
excerpt: "HTML에서 JSON을 어떻게 처리하는가"
tag:
- Hyun
- REST API
- JSON
- Python
- JavaScript
comments: true
---

## JSON 파일은 어떻게 처리되는가?
- - -

POST를 이용해 정보를 전달하는 것도 알았고, GET을 통해 정보를 받을 수 있는 것도 알았다. 그런데 POST를 이용해서 보낸 자료가 그 uri에 전달이 된 후 어떻게 변화가 이루어지는 지를 아직 모르겠다. 그래서 좀 더 찾아 보았다.

그렇게 찾은 블로그가 [Poiemaweb](https://poiemaweb.com/js-rest-api) 이라는 블로그이다. 여기서 REST API에 대해서 설명이 나온다. 물론 JavaScript를 통해 진행하지만, 어떤식으로 되는지 참고를 위해 보게되었다.

- - -
### TEST를 위한 서버 구축
- - -
[참조](https://poiemaweb.com/json-server)

이 블로그에서는 json-server를 이용해서 서버를 구축하였다. 그래서
```bash
$ mkdir json-server-exam && cd json-server-exam
$ npm init -y
$ npm install json-server --save-dev
```
를 통해서 json server 설치를 해주고, json 예제를 작성한다.
```json
{
  "todos": [
    {
      "id": 1,
      "content": "HTML",
      "completed": true
    },
    {
      "id": 2,
      "content": "CSS",
      "completed": false
    },
    {
      "id": 3,
      "content": "Javascript",
      "completed": true
    }
  ],
  "users": [
    {
      "id": 1,
      "name": "Lee",
      "role": "developer"
    },
    {
      "id": 2,
      "name": "Kim",
      "role": "designer"
    }
  ]
}
```
라는 내용이다.
이제 json-server를 이용해서 확인해보자.
```bash
## 기본 포트(3000) 사용
$ json-server --watch db.json
## 포트 변경
$ json-server --watch db.json --port 5000
```
를 통해 변경하고, 명령어를 줄이기 위해, package.json을
```json
{
  "name": "json-server-exam",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "start": "json-server --watch db.json"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "json-server": "^0.15.1"
  },
  "dependencies": {},
  "description": ""
}
```
와 같이 scripts 부분을 수정한다. 이제 `npm start`를 이용해 서버를 실행시킨다.


여기까지는 블로그와 동일하지만, 이제 이를 python을 이용해보자.

먼저 GET은
```python
import requests,json

res = requests.get('http://localhost:3000/todos')

print(res.json())
```
를 통해 실행할 수 있다.

그러면 결과물은
![GET결과]({{site.url}}/assets/img/posting0917/img1.png)

와 같이 나타난다. 여기서 봐야할 점은 json에서 객체의 key 값으로 주어진 todos가 위치로 나타나지며, 거기에 포함된 value 들이 출력이 된다는 점이다.

그러면 다음으로 POST는
```python
import requests,json


data = {"id": 4, "content": "Angular", "completed": True}
res = requests.post('http://localhost:3000/todos', data=data)

print(res.json())
```
와 같이 data를 dictionary 형태로 전달하거나 혹은 json 형태로 변경하여 전달해도 된다. 그렇게 되면 결과는
![POST결과]({{site.url}}/assets/img/posting0917/img2.png)

와 같이 나타난다. id 4번이 추가된것을 볼수 있다.

이제 수정을 해주는 PUT을 해보자. 여기서 조금 버벅거렸는데, `id`가 4인 방금 입력한 값을 수정할 것인데 `uri` 값을 그대로 진행을 하게 되면, `404` 코드가 돌아오며 오류가 난다. 여기서 중요한 점인 `idempotent`가 나타난다.

**idempotent**
- - -
PUT과 POST를 이해하는 데 필요한 요소이다. ``f(x) = f(f(x))``  
와 같은 수식을 의미하는데 몇번이고 같은 연산을 해도 같은값이 나온다는 것이다.

여기서 **POST** 는 **idempotent하지 않다.** POST는 리소스의 위치를 지정하지 않았을 때, 리소스를 생성하기 위해 사용하는 연산이다. 이 연산을 수행하면, 해당 리소스 위치의 자식위치의 다양한 곳에 값이 생성될 수 있다.

반면, **PUT** 은 **idempotent하다.** 특정 위치가 명확히 지정된 다음 요청을 해주는 것이다. 그리고 만약에 없다면 생성을 하기도하고 업데이트를 위해 사용할 수 있다.
- - -

그래서 이러한 특징 때문에 방금전의 GET이나 POST처럼 ``http://localhost:3000/todos`` 위치를 받아오게 되면 오류가 발생한다. 일부러 그런지는 모르겠지만, `id` 가 4인 값이 4번째의 위치에 존재하기 때문에 그위치를 특정 지어주고 값에 변화를 주는 것이다. 그렇기 때문에 ``http://localhost:3000/todos/4``위치에 PUT을 진행해야 한다.

그래서 코드는
```python
import requests,json


data = {"id": 4, "content": "React", "completed": False}
res = requests.put('http://localhost:3000/todos/4', data=data)

print(res.json())
```
와 같이 작성하고 결과는,
![PUT결과]({{site.url}}/assets/img/posting0917/img3.png)

과 같이 나타나게 된다.

PATCH도 실행해보면,
```python
import requests,json


data = {"completed": True}
res = requests.patch('http://localhost:3000/todos/4', data=data)

print(res.json())
```
결과는
![PATCH결과]({{site.url}}/assets/img/posting0917/img4.png)

DELETE의 경우는,
```python
import requests,json

res = requests.delete('http://localhost:3000/todos/4')

print(res.json())
```
결과는
![DELETE결과]({{site.url}}/assets/img/posting0917/img5.png)

이렇게 간단하게 json파일에 대한 처리를 하는 것을 배웠고, 어느정도 눈을 뜨게 된거같다. 이제 본격적으로 문제를 풀어보자.
