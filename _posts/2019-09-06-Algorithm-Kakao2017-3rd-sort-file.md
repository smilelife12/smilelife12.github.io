---
layout: post
title:  "[2017 KAKAO] 3차_파일명 정렬"
date:   2019-09-06
excerpt: "알고리즘 문제풀이"
tag:
- Hyun
- Algorithm
- Python
- wlecomekakao
comments: false
---

## 2017 KAKAO BLIND RECRUITMENT [3차] 파일명 정렬
- - -


### 문제설명
- - -
[문제링크](https://www.welcomekakao.com/learn/courses/30/lessons/17686)

과거 버전이 모두 저장되어 있어 이름순으로 정렬된 파일목록이 보기가 힘든 구조이다. 예를들어 ver-10.zip이 ver-9.zip 보다 먼저 표시되기 때문이다. '1.png','10.png','12.png','2.png' 이런식으로 정렬되어 보기힘들다.
그래서 이를 숫자를 반영하여 정리할 수 있는 정렬 기능을 구현하려한다.

파일명은 100글자이내로 영문 대소문자, 숫자, 공백, 마침표, 빼기부호로 이루어져있다. 영문자로 시작하며, 숫자 하나 이상을 포함한다.

파일명은 HEAD, NUMBER, TAIL 세가지로 구성된다.
- HEAD는 숫자가 아닌 문자로 이루어졌고, 한글자 이상
- NUMBER는 한글자에서 최대 5글자의 연속된 숫자로 이루어져 있고, 앞쪽에 0이 올수 있음(`0000`,`0101`과 같은 구조)
- TAIL은 나머지 부분으로, 여기에는 숫자가 나올 수도 있고, 아무글자도 없을 수도 있음

세 부분으로 분리후, 다음 기준에 따라 파일명 정렬
- HEAD를 기준으로 사전순으로 정렬한다. 대소문자는 구분하지 않는다.
- HEAD부분이 대소문자 차이 외에는 같을 경우, NUMBER의 숫자순으로 정렬하고 숫자 앞의 0은 무시. 012와 12는 같은 값으로 처리된다.
- HEAD와 NUMBER부분이 같을 경우 원래입력에 주어진 순서를 유지한다.

### 문제 풀기
- - -
문자 자체를 구분을 해주어야 한다. 숫자가 나오는 부분까지의 HEAD, 숫자부분의 NUMBER, 나머지 TAIL을 구분한다. 이를 저장을 어떤 방식으로 할지가 가장 중요해 보인다.
대소문자의 구분이 없기 때문에, HEAD를 그냥 키값으로 가져와 dictionary에 저장하는 방식은 아닌 것으로 보인다.

가장 쉽게 생각할 수 있는 방법은, 받아온 리스트에서 head, number, tail을 나눠서 또 다른 리스트로 만들고, 현재 내가 저장하는 배열에 그대로 저장해놓고 비교하는 방식이다.
- - -
이번 문제는 배울게 많은 문제였다. 우선 비교 함수를 적절하게 사용하지 못하고 있었는데, 이에 대한 이해를 넓혔고, 여기에 `lambda` 함수와 함수내의 함수를 사용하는 법에 대해서 공부하게 되었다.

문자를 나누고 비교하고, 또 그를 기준으로 또 비교하는 비효율적인 방법이 아닌 `sorted`함수에 적절한 함수를 끼워 넣어서 비교를 쉽게 할 수 있었고, 이 부분에 대한 이해를 넓혔다.
- - -
여기서 주요하게 사용했던것들
```python
sorted(list, key = function) # 리스트의 내용을 function함수를 이용해 리턴 받은 값을 이용해 정렬하였다.
lambda x: x*x
def fun(x):
  return x*x
# 위의 두개가 같은 동작을 하며, lambda 를 이용해서 간단한 함수를 만들어주는 방법을 잘 써야한다.
```

### 코드
- - -
<details markdown="1">
<summary>코드</summary>
``` python
def solution(files):
    print((lambda x: x.lower())(files[3]))

    def file_name(name):
        save_idx = 0
        idx = 0
        H = ""
        N = 0
        while True:
            if '0' <= name[idx] <= '9':
                save_idx = idx
                while idx < len(name) and '0' <= name[idx] <= '9':
                    idx += 1
                H += str(name[:save_idx])
                N = int(name[save_idx: idx])
                break
            idx += 1
        return H.lower(), N

    answer = sorted(files, key=file_name)
    return answer
```
