---
layout: post
title:  "Python의 GUI, PyQt5 공부(2)"
date:   2019-06-13
excerpt: "Python, PyQt5 공부하기"
tag:
- Hyun
- Game
- Python
- PyQt5
comments: false
---

## PyQt5 공부
- - -

일단 계속 공부를 하다보니 내가 필요한 요소를 공부하는 느낌이 아니었다. 내가 하려는 테트리스는 이런 식으로 플레이가 되는 게 아니었고, 나는 블럭을 만드는 것을 알아야 했다. 그래서 블럭 만드는 법을 찾아봤다. 우선적으로 찾아본 방법은 layer를 색칠하는 방식인데 이를 그대로 하는건 좀 어렵다고 판단하고 다른 방법을 찾았다.

그래서 찾은 방법은 QRect()을 이용하는 방법이다. 여기서 이용하는 함수는
```python
paintEvent(self,event)
```
 라는 내장함수를 이용한다. 여기서 구현 방식을 아직 자세히 이해하지는 못했지만
```python
QPainter()
QRect(x,y,w,h)
```
라는 메서드를 이용해 내가 그리고자 하는 부분을 선택해준다. 그리고 QPainter의 내장함수들을 이용해서 원하는 도형을 그리는 방식으로 진행한다. 블럭을 QRect라는 직사각형 하나로 대체를 해주고 이를 저장할 수는 없기에 원하는 위치와 크기를 저장한 list를 만들어준다. 그래서 그 리스트에서 위치와 크기값을 가져와서 블럭을 그리는 방식으로 진행했다.
<details markdown="1">
<summary>예시코드</summary>
```python
def paintEvent(self, e):
     painter = QPainter(self)
     rec = [[self.x1, self.y1, 10, 10],
            [self.x1 + 10, self.y1, 10, 10],
            [self.x1, self.y1 + 10, 10, 10],
            [self.x1 + 10, self.y1 + 10, 10, 10]]
     for r in rec:
      painter.drawRect(r[0],r[1],r[2],r[3])
```

이제 여기서 event중
```python
KeyPressEvent(self,event)
```
를 이용해 입력한 키를 통해서 처리하는 방식을 정해야한다. 이때 방향키를 통해서 블럭들을 이동시킬 건데, 이를 이용할 때, 테트리스를 생각해보면, 지금 내가 현재 움직이는 블럭들이 있고, 이미 위치를 시킨 블럭들도 있을 것이다. 그러면 내가 해야할 일은
* 게임 내에서 이미 도달한 블럭들을 저장해 놓고, 계속 다시 그려줄 방법
* 새로운 블록이 나왔고, 이를 움직일 때, 위치 값에 대한 저장을 어떻게하고, 어떻게 움직일 것인가.
* 위치한 블럭들에 도달했을 때 어떻게 멈추게 할 것인가.
에 대한 고민을 해야할 것 같다.
