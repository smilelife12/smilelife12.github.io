---
layout: post
title:  "[2017 KAKAO] 3차_자동완성"
date:   2019-09-06
excerpt: "알고리즘 문제풀이"
tag:
- Hyun
- Algorithm
- Python
- wlecomekakao
comments: true
---

## 2017 KAKAO BLIND RECRUITMENT [3차] 자동완성
- - -


### 문제설명
- - -
[문제링크](https://www.welcomekakao.com/learn/courses/30/lessons/17685)

 한 번 입력된 문자열을 학습해 다음 입력 때 활용하려 한다.
 예를 들어, `go`입력시, 다음사영자가 `g`만 입력해도  `go`를 추천해주므로 `o`를 입력하지 않아도 된다. 앞부분이 같은경우는 다른문자가 나올때까지 입력해야 한다.
  학습된 문자들을 찾을 때 까지 몇글자를 입력해야 하는지 찾는다.

중복없는 단어 N개가 주어지며, 단어의 수 N은 2<= N <= 100,000, 단어들의 길이의 총합 L 범위는 2<= L <= 1,000,000 이다. 출력은 단어 찾을 때 입력해야 할 총 문자수를 리턴한다.


### 문제 풀기
- - -
 단어들이 주어지고 그 단어들이 안 겹치는 순간까지 몇개의 단어가 필요한지 찾는 문제로 보인다. 처음부터 단어를 저장할 때, 단어를 나누면서 딕셔너리 형태로 저장하는 것도 생각해 보았다. 단어수가 10만개이기 때문에 각각의 단어들의 최대 길이를 100이라고 해서 계산하면 천 만번의 확인이 필요하다. 아무래도 시간이 너무 오래 걸릴 것으로 보인다.

 저장 방식을 개선할 것인지, 탐색 방식을 개선할 것인지 정해야 할 것 같다. 단어들의 리스트를 주기 때문에 리스트를 탐색해야하는 데, 가장 쉽게 드는 생각은 한자리씩 늘려나가는 방식이다. 해당 문자를 리스트에서 찾고, 그 문자를 포함하는 단어가 하나라면 거기서 탐색이 끝나고 다음문자로 넘어가면된다.
- - -
 단어 처리하는 데 있어서, 헷갈린 부분이 많아서, 디테일한 부분에서 시간이 많이 걸렸고, 최종적으로 완성해본 코드도 3개문제에서 런타임에러와 실패가 나왔다. 그래서 계속 틀린점을 찾다보니, 틀린 부분은 보통 while문을 통해 중첩이 되면서 answer 값에 중첩된 값들 때문에 틀리는 경우가 다반사 였다. 그래서 while문들을 모두 제거하니 성공하였다.

- - -
 너무 단순히 접근한것 같아서 해설을 보았다.
 트라이(trie)자료구조를 이용하는 방법이 예시로 나왔었다. 과거 학교 수업 내용에서 들었던 자료 구조였다. 트리형태로 같은 접두어를 가지는 단어를 효과적으로 저장하는 방식이다.

 이를 우선적으로 간단히 표현해 보기위해서 `dictionary` 를 이용하고 단어마다 전체 탐색을하며, 저장을 해주는 방식을 시도해 보았다.
 <details markdown="1">
 <summary>시도한 코드</summary>
 ``` python
 def solution(words):
    answer = 0
    dict = {}
    for word in words:
        for i in range(1, len(word)+1):
            w = word[:i]
            if w in dict:
                dict[w] += 1
            else:
                dict[w] = 1
    # print(dict)
    for word in words:
        for i in range(1, len(word)+1):
            w = word[:i]
            if dict[w] == 1 or i == len(word):
                # print(w)
                answer +=i
                break
    return answer

 ```
코드 자체는 굉장히 간단해 졌고 테스트 코드들은 다 성공했다. 하지만 실제 시행에서 시간 초과가 3번이 나오게 되었다. 트라이 자료구조를 만드는걸 새롭게 해야할 것 같다. 노드를 만들어서 next() 를 통해 연결해서 이용을 하면 좀 더 효율적으로 트라이 구조를 사용할 수 있을 것 같다.

간결한 코드를 보게되어 가져왔다.
```python
class Trie():
    def __init__(self):
        self.next = dict()
        self.count = 0

def solution(words):
    answer = 0
    tree = Trie()
    for word in words:
        sub = tree
        for i, w in enumerate(word):
            sub.count += 1
            if w not in sub.next:
                sub.next[w] = Trie()
            sub = sub.next[w]
            if i == len(word) -1:
                sub.count += 1
    # print(dict)
    for word in words:
        sub = tree
        cnt = 0
        for i, w in enumerate(word):
            if sub.count == 1:
                answer += cnt
                break
            elif i == len(word)-1:
                answer += cnt + 1
                break
            else:
                sub = sub.next[w]
                cnt += 1
    return answer
```
- - -
그런데 생각외로 많은 처리시간의 차이는 보이지 않는다. 아무래도 나는 받아온 값들을 정렬하는 대신, 바로 찾으며 분류를 하는 방식을 택해서 그런 것 같다.

- - -
여기서 주요하게 사용했던것들
```python
list = [a,b,c,d,e]
list = list[1:3] # 리스트에 대한 슬라이싱을 통해 원하는 위치 얻어옴
list.remove(c) # 리스트에서 해당 값이 먼저나타나는 부분을 제거한다.
for i, w in enumerate(word):
  print(i, w) # enumerate 함수는 해당 iterate값의 index값과 index에 해당하는 값을 출력해 준다.
```

### 코드
- - -
<details markdown="1">
<summary>코드</summary>
``` python
def solution(words):
    answer = 0
    words.sort()
    answer = find_word(words[0], words[1:], 0, answer)
    return answer


def find_word(word, ws, cnt, a):
    if len(word) == cnt:
        if len(ws) != 0:
            a += cnt
            t_w = ws[0]
            ws.remove(t_w)
            a = find_word(t_w, ws, cnt, a)
            return a
    li = []
    ot = []
    for t_w in ws:
        if len(t_w) < cnt or t_w[cnt] != word[cnt]:
            ot += [t_w]
        else:
            li += [t_w]
    if len(li) == 0:
        a = a + cnt + 1
    else:
        a = find_word(word, li, cnt+1, a)
    if len(ot) != 0:
        t_w = ot[0]
        ot.remove(t_w)
        if len(ot) == 0:
            a = a + cnt + 1
        else:
            a = find_word(t_w, ot, cnt, a)
    return a
```
