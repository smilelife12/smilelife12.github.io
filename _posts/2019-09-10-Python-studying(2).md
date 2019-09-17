---
layout: post
title:  "파이썬을 파이썬 답게(2)"
date:   2019-09-10
excerpt: "리스트 뒤집기, type 변환하기, iteratortools"
tag:
- Hyun
- Python
comments: true
---


## 파이썬을 파이썬 답게...
- - -
[파이썬을 파이썬답게](https://programmers.co.kr/learn/courses/4008)를 이어서 공부해 보자.


### 2차원 리스트 뒤집기
- - -

 이차원 리스트를 인자로 받고 행과 열을 뒤집은 값을 리턴시켜보자
```python
def solution(mylist):
    answer = []
    for i in range(len(mylist)):
        temp = []
        for j in range(len(mylist[0])):
            temp.append(mylist[j][i])
        answer.append(temp)
    return answer
```
간단하게 append를 통해 행의 값과 열의 값을 바꿔서 저장시켜줬다.
- - -
파이썬에서는 zip이라는 함수를 이용하면 쉽게 뒤집을 수 있다.
>#### zip
zip(\*iterables)는 각 요소들을 모으는 이터레이터를 만들어주고, 튜플의 이터레이터를 돌려주는데, i번째 튜플은 각인자로 전달된 시퀀스나 이터러블의 i번째 요소를 포함한다.

```python
mylist = [ [1,2,3], [4,5,6], [7,8,9] ]
new_list = list(map(list, zip(*mylist)))
```
위의 코드를 분석해보면, zip을 통해서 mylist의 첫번째 요소를 모으는데, 이를 map을 통해서 순회를 해서 list형태로 모아준다. 그리고 다음 다음을 진행하면서 합쳐진다. 그렇게 map을 통해서 list형태 합친 요소를 한번 더 list 형태로 합쳐서 2차원 배열형태로 진행하게 되면 각 행의 1열값을 모은게 1번 리스트가 되면서 1행으로 가게되고 그 다음을 진행하는 방식이다.

그리고 이에 대한 예시코드로
```python
list1 = [1, 2, 3, 4]
list2 = [100, 120, 30, 300]
list3 = [392, 2, 33, 1]
answer = []
for i, j, k in zip(list1, list2, list3):
   print( i + j + k )
```
위와 같이 동시 순회할 때 사용가능하다.

또
```python
animals = ['cat', 'dog', 'lion']
sounds = ['meow', 'woof', 'roar']
answer = dict(zip(animals, sounds)) # {'cat': 'meow', 'dog': 'woof', 'lion': 'roar'}
```
위와 같이 dict 생성자를 이용해 딕셔너리를 만들어 줄 수 있다.

### 모든 멤버의 type 변환하기
- - -
문자열 리스트를 입력받아 이를 정수형리스트로 바꿔 리턴해주는 함수를 만들어보자.

map함수를 이용하면 간단하게 할 수 있을 것 같다.
```python
def solution(mylist):
    answer = list(map(int,mylist))
    return answer
```
- - -

처음으로 답과 같은 걸 맞췄다!

for문을 돌리지 않고 map을 통해서 멤버 타입을 변경하는 걸 설명하는 부분이었다.

**map 함수 응용하기**
- - -
map의 첫 번째 인자에 함수형태를 이용해서 구할 수 있다. 각 이차원 리스트내의 리스트 길이를 구하는 방법에 대해 설명이 있었다.
```python
def solution(mylist):
    answer = list(map(len,mylist))
    return answer
```
위와 같이 len을 통해서 사이즈를 구해서 리턴해주면 각 사이즈를 구해 돌려준다.

### sequence 멤버를 하나로 이어붙이기.
- - -
문자열 리스트를 입력받고 이를 하나로 하비는 방법에 대해서 공부해보자.
```python
def solution(mylist):
    answer = ''
    for i in mylist:
        answer += i
    return answer
```
뭔가 `zip`을 통해서 할 수 있을 것 같아서 str형태로 형변환을 하며 사용해 봤지만. 이는 잘못 생각했다. mylist는 리스트 하나를 가지고 하는 것이기 때문에 이렇게 쓰는게 아니므로 우선 풀 수 있는 방법으로 했다.

- - -
여기서는 파이썬의 join을 사용해서 이 코드를 줄인다.
```python
def solution(mylist):
  return ''.join(mylist)
```
와 같이 굉장히 쉽게 `join`을 이용해서 풀 수 있다.

### 삼각형 별찍기
- - -
코딩을 처음 배울 때 for문을 작성하며 배우는 가장 기초적인 문제이다.

```python
n = int(input().strip())
for i in range(1,n+1):
  print('*'*i)
```
- - -
여기서는 print문 안에서 사용한 것이 중요한 부분이다.
파이썬이 아닌 곳에서는 저런식으로 `char`값을 여러개 곱해서 나타내는게 되지 않는다. 하지만 파이썬에서는 `*`을 통해서 여러 번 나타나도록 설정하는게 가능해진다.
리스트에서도 사용이가능하다!
- - -
**곱집합 구하기**
추가적으로 곱집합을 구하는 방법에 대해 알아보면
기존에는
```python
iterable1 = 'ABCD'
iterable2 = 'xy'
iterable3 = '1234'

for i in iterable1:
    for j in iterable2:
        for k in iterable3:
            print(i+j+k)
```
와 같은 방식으로 for문을 해당 문자열 만큼 돌리면서 각각에 대해서 구하는 방식을 사용할 수 있지만,

파이썬에서는 `itertools.product`를 사용하면 for문이 필요하지 않다.
```python
import itertools

iterable1 = 'ABCD'
iterable2 = 'xy'
iterable3 = '1234'
itertools.product(iterable1, iterable2, iterable3)
```
와 같이 간단하게 사용이 가능하다.

### 2차원 리스트를 1차원 리스트로 만들기.
- - -
이차원 리스트를 일차원 리스트로 만들어 리턴해주자.

```python
def solution(mylist):
    answer = mylist[0]
    for i in range(1,len(mylist)):
        answer += mylist[i]
    return answer
```
이런식으로 하면 간단히 만들어 줄수있다.

- - -
하지만, 파이썬에서는 다양한 기능을 사용하면 더 쉽게 가능하다. 굉장히 많아서 놀랍다.
```python
# 방법 1 - sum 함수
answer = sum(my_list, [])

# 방법 2 - itertools.chain
import itertools
list(itertools.chain.from_iterable(my_list))

# 방법 3 - itertools와 unpacking
import itertools
list(itertools.chain(*my_list))

# 방법4 - list comprehension 이용
[element for array in my_list for element in array]

# 방법 5 - reduce 함수 이용1
from functools import reduce
list(reduce(lambda x, y: x+y, my_list))

# 방법 6 - reduce 함수 이용2
from functools import reduce
import operator
list(reduce(operator.add, my_list))
```
기본적으로 sum을 이용해서 가장 간단하게 할 수 있다. 각 리스트들을 합쳐주는 것이다.
