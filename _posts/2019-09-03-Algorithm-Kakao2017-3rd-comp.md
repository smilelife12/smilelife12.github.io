---
layout: post
title:  "[2017 KAKAO] 3차_추석 트래픽"
date:   2019-09-03
excerpt: "알고리즘 문제풀이"
tag:
- Hyun
- Algorithm
- Python
- wlecomekakao
comments: false
---

## 2017 KAKAO BLIND RECRUITMENT [3차] 압축
- - -


### 문제설명
- - -
[문제링크](https://www.welcomekakao.com/learn/courses/30/lessons/17684?language=python3)

메세지를 압축하여 보내며, 압축 저의 정보를 완벽 복원 가능한 무손실 압축 알고리즘 구현하기로 했다. LZW(Lempel-Ziv-Welch)압축을 구현하기로 했다.
과정은 다음과 같다.
1. 길이가 1인 모든 단어를 포함하도록 사전 초기화
2. 사전에서 현재입력과 일치하는 가장 긴 문자열 `w`를 찾음
3. `w`에 해당하는 사전의 색인 번호를 출력하고, 입력에서 `w`를 제거한다.
4. 입력에서 처리되지 않은 다음 글자가 남아있다면(`c`),`w+c`에 해당하는 단어를 사전 등록
5. 2단계로 돌아간다.

대문자를 처리한다고 할때 1부터 시작해서 26까지 색인번호를 입력한다. 예를 들어 `KAKAO` 입력시
1. `KAKAO`의 첫글자 `K`가 등록되어 있으므로 11을 출력하고 다음글자 `A`를 포함한 `KA`가 없으므로 `Z`인 26다음 번호인 27에 `KA`를 등록한다.
2. 'A'는 사전에 있고, `AK`는 없으므로 1을 출력하고 28으로 등록한다.
3. `KA`가 사전에 있으므로 27을 출력하고 `O`가 포함된 `KAO`를 29에 등록한다.
4. `O`에 해당하는 15를 출력한다.

이 과정을 통해 [11, 1, 27, 15]로 압축된다.

이러한 방식으로 영어 대문자로만 이뤄진 문자열 msg가 주어진다. msg의 길이는 1글자 이상, 1000글자 이하이다. 주어진 문자열을 압축후의 사전 색인 번호를 배열로 출력하라.

### 문제 풀기
- - -
문자열을 압축해야 하며, 새로운 값에 대한 처리가 추가로 진행되어야 한다. 파이썬을 이용한다면, dictionary를 사용하면 간단하게 풀 수 있을 것으로 보인다. 여기서 가장 중요하게 처리해야 할 부분은 4 단계의 존재하는 처리되지 않은 다음 글자가 나올 때 까지 진행을 해야 하는 부분이다. 그 때문에 for문을 사용할 때 단순히 char값을 받지 말고 문자열의 순번으로 값을 받아와서 처리하는게 편할 것으로 보인다. 그를 통해서 다음 문자열과 합쳐서 dictionary에 해당 단어가 저장되어 있는지를 파악하고, 그 값이 있으면, 다음 문자로 진행할 수 있도록 처리한다.

그리고 있는지 없는지 확인하고 새로운 key를 만들어서 저장시킨 후 다음문자로 진행한다.
- - -
어려운 문제는 아니었지만, 다음 문자에 대한 처리를 어떤 식으로 하는지가 처음 생각했던 것처럼 중요한 부분이었다. for문으로 처음에는 해결하려 했지만, 루프를 도는 중에 그 값에 변화를 주면 루프를 다시 돌 때 +가 되지않을까 싶었지만 되지 않았고, while문을 통해서 변화를 주는 방식으로 바꿔서 처리했다.

- - -
여기서 주요하게 사용했던것들
```python
dictionary = {'key' :'value'}
if 'key' in dictionary:
  pass
else:
  pass

ord("문자") # 아스키값 반환
chr('숫자') # 아스키 값에 해당하는 문자 반환
```

### 코드
- - -
<details markdown="1">
<summary>코드내용</summary>
``` python
dic = {}
alp = ord("A")

for i in range(1,27):
    dic[chr(alp)] = i
    alp += 1

# print(dic)


def solution(msg):
    idx = 27
    answer = []
    j = 0
    # l = len(msg)
    # print(msg[l-1])
    while j < len(msg):
        if j == len(msg)-1:
            answer += [dic[msg[j]]]
            break
        ch = msg[j]
        n_ch = msg[j+1]
        word = ch + n_ch
        while True:
            if word in dic:
                j += 1
                if j == len(msg) - 1:
                    answer += [dic[word]]
                    j += 1
                    break
                n_ch = msg[j+1]
                ch = word
                word += n_ch
            else:
                answer += [dic[ch]]
                dic[word] = idx
                idx += 1
                j += 1
                break
    return answer


print(solution("TOBEORNOTTOBEORTOBEORNOT"))
```
