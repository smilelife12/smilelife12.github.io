---
layout: post
title:  "2019 카카오 2차 블라인드 오프라인 테스트"
date:   2019-09-19
excerpt: "엘레베이터 문제 풀어보기"
tag:
- Hyun
- REST API
- JSON
- Python
comments: true
---

## 2019 카카오 블라인드 공채 2차 오프라인 Elevator 문제
- - -

오프라인 테스트를 대비하기 위해, 작년 오프라인 테스트 문제를 풀어보려한다.

문제해설은 [카카오 공식 블로그](https://tech.kakao.com/2018/10/23/kakao-blind-recruitment-round-2/) 여기서 문제해설을 볼 수 있고, 이를 직접 풀어보기 위한 [깃허브](https://github.com/kakao-recruit/2019-blind-2nd-elevator)페이지가 있다. 그래서 여기에 연결해서 Getting Started 부분을 따라 Clone하고 실행한다. example 코드까지 실행해보았고, 이를 분석하기 위해 `example.py` 코드를 보자.

```python
import requests


url = 'http://localhost:8000'


def start(user, problem, count):
    uri = url + '/start' + '/' + user + '/' + str(problem) + '/' + str(count)
    return requests.post(uri).json()


def oncalls(token):
    uri = url + '/oncalls'
    return requests.get(uri, headers={'X-Auth-Token': token}).json()


def action(token, cmds):
    uri = url + '/action'
    return requests.post(uri, headers={'X-Auth-Token': token}, json={'commands': cmds}).json()


def p0_simulator():
    user = 'tester'
    problem = 0
    count = 2

    ret = start(user, problem, count)
    token = ret['token']
    print('Token for %s is %s' % (user, token))
```
기본적으로 이런식의 구성을 보이게 된다.

여기서 action의 command를 통해서 엘레베이터를 운행하는 것으로 보이는데, example에서는 이미 내용을 아는 것으로 가정하고, command를 정해서 전달하여 처리한다.

그리고 여기에 있는 메서드들은 작동하는 API를 참고하여 만들어진 것으로 보인다.
- - -
Start Api의 경우
```
POST /start/{user_key}/{problem_id}/{number_of_elevators}
```
이런 식으로 받아오도록 하게 되어있다.

이를 Python에서
```python
def start(user, problem, count):
    uri = url + '/start' + '/' + user + '/' + str(problem) + '/' + str(count)
    return requests.post(uri).json()
```
로 받아오도록 한다. 이를 통해서 user를 위한 엘레베이터와 토큰 등의 값들을 전달해주는 것으로 보인다. return 값의 예는
```json
{
  "token": "TVqpM5MX0amQqhrYKd3ZwMZn3Im6y4ovJwEa_A1-2d6mBpl4QhwJmmkrrvG4MsaD+mG44dL0aC0RNYL",
  "timestamp": 0,
  "elevators": [
    {
      "id": 0,
      "floor": 1,
      "passengers": [],
      "status": "STOPPED"
    },
    {
      "id": 1,
      "floor": 1,
      "passengers": [],
      "status": "STOPPED"
    }
  ],
  "is_end": false
}
```
이므로 엘레베이터의 숫자와 토큰, 등의 값들을 전해준다.
- - -

On Calls API의 경우는 현재 timestamp를 기준으로 call 목록을 반환한다. 승객이 탑승하면 call에서 사라지게 된다.즉, call의 id를 엘레베이터로 옮기게 되면 call목록이 없어진다는 것이다. 그리고 승객이 원하는 층에서 내리지 못하면 call이 새롭게 추가된다.

```
GET /oncalls
X-Auth-Token: {Token}
```
와 같은 방식으로 진행하게 되는데 이를 파이썬으로 옮긴것이
```python
def oncalls(token):
    uri = url + '/oncalls'
    return requests.get(uri, headers={'X-Auth-Token': token}).json()
```
라고 전달된다. `headers`를 통해서 헤더의 토큰값을 전달하는 것이 주요하고, 이를 전달하지 않으면 값을 전달 받지 못하게 된다.

그리고 return 값의 형태는
```json
{
"token": "TVqpM5MX0amQqhrYKd3ZwMZn3Im6y4ovJwEa_A1-2d6mBpl4QhwJmmkrrvG4MsaD+mG44dL0aC0RNYL",
"timestamp": 0,
"elevators": [
  {
    "id": 0,
    "floor": 1,
    "passengers": [],
    "status": "STOPPED"
  },
  {
    "id": 1,
    "floor": 1,
    "passengers": [],
    "status": "STOPPED"
  }
],
"calls": [
  {
    "id": 0,
    "timestamp": 0,
    "start": 6,
    "end": 1
  }
],
"is_end": false
}
```
이런식으로 calls가 추가되어 돌려받게 된다. 여기에 `calls`를 보면 id와 추가된 시간인 timestamp, 출발층인 start, 하차 층인 end가 추가되게 된다.

- - -
Action API는 엘레베이터에 명령을 실행한다. `commands`에 모든 명령이 포함되어야 하며, 각 엘레베이터당 하나의 명령만 전달가능하다.
```
POST /action
X-Auth-Token: {Token}
Content-Type: application/json
```
와 같은 형태로 전달해야하며,
```python
def action(token, cmds):
    uri = url + '/action'
    return requests.post(uri, headers={'X-Auth-Token': token}, json={'commands': cmds}).json()
```
이런 식으로 전달이 가능하다. cmds에 command의 명령 배열이 추가되며 예시로는

```json
{
  "commands": [
    {
      "elevator_id": 0,
      "command": "ENTER",
      "call_ids": [0]
    },
    {
      "elevator_id": 1,
      "command": "STOP"
    }
  ]
}
```
와 같은 형태로 전달하게 된다. 위와 같이 전달하기 위해서는 `commands`안의 값만 전달해 주면 된다.

- - -
이제 각각의 Resource들을 살펴보자.

Elevator의 경우 엘레베이터 하나의 상태를 표현하며
```json
{
  "id": 0,
  "floor": 1,
  "passengers": [],
  "status": "STOPPED"
}
```
의 형태로
- id : `integer`로 해당 엘레베이터 번호
- floor : `integer`로 해당 엘레베이터의 현재 위치(층)
- passengers : `array of call`로 해당 엘레베이터에 타고 있는 승객들 표현하는 call의 목록
- status : `string` 해당 엘레베이터 상태

Call의 경우
call을 표현하며
```json
{
  "id": 0,
  "timestamp": 0,
  "start": 1,
  "end": 2
}
```
의 형태로
- id : `integer`로 call의 고유 번호
- timestamp : `integer`로 해당 call이 발생한 timestamp
- start : `integer`로 출발 층
- end : `integer`로 가려는 층

Command의 경우
엘레베이터의 제어 명령을 표현하며
```json
{
  "elevator_id": 0,
  "command": "ENTER",
  "call_ids": [0]
}
```
- elevator_id : `integer`로 명령을 실행할 엘레베이터의 번호(`Elevator`의 `id`)
- command : 'sring'으로 실행할명령(`STOP`,`OPEN`,`ENTER`,`EXIT`,`CLOSE`,`UP`,`DOWN`중 하나)
- call_ids : `array of integer`로 태우거나 내려줄 `Call`의 `id`

- - -
위의 리소스를 이용해서 알고리즘을 짜는 것이 목적이다. 결국 종료가 되는 것은, `is_end` 가 `True`로 바뀌는 구간까지 진행을 하는 것이다.

### 풀어보자!
- - -
1번문제부터 차근차근 생각해보자.
```
출근을 위해 집을 나선 라이언. 라이언이 살고있는 어피치 맨션은 총 5층 높이의 작은 맨션이다. 이동이 많지 않은 기본적인 엘리베이터 동작을 구현하여 승객을 수송해보자!

조건

엘리베이터의 최대 수용인원(=Call) : 8명
건물의 최고층 : 5층
Call 수 : 6개
```
1번 문제는 우선 제약이 적다. 5개층에서 6개의 call이 발생하게 된다. 그리고 내가 해야 할 것은, 엘레베이터를 어떻게 움직일 것인가가 문제이다. call을 진행하면, timestamp는 진행이 안되고 해당하는 call만 불러오게 된다. 그리고 action을 해야 timestamp가 증가한다는 것 까지 테스트를 통해서 알게 되었다.

즉, call을 진행하고 그에 맞는 action을 해야한다는 지극히당연한 것을 알게 되었다.
- - -
그럼 문제를 접근해보자. call을 통해서 승객의 값을 받게 될 것이고, 내가 가지고 있는 elevator의 상태가 각각 다르게 되어있을 것이다.  
elevator의 상태중 `UPWARD`, `DOWNWARD`,`OPEND`의 경우는 무언가 진행을 하고 있다는 뜻이고, `STOPPED`는 대기중이라는 뜻이다.  
- `UPWARD`나 `DOWNWARD`의 경우는 승객을 태우러 가거나, 내려주러 가는 중이라는 뜻이다. 다음 명령은 `STOP` 혹은 해당에 맞는 `UP`이나 `DONW`이 계속해서 진행 될 것이다.
- `OPEND`의 경우는 승객을 내려주거나 태우고 있다는 뜻이고, 이 다음명령은 `ENTER`혹은 `EXIT`이 우선시 될 것이고, 이 명령 중 행할 것이 없으면 `CLOSE`를 통해서 `STOPPED`상태로 넘어가도록 해준다.
- `STOPPED`의 경우는 대기중인 값으로 `CALL`에 대한 명령을 처리할 수 있도록 한다.

그럼 이 엘레베이터의 움직임을 어떤식으로 접근할 것인가. 일단은 기본적인 엘레베이터를 생각해보자. 가장 먼저 버튼을 누른 사람에 따라 이동방향이 정해지고, 이동방향으로 가는 길에 사람들의 목적지나 출발지가 있으면 태우거나 내리는 일을 처리하게 된다.
이제 리소스를 확인하면 `CALL`을 통해서 받아온 값들이 존재한다. 엘레베이터의 상태, CALL의 목록들. 기본적으로 엘레베이터는 `CALL`에 의해서 움직이게 된다. 그럼 `CALL`의 목록을 먼저 체크해서, 대기중인 `CALL`에 대해 엘레베이터를 할당하도록 하고, 그에 따라서 움직인다. 그럼 기본적인 엘레베이터의 룰을 따르기 위해서는 처음으로 나타나는 `CALL`의 ID를 기준으로 엘레베이터는 움직여야한다. 그러면 그 `CALL` 에 대한 정보를 해당 엘레베이터에 저장하는 값도 만들어줘야 할 것 같다. 그래야 그 엘레베이터가 해당 `CALL`에 도달할 때 까지 해야할 일들을 처리할 수 있다.

정리하면, 각 엘레베이터에 기준이 되는 ID들을 저장하자. 그리고 해당 ID의 일을 처리하는 동안의 `CALL`은 할 수 있으면 하고 아니면 내버려 두고 진행하도록 하자.
- - -
