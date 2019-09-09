---
layout: post
title:  "파이썬을 파이썬 답게-1"
date:   2019-09-04
excerpt: "Python like python"
tag:
- Hyun
- Python
comments: false
---

## 파이썬을 파이썬 답게...
- - -
파이썬을 사용하면서 단순히 자바나 c에서 사용하는 그대로를 가져다가 코드를 짜는게 다반사였다.
그러던중 프로그래머스 사이트에서 [파이썬을 파이썬답게](https://programmers.co.kr/learn/courses/4008)라는 강의를 보게 되었다. 여기서 별거 아니겠지 하고 첫번째 문제를 풀어보았다.

- - -
이차원 리스트의 길이를 출력하는 문제였고 나는 쉽게
```python
def solution(mylist):
    answer = []
    for li in mylist:
        answer += [len(li)]
    return answer
```
이렇게 풀었고, 당연히 맞췄고, 당연하게 넘어갔다. 하지만, 다음 강의에서 `이 코드는 파이썬이 아닙니다. c나 자바에 가깝죠`라는 말을 듣고 바로 수강을 해야겠다는 생각이 들었다. 그래서 공부를 시작했다.

## 파트2. 정수 다루기
- - -

**몫과 나머지**
- - -
다시 한 번, 테스트 코드가 나왔다. 몫과 나머지를 출력하세요 라는 문제였다.
```python
a, b = map(int, input().strip().split(' '))
print(int(a/b),a%b)
```
라고 구했고, 여기서 나는 또 부족함을 느끼게 된다.
```python
int(a/b)
```
부터 잘못되었다. 이를 이용할 필요도 없이
```python
a//b
```
를 통해서 정수만 출력하고 버림을 시행한다.
그리고 파이썬에서는
```python
*divmod(a,b)
```
를 이용해서 몫과 나머지를 한 번에 출력할 수 있다. 물론 이방법이 항상 좋은 것은 아니고, 스타일에 따라 다르며 작은 숫자의 경우는 전자가 빠르고, 큰 숫자는 후자가 빠른 모습을 보인다.

**n진법으로 표기된 string을 10진법으로 변환**
- - -
n진법으로 나타난 숫자를 10진법으로 출력하는 방법이다.
나의 코드는
```python
num, base = map(int, input().strip().split(' '))
s = 0
i = 0
while num!= 0:
    s += (num%10*(base**i))
    i+=1
    num = num//10
print(s)
```
라고 구했다.

그런데 아예 while문은 필요가 없었다.
```python
num, base = map(int, input().strip().split(' '))
answer = int(num,base)
```
위와 같이 진수 변환을 파이썬에서는 지원을 한다. 오늘 푼 진수변환문제도 그냥 이렇게 풀었으면 끝... 정말 간단한 부분이었다.

## 파트3. Str 다루기
- - -

**문자열 정렬하기**
- - -
문자열을 길이 n인 문자열에 왼쪽, 가운데, 오른쪽 정렬하여 출력하는 방법이다.
나의 코드는
```python
s, n = input().strip().split(' ')
n = int(n)
arr = ""
arr += s +"\n"
for i in range((n-len(s))//2):
    arr += " "
arr += s+"\n"
for i in range(n-len(s)):
    arr += " "
arr += s
print(arr)
```
각 공백을 추가하여 정리하는 방법을 사용했다.

하지만 파이썬엔 내장함수가 있었다.
```python
s.ljust(n) # 왼쪽 정렬
s.center(n) # 가운데 정렬
s.rjust(n) # 오른쪽 정렬
```
단 세줄로 모든 정렬을 다 해냈다.


**알파벳출력하기**
- - -
입력이 0 이면 소문자 출력, 1이면 대문자 출력하는 코드를 짜야한다.
나의 코드는
```python
num = int(input().strip())
str = 'abcdefghijklmnopqrstuvwxyz'
if num == 1:
    str = str.upper()
print(str)
```
나름 `upper()`함수를 쓰면서 잘썻다고 생각했다. 하지만, 파이썬은 더 간단했다.
이 데이터들을 상수로 저장해 놓았다.
```python
import string

string.ascii_lowercase #소문자 알파벳
string.ascii_uppercase #대문자 알파벳
string.ascii_letters   #소문자 대문자 순서로 출력
string.digits #숫자 0~9
```
이 상수들은 [pytohon documents](https://docs.python.org/3.4/library/string.html) 에서 볼 수 있다.
