---
layout: post
title:  "[2017 KAKAO] 3차_N진수게임"
date:   2019-09-04
excerpt: "알고리즘 문제풀이"
tag:
- Hyun
- Algorithm
- Python
- wlecomekakao
comments: false
---

## 2017 KAKAO BLIND RECRUITMENT [3차] N진수 게임
- - -


### 문제설명
- - -
[문제링크](https://www.welcomekakao.com/learn/courses/30/lessons/17687)
 둥글게 앉아서 숫자를 하나씩 차례대로 말하는 게임으로 규칙은 다음과 같다.
  1. 0부터 시작해서 차례대로 말하고, 열번째 사람은 9를 말한다.
  2. 10 이상 부터는 한 자리씩 끊어서 말한다. 즉, 열한 번째는 1, 열 두번째는 0을 말한다.
  예를 들어 `0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 1, 0, 1, 1, 1, 2` 와 같은 방식으로 진행된다.
  이를 10진수가 아닌 모든 진법으로 게임을 하려 한다. 이를 자신의 차례에 어떤 숫자를 말해야하는 지 찾는 프로그램을 구현하자.

  진법 `n`, 미리 구할 숫자의 갯수 `t`, 게임 참가 인원 `m`, 자신의 순서 `p` 가 주어진다.
  출력시는 `t`개를 공백없이 출력, 단 `10`~`15`는 `A`~`F`로 대체하여 출력한다.


### 문제 풀기
- - -
진법 변환을 하고, 각 변환 값을 어떤 식으로 끊어서 진행할 지에 대해서 처리하는 게 관건으로 보인다. 처음에 생각 나는 방식은 미리 구할 숫자 `t`가 될 때까지 늘려나가는 값 `cnt`를 정해놓고 그 값을 이제 `n`진수로 변환을 시킨 후, 변환 된 값의 길이만큼 계속 차례를 진행하며 말하도록 한다.
 그러면 이 때 또 필요한 값은 차례에 대한 값을 정해야 한다. 해당 차례를 나타내는 값을 `who`라고 정하고 인원의 크기 `m`을 지나가면 0으로 바꾸는 식으로 실행하자. 그리고 `who`가 자신의 순서 `p`가 되면 해당 문자를 `answer`값에 추가하고, `t`의 숫자를 늘려준다.

  그럼 구현해야 하는 것은 n진수로 변환하는 함수와 그 값을 순서에 맞게 루프를 돌리는 함수가 필요로 한다.
- - -
생각한대로 꽤나 잘풀려서 별로 막히지 않았다. 한 번 헷갈린 것은, 진수법을 바꾸고 나서, 역순으로 출력을 해야하는 것을 착각했다. 그 순서만 바꾸니 잘 출력되었다. 그리고 10 이상의 숫자는 dictionary를 이용해서 처리했다.
- - -
여기서 주요하게 사용했던것들
```python
dic = {10: 'A', 11: 'B', 12: 'C', 13: 'D', 14: 'E', 15: 'F'}# 10이상의 숫자 처리
def changed(cnt, n):
    num = []
    while cnt > 0:
        x = cnt % n
        cnt = int(cnt/n)
        if x >= 10:
            num += [dic[x]]
        else:
            num += [str(x)]
    return num
# 인수 변환, 역순으로 출력되므로 다른 방식도 가능.
```

### 코드
- - -
<details markdown="1">
<summary>코드내용</summary>
``` python
dic = {10: 'A', 11: 'B', 12: 'C', 13: 'D', 14: 'E', 15: 'F'}


def solution(n, t, m, p):
    answer = ''
    cnt = 0
    who = 0
    p -= 1
    while t > 0:
        if cnt < n:
            if who == p:
                if cnt < 10:
                    answer += str(cnt)

                else:
                    answer += dic[cnt]
                t -= 1
            who += 1
            if who == m:
                who = 0

        else:
            num = changed(cnt, n)
            i = len(num)-1
            while i > -1:
                x = num[i]
                i -= 1
                if who == p:
                    answer += x
                    t -= 1
                    if t == 0:
                        break
                who += 1
                if who == m:
                    who = 0
        cnt += 1
    return answer


def changed(cnt, n):
    num = []
    while cnt > 0:
        x = cnt % n
        cnt = int(cnt/n)
        if x >= 10:
            num += [dic[x]]
        else:
            num += [str(x)]
    return num

# print(changed(15,16))
# print(solution(2,4,2,1))
```
