---
layout: post
title:  "REST API와 JSON format(5)"
date:   2019-09-17
excerpt: "Rest API에서 http request 처리 이어서"
tag:
- Hyun
- REST API
- JSON
- Python
comments: true
---

## REST API와 HTTPS requests 처리
- - -
(**API Integration in Python – Part 1**)[https://realpython.com/api-integration-in-python] 를 통한 공부를 이어서 하자.

### Talking REST
- - -

requests 모듈을 이용해서 이제 http method 들을 처리할 것이다. 그런데 이 사이트를 이용해서 공부를 진행하는 중 'https://todolist.example.com/' 이 사이트를 계속 사용하는데 접근할 수 없는 사이트로 없어진 것으로 보인다. 그래서 내용을 참조하는 형태로만 공부해야 할 것 같다.

- - -
처음으로 GET 을 이용해 보자.
```python
import requests

resp = requests.get('https://todolist.example.com/tasks/')
if resp.status_code != 200:
    # This means something went wrong.
    raise ApiError('GET /tasks/ {}'.format(resp.status_code))
for todo_item in resp.json():
    print('{} {}'.format(todo_item['id'], todo_item['summary']))
```
와 같이 작성할 수 있다.
- requests 모듈은 get 함수를 가지며 이는 HTTP GET과 같은 역할이다.
- response object는 json 메서드를 가지며 `json.loads()` 를 통해서 dictionary형태로 반환할 수 있다.
- `status_code`를 이용해서 리턴받은 코드값을 체크하고, 제대로 된 반환이 아니면 에러를 발생시킨다.

그러면 이제 새로운 값을 만드는 POST를 진행해보자. 여기서 필요한 값은 "summary"와 "description"이 반드시 필요하다.

```python
task = {"summary": "Take out trash", "description": "" }
resp = requests.post('https://todolist.example.com/tasks/', json=task)
if resp.status_code != 201:
    raise ApiError('POST /tasks/ {}'.format(resp.status_code))
print('Created task. ID: {}'.format(resp.json()["id"]))
```
- task에 dictionary형태로 작성을 해서 전달시 json으로 전달이 가능하다.
- requests의 post 함수가 HTTP POST역할을 한다.
- 그리고 201번 코드가 생성이 제대로 되었다는 것을 이야기하며, 이가 아닌 경우 에러가 발생했음을 나타내는 부분을 표시한다.
- json format 같은경우는 json 모듈을 이용해 `json.dump()`를 이용해도 된다.


이 이후 부분은 라이브러리를 만들어서 쉽게 파일을 주고 받을 수 있도록 만드는 부분이다. 함수를 설정해 작업을 추가하거나 받아오는 등의 일을 처리하도록 만드는 것이다.

- - -

여기서 GET을 통해서 정보를 받아오는 것은 알겠지만, POST를 통해서 정보를 보내 수정하는 부분에 대한 실습이 정확하게 이루어지지 않았다. post를 통해서 정보가 전달되는 것은 알겠지만, 그 정보가 전달되고 수정되는 것을 확인을 해볼 수가 없었다. 그래서 이 부분을 공부하려 한다.
