---
layout: post
title:  "JS하다보니 jQuery"
date:   2019-10-23
excerpt: "jQuery 공부"
tag:
- Hyun
- JavaScript
- jQuery
comments: true
---


## jQuery 적용해보기
- - -
jQuery를 만들어본 코드에 적용을 해보았다.
```JavaScript
$(document).ready(function () {
  $.fn.myFunction = function () {
    alert($('#search').val());
  }
  $("#btn").click(function f() {
    $.fn.myFunction();
  });
});

```


```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <link rel="stylesheet" href="search.css">
  <script
  src="https://code.jquery.com/jquery-3.4.1.min.js"
  integrity="sha256-CSXorXvZcTkaix6Yvo6HppcZGetbYMGWSFlBw8HfCJo="
  crossorigin="anonymous"></script>
  <script src="search.js"></script>
  <title>Title</title>
</head>
<body>
  <div>
    <label>
      <input type="text" id="search" placeholder="겁색어입력">
    </label>
    <button id="btn">검색</button>
  </div>
</body>
</html>
```
위의 두가지를 통해서 만들어 줄 수 있었는데, 이를 통해서 알게된 점은 jQuery를 이용하면 확실하게 element에 접근하기 쉬워진다는 점이다. 간단하게 `$('#id')`혹은 `$('tag')` 등으로 간단히 접근하고 그 내부의 함수 등을 파악해 사용하면 축약할 수 있다는 장점이 있다.  
그런데 아직 모르겠는건 함수를 어떻게 써야할 지 모르겠다. 좀 더 사용해 봐야 알 수 있을 것 같다.
