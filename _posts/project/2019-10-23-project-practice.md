---
layout: post
title:  "프로젝트 끄적이기"
date:   2019-10-23
excerpt: "검색창 만들어보기"
tag:
- Hyun
- CSS
- HTML
comments: true
---

## 그냥 시작하기
- - -
검색창을 만들어보자.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
  <div>
    <input type="text" placeholder="검색어 입력">
    <button id="btn" onclick="test()">검색</button>
    <script>
      function test() {
          alert("hello, you click button");
      }
    </script>
  </div>
</body>
</html>
```
html을 통해서 검색어 입력창을 만들고, 그 옆에 `검색`이라는 버튼창을 만들어준다. 단순 검색창 만들기랑 클릭에 대해 값이 표현되도록 해보았다.  
>>**id**
id같은 경우는 html 내의 단 하나 사용하는 값으로 해당 값을 입력받고 html 전체에서 내용을 사용할 수 있다.  


![첫 걸음]({{src.url}}/assest/img/posting1023/img1.png)  


- - -
이제 약간 꾸며볼 예정인데 CSS같은경우는 돌아다니는 파일로 만들어주자.  
[https://hongku.tistory.com/300](https://hongku.tistory.com/300)에서 버튼이 깔끔해보여서 이를 이용해 보았다.
```CSS
div{
  height: 40px;
  width: 400px;
  border: 1px solid skyblue;
  background: #ffffff;
}

input{
  font-size: 16px;
  width: 325px;
  padding: 10px;
  border: 0px;
  outline: none;
  float: left;
}

button#btn{
  width: 50px;
  height: 100%;
  border: 0px;
  border-top-right-radius: 5px;
  border-bottom-right-radius: 5px;
  margin-left:+5px;
  background-color: rgba(0,0,0,0);
  color: skyblue;
  padding: 5px;
}
button:hover#btn{
  color:white;
  background-color: skyblue;
}
```
- `div` : `검색창` 부분의 크기를 조절하고 테두리색도 조정해 주었다.  
- `input` : 검색하는 내용 부분에 대해서 설정을 해주는 부분으로 크기와 `padding`을 통한 테두리와의 위치 조절, `float`을 통한 왼쪽 정렬도 해준다.  
- `button#btn` : 이 부분은 `button`태그의 `id`가 `btn`인 부분을 조절한다. 크기조절과, `border-top-right-radius`, `border-bottom-right-radius`를 통해서 오른쪽 위와 왼쪽 아래에 각진 부분을 둥글게 만들어 준다.  
- `button:hover#btn` : 버튼의 `hover`는 손을 가져다 댔을 때 변하게 되는 스타일이다.  

![두걸음]({{src.url}}/assest/img/posting1023/img2.png)

- - -
검색어입력에 사용한 값을 활용하기 위해서 어떻게 해야할까. `alert()` 에 값을 전달해 줘보자.

```html
<input type="text" id="search" placeholder="겁색어입력">

<script>
      function test() {
          alert(document.getElementById("search").value);
      }
    </script>
```
각각 `input`위치에 대한 수정과 `test` 함수에 대한 수정을 했다. `id`를 `search`로 설정해준 뒤에 `script`를 이용해서 작성한 `test()`함수 속 `alert()`에 입력한 값이 전달되도록 한다.  
여기서 `document.getElementById()`가 사용이 된다.
>**(document.getByElementByID(id))[https://developer.mozilla.org/ko/docs/Web/API/Document/getElementById]**
찾으려는 id값을 매개변수로 전달하며, 대소문자 구분이된다. 그리고 id와 일치하는 DOM요소를 나타내는 `Element`를 리턴하고 없으면 `null`을 리턴한다.   

지금같은 경우는 `input` 값에 대한 내용의 `Element`가 리턴되었으므로 여기의 `value`값에 작성한 내용이 저장이된다. 이를 이용해서 입력값을 출력시킨다.  

![세걸음]({{src.url}}/assest/img/posting1023/img3.png)
