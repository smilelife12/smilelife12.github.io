---
layout: post
title:  "REST API와 JSON format(3)"
date:   2019-09-16
excerpt: "JSON이란?, Rest API에서 http request 처리"
tag:
- Hyun
- REST API
- JSON
- Python
comments: true
---

## JSON?
- - -
JSON(JavaScript Object Notation)은 속성-값 쌍 또는 "키-값 쌍"으로 이루어진 데이터 오브젝트를 전달하기 위해 인간이 읽 을 수 있는 텍스틑를 사용하는 개방형 표준 포맷이다. 인터넷에서 자료를 주고 받을 때는 그 자료를 표현하는 방법으로 알려져 있다. 자료 종류 제한 없고, 변수 표현하는 데 적합하다. 다양한 프로그래밍 언어에서 쉽게 이용할 수 있다.

기본 자료형으로 수, 문자열, 참/거짓, 배열, 객체, null 이 존재한다.

배열은 대괄호를 사용한다. 객체는 중괄호를 이용하며, 각 쌍들을 쉼표(\,)로 구분한다. 순서는 의미없다.
출처는 [위키백과](https://ko.wikipedia.org/wiki/JSON)

## JSON 파일 읽고 파싱하여 사용하기
- - -
>**우선 파싱이란**
 언어학에서 parsing은 구문 분석이라고도 하고, 문장을 그 것을 이루고 있는 구성 성분으로 분해하고 그들 사이의 위계관게를 분석하여 문장의 구조를 결정하는 것을 말한다.
 컴퓨터 과학에서 파싱은 일련의 문자열을 의미있는 토큰으로 분해하고 이들로 이루어진 파스트리(parse tree)를 만드는 과정을 말한다.
 파서는 인터프리터나 컴파일러의 구성요소 가운데 하나로, 입력 토큰에 내재된 자료구조를 빌드하고 문법을 검사한다.
 출처 [위키백과](https://ko.wikipedia.org/wiki/%EA%B5%AC%EB%AC%B8_%EB%B6%84%EC%84%9D)

 그리고 이제 Python에서 json 포맷을 읽고 사용해 보자.

 처음 참조를 한 사이트는 [w3schools.com](https://www.w3schools.com/python/python_json.asp)을 통해서 시작해 보았다. 우선 json 모듈을 설치해야 하므로 cmd 창에 `conda install -c jmcmurray json` 를 입력하여 설치해 주자.

여기서 json string을 파싱하여 사용하기 위해서 `json.loads()`를 사용한다. 그러면 dictionary 변수 형태로 나타난다.

예시를 보면,
```python
import json

# some JSON:
x =  '{ "name":"John", "age":30, "city":"New York"}'

# parse x:
y = json.loads(x)

# the result is a Python dictionary:
print(y["age"])
```
가 있다. x를 보면 string형태로 JSON포맷을 만들어 나타냈다. 그리고 y에서 `json.loads()`를 이용해파일을 받아왔다.

그리고 나서, 디버깅을 한 후의 모습을 보면
![파일 형식]({{site.url}}/assets/img/posting0916/img1.png)

x는 string 형태이고, y는 dictionary 형태로 저장된것을 볼수 있다.

그럼 이제 파일에 대해서 출력을 해보자. 저번에 했던, `with - as`구문을 사용해 보자.

사용전에 `json.loads()`는 뒤에 붙은 `s` string을 의미하여 string 변수를 가져와 파싱하는 것이고 `json.load()`의 경우는 파이썬 객체를 기반으로 한다. 그러므로 파일 자체를 읽어올 때는 `json.load()`를 이용한다.

```python
import json

with open('ex.json') as data_json:
    y = json.load(data_json)
```
과 같이 json 파일의 내용을 불러와서 y에 dictionary형태로 저장해 줄 수 있다.

이를 반대로 json형태로 변환하는 방법은 'json.dumps()/dump()'를 이용해 주면된다.

여기서는 `sort_keys`와 `indent`를 이용해 더 깔끔하게 만들어 줄 수 있다. `sort_keys`를 통해서는 `sort_keys=True`를 하면 sorting을 해준다. 그리고 `indent`를 이용하면 자동으로 들여쓰기를 해준다.
예를 들어,
```python
import json
print(json.dumps({'4' :5, '6': 7}, sort_keys=True, indent=4))
```
를 실행하면
```
{
    "4": 5,
    "6": 7
}
```
처럼 출력이 된다.
- - -

생각보다 json 파일을 가져오는 것 자체는 어렵지 않다. 이제 이를 이용해서 REST api에 적용하는 것이 문제로 보인다. http method를 이용해서 하는 것이기 때문에 이부분에 대해서 더 공부해야한다.
