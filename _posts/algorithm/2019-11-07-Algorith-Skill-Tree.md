---
layout: post
title:  "[Programmers] 스킬트리"
date:   2019-11-07
excerpt: "알고리즘 문제풀이"
tag:
- Hyun
- Algorithm
- Python
- Programmers
comments: true
---

## [2018 윈터코딩] 스킬트리
- - -
면접준비때매 너무 오랫동안 못했는데 다시 시작하자.

### 문제설명
- - -
[문제링크](https://programmers.co.kr/learn/courses/30/lessons/49993#fnref1)
어떤 스킬을 배우기전에 반드시 배워야하는 스킬이 있다. 이를 선행 스킬이라 하며, 선행스킬을 반드시 배워야 다음 스킬을 배울 수 있다.  
선행 스킬 순서를 주고, 스킬 트리로 이루어진 배열이 주어진다. 스킬트리 중 가능한 개수를 반환하는 프로그램을 만들어주자.  


### 문제 풀기
- - -
해당 배열의 스킬트리가 주어질 때, 스킬 순서 안에 해당 스킬이 존재하면 인덱스 값을 설정해서 그 인덱스와 일치하는지 확인하도록 하면 될 것 같다. 일치하면 인덱스를 하나씩 증가시켜서 검사 위치를 바꿔주는 방식으로 진행한다.

- - -
여기서 주요하게 사용했던것들
```
```

### 코드
- - -
<details markdown="1">
<summary>코드</summary>
``` python
def solution(skill, skill_trees):
    answer = 0
    for s in skill_trees:
        idx = 0
        clear = True
        for c in s:
            if c in skill:
                if c is skill[idx]:
                    idx += 1
                else:
                    clear = False
                    break
        if clear:
            answer += 1
    return answer
```
