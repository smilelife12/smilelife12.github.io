---
layout: post
title:  "[2018 KAKAO] 실패율"
date:   2019-09-06
excerpt: "알고리즘 문제풀이"
tag:
- Hyun
- Algorithm
- Python
- wlecomekakao
comments: false
---

## 2018 KAKAO BLIND RECRUITMENT 실패율
- - -


### 문제설명
- - -
[문제링크](https://www.welcomekakao.com/learn/courses/30/lessons/42888)

게임의 난이도 조절을 위해서 실패율을 구하려한다.
실패율은 다음과 같이 정의한다.
- 스테이지에 도달했으나 아직 클리어하지 못한 플레이어의 수/ 스테이지에 도달한 플레이어 수

전체 스테이지의 개수 N, 게임을 이용하는 사용자가 현재 멈춰있는 스테이지의 번호가 담긴 배열 stages가 매개변수로 주어질 때, 실패율 높은 스테이지 부터 내림차순으로 스테이지의 번호가 담겨있는 배열을 return 하도록한다.

제한사항
- 스테이지 개수는 1이상 500이하
- stages의 길이는 1이상 200,000 이하
- stages에는 1 이상 N+1 이하의 자연수가 담겨있다.
  - 각 자연수는 도전중인 스테이지 번호를 나타냄
  - N+1번은 마지막까지 클리어한 사용자를 나타냄
- 실패율이 같은 스테이지가 있으면 작은 번호의 스테이지가 먼저오도록 한다.
- 도달한 유저가 없는 경우 `0`으로 정의한다.

### 문제 풀기
- - -
 stages의 숫자가 유저의 숫자랑 같기 때문에, 이를 이용하면 된다. 그리고 dictionary를 이용해서 값 변화를 주는 것이 적절할 것으로 보인다. 그리고 스테이지의 실패한 사람의 숫자는 해당 스테이지에 머무르고 있는 사람의 숫자와 같다고 보면되며, 해당 스테이지보다 큰 값을 가진 사람은 그 스테이지를 깼다고 생각하면 된다.

 즉 스테이지번호를 key로 가지며, 스테이지를 도달한 사람과 스테이지를 클리어 한사람을 value로 갖는 dictionary를 만들어주면 되는 것으로 보인다.
 이를 모두 계산하고, value를 불러와서 실패율을 기준으로 내림차순으로 계산하며, 이를 key값으로 출력해 주면된다.

- - -
쉽게 풀 수 있다고 생각했는데, 가장큰 파일을 불러올 때 시간초과가 뜨게된다. 나머지는 전부 통과하는데 안되는 걸 보면 처리방법에서 문제가 있는 것으로 보인다. 다른 풀이들과 비교해보니 유저가 해당 스테이지에 도달했음을 for문을 돌며 모든 값에 추가를 해주다 보니 시간이 오래 걸리는 것으로 보인다.

여기서 차이점을 보인것은 실패를 계산하는데 있어서 for문을 한번 돈다는 점이다. 가장 눈에 띄게 푼 문제는 모든 사람이 1스테이지 부터 진행 하기 때문에 유저숫자를 기준으로 진행하며, stages 내에 해당 스테이지와 같은 값을 가지면 거기에 머물러 있으며 깨지 못했다는 뜻이다. 즉, 그 숫자만큼 유저에서 제외하면 이제 다음 스테이지로 진행한 사람의 숫자를 구할 수 있다는 것이다. 이를 반복하며 클리어한 유저가 0 이되면 계속 실패율을 0으로 저장한뒤, 그 저장해 놓은 dictionary의 값을 sorted함수를 이용해 역순으로 출력해주면 된다.

- - -
여기서 주요하게 사용했던것들
```python
sorted(dictionary, key=lambda x: dictionary[x], reverse=True)
# dictonary 값들은 굳이 key값들을 뽑아내지않고, value간의 비교가 가능하다.
```

### 코드
- - -
<details markdown="1">
<summary>시도한 코드</summary>
``` python
def solution(N, stages):
    answer = []
    clr = [0] * (N+1)
    user_in = [0] * (N+1)
    rate ={}
    for user in stages:
        for i in range(1, user):
            clr[i] += 1
            user_in[i] += 1
        if user <= N:
            user_in[user] += 1
    for i in range(1, N+1):
        if user_in[i] == 0:
            rate[i] = 0
        else:
            rate[i] = round((user_in[i]-clr[i])/user_in[i], 20)

    answer = sorted(rate, key=lambda x: rate[x], reverse=True)
    return answer

```
<details markdown="1">
<summary>정답 코드</summary>
``` python
def solution(N, stages):
    result = {}
    denominator = len(stages)
    for stage in range(1, N+1):
        if denominator != 0:
            count = stages.count(stage)
            result[stage] = count / denominator
            denominator -= count
        else:
            result[stage] = 0
    return sorted(result, key=lambda x : result[x], reverse=True)

```
