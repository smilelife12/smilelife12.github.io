---
layout: post
title:  "MarkDown 정리하기 1"
date:   2019-06-03
excerpt: "글쓰면서 찾은 Markdown사용법"
tag:
- Hyun
- Post
- Markdown
comments: false
---

## 코드 인용하기
- - -

 코드를 인용하는 방법은
 ````
 ```언어명
 코드내용
 ```
 ````
를 작성하면 된다. 자기가 작성한 언어 명을 작성하고 개행 후에 그대로 코드를 작성하면 자동으로 코드를 인식해서 작성해준다.  
예를 들어
````
```java
import java.util.*;

class Hello{
  public static void main(String args){
    System.out.println("Hello World!\n");
  }
}
```
````
라고 작성하면

```java
import java.util.*;

class Hello{
  public static void main(String args){
    System.out.println("Hello World!\n");
  }
}
```

## \`와 같은 예약문자 사용
- - -

`` ` `` 같은 문자는 이미 지정된 처리 방식이 있기 때문에 \\(백 슬래쉬)를 앞에다가 붙여서 사용하면 된다.
그리고 이 애먹은 내용인 인용의 경우 `` ` `` 를 하나 더붙여주면 된다. 인라인의 경우는 이렇게 쓰면되고 코드인용의 경우는
`````
````
```
내용작성
```
````
`````
이런 식으로 써 주면 예약문자의 내용이 처리되지 않고 글로 작성된다.


## 사진 삽입
- - -

`` ![파일 설명]({{site.url}}/파일위치/sample.PNG)``
라고 지정해주면 절대 경로로 이미지를 지정해 줄 수 있다. 물론 미리 사진파일을 저장해 두어야 위에 방식처럼 사용이 가능하다.
