---
layout: post
title:  "프로젝트 끄적이다보니 JS"
date:   2019-10-23
excerpt: "JS 공부"
tag:
- Hyun
- JavaScript
comments: true
---

## JavaScript
- - -
하려다 보니 script에 대한 학습이 필요했다.  
언제나 이용하는 생활코딩의 (https://opentutorials.org/course/3085)[https://opentutorials.org/course/3085]에서 자바스크립트 중 필요성을 느끼는 부분들을 골라서 공부하자.

## 제어할 태그 선택하기
- - -
해당 태그에서 해당 태그의 내용 중 변경하는 것을 자바스크립트언어를 이용해 편집하는 방법을 알아보면,
```html
<input type ="button" value="night" onclick="document.querySelector('body').style.backgroundColor = 'black';">
```
위와 같이 버튼을 클릭하고 그에 대한 대응으로 자바스크립트를 넣어줄 수 있다. 이를 `;`를 통해서 구분하고, 원하는 스타일에 대한 변경을 할 수 있다.  


## 태그에 대한 선택과 편집
- - -
이를 통해서 알게 된 것은 `document`를 통해서 뒤에 어떤 방식으로든 원하는 값을 `Element`를 선택하고 해당 `Element`에서 변경을 하려는 값에 대해서 편집을 하는 과정을 거치는 것이다. 즉, 직접 해당 값에서 편집하는 것이 아닌 접근에 대한 권한을 얻어서 편집하는 거라고 생각된다.  

그리고 JavaScript에서는 변수에다가 해당 `Element`를 저장할 수 있으므로 이를 통해서 편하게 접근하는 방식을 택할 수 있다.

## Script를 통한 함수만들기
- - -
해당 스크립트들을 태그 안에 쓰는 것이 아닌 따로 편집하기 위해서 만드는 것이 `script`태그이고 이 안에 JavaScript에 대한 함수 혹은 변수에 대해서 작성하면 된다.


## 객체
- - -
변수를 저장하는 것을 Python의 딕셔너리 형태로 저장할 수도 있다. 하지만 이를 접근하는 방법은 동일하게 사용할 수도 있고, 키값을 객체에 속하는 값을 사용하듯 사용하면 된다. 그리고 이를 프로퍼티라 한다.
```JavaScript
var coworkers = {
  "programmer": "egoing",
  "desinger":"leezche"
};
document.write("programmer: "+coworkers.programmer+"<br>");
document.write("designer: "+coworkers.designer+"<br>");
coworkers["data scientist"]="taeho";
```

반복문은 `for each`문을 통해서 사용할 수 있다.

## 객체안에 메소드 만들기
- - -
```JavaScript
var Body = {
  setColor:function (color){
    var a;
    alert(a);
  }
}
```
이런식으로 사용하며, 함수를 설정할 때 함수 앞에 해당 이름을 작성해야하는 것이 중요하다. 그리고 해당 객체 작성시 내부 내용 구분은 `,`를 통해서 구분해준다.  


## 자바스크립트 구분하기
- - -
해당 `script`에 작성한 내용을 `js`파일로 따로 저장을 한 뒤 `head`에
```html
<script src="colors.js"></script>
```
와 같이 작성하면 해당 `js`파일에 작성한 내용을 그대로 사용할 수 있다.  

이는 만들었던 것에도 쉽게 적용할 수 있으므로 그대로 적용시킨다.


## 라이브러리와 프레임워크
- - -
라이브러리중 가장 유명한건 `jQuery`라이브러리아고, 이를 통해서 좀 더 생산적으로 사용할 수 있다.  
보통 `CDN`을 통해서 사용할 수 있다.  
예를 들어 `CSS`를 변경시켜줄 때 jQuery를 이용하면
```JavaScript
var Links = {
  setColor:function(color){
    $('a').css('color',color);
  }
}
```
와 같이 짧게 사용할 수 있다. 즉, 해당 태그의 내용의 css에 대한 값의 `color`값을 매개변수 `color`를 통해 작동한다 라는 것을 굉장히 간결하게 사용할 수 있다.


## 필요한 것들
- - -
- **document** 태그를 삭제, 자식 태그 설정
- **DOM** 위의 값으로 안될 경우 사용하여 범위 넓힌다
- **window** 웹브라우저 자체를 제어할 때 필요
- **ajax** 웹페이지를 리로드하지 않고 웹페이지 정보변경
- **cookie** 웹페이지가 리로드 되어도 현재 상태 유지
- **offline web application** 오프라인 상태로 작동하는 어어플리케이션
- **webRTC** 화상통신
- **speech** 음성을 사용
- **webGL** 그래픽
- **webVR** 가상현실 사용
