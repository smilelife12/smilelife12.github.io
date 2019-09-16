---
layout: post
title:  "파이썬을 파이썬 답게-3"
date:   2019-09-12
excerpt: "Python like python"
tag:
- Hyun
- Python
comments: true
---


## 파이썬을 파이썬 답게...
- - -
[파이썬을 파이썬답게](https://programmers.co.kr/learn/courses/4008)를 이어서 공부해 보자.


### 순열과 조합
- - -
 숫자를 담은 일차원 리스트를 이용해서 순열을 만들고 사전순으로 리턴해주는 solution을 완성하자.
 >>순열은 원소 순서가 중요하며, 순서가 다르면 다른 경우이다.
 조합은 원소들의 값이 같으면 같은 경우로 처리한다.

 솔직히 말하자면, 어떤식으로 구성할지 시간이 너무 오래걸릴 것 같았다. 재귀함수도 사용하고 sort도 해야할 것 같아서 코드 짜는 것을 패스 했고, 강의 노트를 보게 되었다.
 - - -
 우선 라이브러리를 사용하지 않고 접근한 방법을 보았다.
 ```python
 def permute(arr):
    result = [arr[:]]
    c = [0] * len(arr)
    i = 0
    while i < len(arr):
        if c[i] < i:
            if i % 2 == 0:
                arr[0], arr[i] = arr[i], arr[0]
            else:
                arr[c[i]], arr[i] = arr[i], arr[c[i]]
            result.append(arr[:])
            c[i] += 1
            i = 0
        else:
            c[i] = 0
            i += 1
    return result
```
[순열 관련 정리](https://gorakgarak.tistory.com/522?category=265216)를 참조하면 좀더 쉽게 이해할 수 있을 것 같다.
첫 번째 값과 순서를 바꿔주면서 순열을 만들어내는 사이클을 만드는 것이다.

- - -
라이브러리를 사용하여 간단하게 푼 경우이다.
 ```python
 import itertools

pool = ['A', 'B', 'C']
print(list(map(''.join, itertools.permutations(pool)))) # 3개의 원소로 수열 만들기
print(list(map(''.join, itertools.permutations(pool, 2)))) # 2개의 원소로 수열 만들기
```
와 같이 ``itertools`` 라이브러리에 해당 함수가 존재하며, 조합의 경우는 ``combinations()``를 통해서 구현할 수 있다.

### 가장 많이 등장하는 알파벳 찾기.
- - -
문자열이 주어졌을 때, 가장 많이 등장하는 알파벳을 찾아주는 프로그램을 만들자. 수가 같은 경우 사전순으로 정렬한다.

```python
my_str = input().strip()
set_str = set(my_str)

max = -1
answer = []
for x in set_str:
    t_cnt = my_str.count(x)
    if max < t_cnt:
        max = t_cnt
        answer = [x]
    elif max == t_cnt:
        answer.append(x)
print(''.join(sorted(answer)))
```
딱 봐도 아닌거 같긴 하지만, 우선 문제를 풀어야 해서 풀었다. 후보군을 정할 때 중복은 처리를 안하기 위해서 `set`자료형을 이용해 중복되는 단어들이 제거되도록 해주고, max값을 이용해서 해당 알파벳의 숫자가 몇개나 나타나는 지 확인하고 같으면 추가 더 크면 이 알파벳만 리스트에 존재하도록 설정하는 방식을 이용하며, 마지막에 answer를 join을 이용해서 문자열 형태로 출력해주었다.

- - -

해당 풀이는 ``collections.Counter``클래스를 사용해서 리스트에 있는 원소가 몇개씩 가지고 있는지를 나타내는 dictionary 변수를 만들어준다.
```python
import collections
my_list = [1, 2, 3, 4, 5, 6, 7, 8, 7, 9, 1, 2, 3, 3, 5, 2, 6, 8, 9, 0, 1, 1, 4, 7, 0]
answer = collections.Counter(my_list)

print(answer[1]) # = 4
print(answer[3])  # = 3
print(answer[100]) # = 0
```
이런식으로 사용할 수 있으며, 이는 문자열의 경우에도 그대로 my_list에 문자열을 넣어서 사용할 수 있다.
그리고 만들어진 dictionary 형에다가 `update`를 이용해서 추가적으로 개수를 셀 수 있다.
```python
answer.update([1,1,2,3,100])

print(answer[1]) # = 6
print(answer[3]) # = 4
print(answer[100]) # = 1
```
과 같은 방식으로 추가적으로 딕셔너리 값이 업데이트 된다.

그리고 key값의 경우는 `elements()`를 통해서 받아올 수 있다. 그리고 위에서 `most_common(n)`을 통해서 빈도수가 높은 순으로 상위 n 개를 나타낸다. 그리고 ``subtract()``를 이용해서 원하는 문자를 빼줄수있다.

그리고 추가적으로 산술/집합연산이 가능하다.
덧셈을 통해서 각각의 값을 합쳐줄 수 있고, 뺄셈을 통해 숫자를 뺄 수 있으며(음수는 출력하지 않음), 교집합과 합집합도 사용이가능하다.
- - -

### for문과 if문을 한번에
- - -

정수를 받은 값을 입력받아 리스트의 원소중 짝수인 값만을 제곱으로 담은 리스트를 리턴하는 solution함수를 만들자.
```python
def solution(mylist):
    answer = []
    for i in mylist:
        if i%2 == 0:
            answer.append(i**2)
    return answer
```

단순하게 풀어냈다.

- - -
파이썬에서는 list comprehension을 이용하면 한 줄 안에 for문과 if문을 처리할 수 있다.
```python
def solution(mylist):
  answer = [ i**2 for i in mylist if i%2 == 0]
  return answer
```
### 두 변수의 값 바꾸기 - swap
- - -
```python
a = 3
b = 'abc'
a, b = b, a
```
간 단

### 이진 탐색하기
- - -
```python
import bisect
mylist = [1, 2, 3, 7, 9, 11, 33]
print(bisect.bisect(mylist, 3))
```
이진 탐색 모듈인 `bisect`를 이용해서 사용 가능하다.

### 클래스 인스턴스 출력하기 - class의 자동 string casting
- - -
파이썬에서는 `__str__` 메소드를 이용해서 class 내부에서 출력 format을 설정할 수 있다.

```python
class Coord(object):
    def __init__ (self, x, y):
        self.x, self.y = x, y
    def __str__ (self):
        return '({}, {})'.format(self.x, self.y)

point = Coord(1, 2)
```

### 가장 큰 수
- - -
코딩 테스트 문제를 풀 때, 최솟값을 저장하는 변수에 큰값을 저장하는데 이를 사용할 때 `inf`라는 값을 사용하면된다.

```python
min_val = float('inf')
max_val = float('-inf')
```
위와 같이 최대값의 경우는 -를 붙여서 지정해주면 좋다.

### 파일 입출력 간단하게 하기
- - -
파일 입출력 코드를 간결하게 짜는 법인데, 보통은 EOF를 만날 때 까지, 파일 읽기를 반복한다.
```python
f = open('myfile.txt', 'r')
while True:
    line = f.readline()
    if not line: break
    raw = line.split()
    print(raw)
f.close()
```

하지만 파이썬 에서는 `with - as`라는 구문을 이용해 코드를 간결하게 짤 수 있는데 `with - as` 블록이 종료되면 파일이 자동으로 close 된다.

```python
with open('myfile.txt') as file:
  for line in file.readlines():
    print(line.strip().split('\t'))
```
위와 같이 사용하며 socket혹은 http에서도 사용이 가능하다.


- - -

여기까지가 설명의 끝이고, 이를 이용해서 추후 코딩을 할 때마다 들여다보며 참고해야겠다!
