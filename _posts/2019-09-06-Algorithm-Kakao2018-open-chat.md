---
layout: post
title:  "[2018 KAKAO] 오픈 채팅방"
date:   2019-09-06
excerpt: "알고리즘 문제풀이"
tag:
- Hyun
- Algorithm
- Python
- wlecomekakao
comments: true
---

## 2018 KAKAO BLIND RECRUITMENT 오픈 채팅방
- - -


### 문제설명
- - -
[문제링크](https://www.welcomekakao.com/learn/courses/30/lessons/42888)

채팅방에 누군가 들어오면 `"[닉네임]님이 들어왔습니다."` 가 출력 되고, 나가면 `"[닉네임]님이 나갔습니다."`가 출력된다.

닉네임을 변경하는 방법은 두 가지이다.
- 채팅방을 나간 후, 새로운 닉네임으로 다시 들어간다.
- 채팅방에서 닉네임을 변경한다.

닉네임 변경할 때는 기존에 채팅방에 있던 메세지의 닉네임도 전부 변경된다.

채팅방을 들어오고 나가거나, 닉네임 변경 기록을 담은 문자열 배열 recor가 매개변수로 주어질 때, 주어진 기록 처리 후, 최종적으로 방을 개설한 사람이 보게되는 메세지를 출력하자.

record의 형태
- 모든 유저는 [유저아이디]로 구분
- "Enter [유저아이디] [닉네임]"
- "Leave [유저아이디]"
- "Change [유저아이디] [닉네임]"
- 첫 단어는 위의 셋 중 한개이다.
- 공백으로 구분되며, 알파벳과 숫자로 이루어짐.


### 문제 풀기
- - -
파이썬에 있는 dictionary를 이용해서 유저아이디를 key, 닉네임을 value로 설정하면 될것 같다. 리스트에 저장할 때 value를 이용해 입력했을 때, 다시 변경이 된다면 간단하게 풀 수 있는 문제로 보여서 테스트 해보았다.
- - -
간단히 해결되지 않았다. 해당 값은 딥 카피 형태로 되어 참조 형태로 저장되는 것이 아닌 것으로 보인다.

다른 방식으로 리스트에 유저 아이디만 저장시키고, 해당 행동만 적용 시키는 방식으로 record의 내용을 처리하려한다.
- - -

생각대로 쉽게 풀린 편이다. 첫 번째 내용에 따라 Enter라면, 유저아이디를 기준으로 닉네임 입력 및 변경을 하고, '님이 들어왔습니다.' 내용을 저장시키고, Leave면 '님이 나갔습니다.'를 저장, Chage의 경우는 닉네임만 변경해서 리스트를 저장해놓고, 각 유저아이디에 대한 dictionary를 지정해서 answer에다가 해당 유저아이디에 해당하는 닉네임을 전송시키는 방식을 채택했고, 정상 작동했다.  


- - -
여기서 주요하게 사용했던것들
```python

```

### 코드
- - -
<details markdown="1">
<summary>코드</summary>
``` python
user = {}
do = {1: "님이 들어왔습니다.", 2: "님이 나갔습니다."}

def solution(record):
    answer = []
    temp = []
    for s in record:
        act = s.split(" ")
        if act[0] == "Enter":
            user[act[1]] = act[2]
            temp += [[act[1], do[1]]]
        elif act[0] == "Leave":
            temp += [[act[1],do[2]]]
        else:
            user[act[1]] = act[2]
    for d in temp:
        line = ""
        line += user[d[0]]+d[1]
        answer += [line]
    return answer
```
