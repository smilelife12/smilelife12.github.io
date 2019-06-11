---
layout: post
title:  "웹개발, 첫단계 HTML 복습"
date:   2019-06-11
excerpt: "HTML 복습하기"
tag:
- Hyun
- Web
- HTML
comments: false
---

## 웹개발 시작하기
- - -
 과거 소프트웨어 프로그래밍을 하며 웹개발의 맛만 본적이 있다. 그 때 정말 간단 간단하게 보고 빠르게 보느라 놓친 부분이 많았다. 그래도 한번 봤던 거기 때문에 기본적인 내용은 정리를 하는 걸로 시작을 하는 걸로 했다.


 공부는 Opentutorials, 생활코딩에 정말 잘되어 있어서 참고하고 공부하기 편했다. HTML 같은 경우는 [Web1 - HTML & Internet](https://opentutorials.org/course/3084)을 통해 공부를 했다.

## HTML 복습하기
- - -

### 기본문법 - 태그
- - -
 HTML은 태그라는 문법이 존재하며, <'태그'> 안에 들어가는 태그에 따라 각 내용을 표현하는 방식이 달라지며, <\/'태그'>를 통해서 닫는 태그도 존재한다. 그리고 태그는 중첩이 가능하다.

 예시)
```html
<strong>creating <u>web</u> pages</strong>
```
\<strong>을 통해서 굵은 글씨를 표현 했고, \<u>를 통해서 밑줄을 그어 줄 수 있다. 그리고 각각의 태그가 닫히는 곳을 지정해 주어서 태그가 적용되는 범위를 지정해 준 것이다.

그래서 이를 실제로 적용하면,

<strong>creating <u>web</u> pages</strong>

라고 표현된다.(markdown도 html언어라 표현이 쉽다.)

태그는 약 150개가 넘게 존재하지만, 필요한 태그는 그렇게 많지 않다. [태그 사용 빈도](https://www.advancedwebranking.com/html/)를 통해 많이 사용되는 태그 위주로 공부를 한다.

### 줄바꿈 태그
- - -
 줄바꾸는 태그 같은 경우 \<br> 태그와 \<p>태그가 존재한다.

#### br 태그
br 태그의 경우는 열린 태그이다. 줄을 바꾸고 싶을 때 \<br>만 작성해 주면, 그 부분에서 줄이 바뀐다.

#### p 태그
p 태그의 경우, paragraph의 줄임말로 단락을 나눠 줄 때 사용한다. 그래서 단순 줄바꿈이 아닌 단락을 지정해서 나누는 것을 이용하고 싶은 경우에는 p태그를 더 적극적으로 활용할 필요가 있다.

하지만, p태그의 경우는 단순히 단락만 나눠 주기 때문에 시각적으로 불편해 보일 수 있다. 이는, CSS를 이용해서 해결해준다.

### 이미지 넣기
- - -
```html
<img src = "파일주소" width = "100%">  
```
이런 식으로 작성하면 이미지 파일을 넣어 줄 수 있다. 뒤의 width를 통해서 이미지 파일의 크기를 설정해 줄 수 있다.

### 태그의 부모 자식 관계
- - -
 위에서 언급했던 것 처럼, 태그 안에 태그가 포함 될 수 있다. 이를 부모자식 관계로 표현하며, 이 관계가 바뀔 수도 있지만, 부모 자식의 순서가 고정된 태그가 존재한다.

#### 목차 태그
- - -
 그 예시중 하나가 목차 태그이다. \<li>태그의 경우 목차를 지정해 줄 수 있다. 예를 들어
```html
<li>1. HTML</li>
<li>2. CSS</li>
<li>3. JavaScript</li>
```
 <li>1. HTML</li>
 <li>2. CSS</li>
 <li>3. JavaScript</li>
이 처럼 나타난다. 또 이 목차들을 구분해 주기 위해 사용해야 하는 태그가 \<ul> 태그이다.
```html
<ul>
  <li>1. HTML</li>
  <li>2. CSS</li>
  <li>3. JavaScript</li>
</ul>
<ul>
  <li>egoing</li>
  <li>k8805</li>
  <li>sorialgi</li>
</ul>
```
<ul>
  <li>1. HTML</li>
  <li>2. CSS</li>
  <li>3. JavaScript</li>
</ul>
<ul>
  <li>egoing</li>
  <li>k8805</li>
  <li>sorialgi</li>
</ul>
이런식으로 구분이 된다. 이는 ul이 아닌 ol 태그로도 사용 가능하며 각각 ul은 unordered list, ol은 ordered list의 약자들이다. 그래서 ol을 사용하면 각 목차들에 번호들이 적용된다.

### 문서구조 형성
- - -
#### title 태그
- - -
\<title> 태그를 이용해 웹페이지의 제목을 설정한다.

#### meta 태그
- - -
\<meta charset="utf-8">을 이용해 UTF-8을 이용했음을 명시한다.
#### head 태그와 body 태그, 그리고 doctype
- - -
지금 사용한 두개의 태그들은 본문 내용을 작성한 것이 아닌, 본문 자체를 설명하는 태그들이다. 그래서 이를 구분해주는 태그를 head와 body를 이용해서 구분해준다.
예시)
```html
<!doctype html>
<head>
<title>WEB1 - html</title>
<meta charset="utf-8">
</head>
<body>
<ol>
  <li>HTML</li>
  <li>CSS</li>
  <li>JavaScript</li>
</ol>
</body>
```


 본문을 설명하는 부분을 \<head> 태그로 감싸 주고, 본문 자체는 \<body> 태그로 감싸준다.
여기에 마지막으로 html문서로 만들어주었다는 것을 표현하기위한 코드로 \<!doctype html>을 작성해주면 기본적인 틀이 완성된다.

### a 태그
- - -
a태그는 anchor의 준말로, 가장 많이 사용되는 태그로 가장 중요한 태그이다. 이를 이용하여 링크를 걸 수 있다.
```html
<a href="링크주소" target="_blank" title = "tool tip"> 설명</a>
```
위처럼 사용할 수 있으며 target="\_blank"의 경우 새창에서 나타나도록 하며, title은 툴팁을 통해 설명을 보여준다.

### Internet과 Web
- - -
인터넷과 웹의 구분은, 인터넷이 도시라면, 웹은 그 위에 있는 건물 한채이다. 인터넷에 포함되어 있고, 그 안에 만들어진 기술이다.

웹은 스위스 입자물리연구소(CERN)에서 최초로 개발되고, 사용되었다. [http://info.cern.ch](http://info.cern.ch)가 바로 최초의 웹사이트이다.


### 서버와 클라이언트
- - -
정보를 공유하기 위해 필요한 부분이다. 클라이언트에서 서버에 request를 하면 서버에서 response하여 정보를 전달한다.
![server&client]({{site.url}}/assets/img/posting0611/server.jpeg)
 위와 같이 클라이언트가 주소를 입력하면 서버에서 주소에 해당하는 내용을 찾아 전달해주는 것이 서버와 클라이언트가 주고 받는 내용이다.

### apache이용하기
- - -
웹서버가 있어야 만든 웹페이지를 사용할 수 있다. 이는 apache를 이용해 만들 수 있으며, bitnami라는 프로그램으로 쉽게 사용할 수 있다. 이를 이용하면 localhost를 이용해 html로 만든 브라우저를 공유할 수 있다. 이는 ip를 이용해 통신할 수 있으며, 같은 와이파이를 이용하며 접근 가능하다.

- - -
html에 관해 공부했던 기초내용들을 짧게 정리해 봤고, 앞으로 css와 javascript를 추가적으로 공부해 나갈 예정이다.
