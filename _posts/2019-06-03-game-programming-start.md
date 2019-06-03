---
layout: post
title:  "Game 개발 한걸음(With PyQt5)"
date:   2019-06-03
excerpt: "Python을 이용해서 게임 개발 시작"
tag:
- Hyun
- Game
- Python
- PyQt5
comments: false
---

## 간단한 게임 개발 시작
- - -
 원래는 java swing을 이용해서 개발을 하려고 했으나, 그보다는 Python에 있는 PyQt가 좀 더 나을거 같다는 판단을 해서 PyQt5 기반으로 개발하려 한다.  


## 설치
- - -
 Python은 기존에 사용하던 PyCharm을 이용해서 편집할 예정이고, 포맷하면서 Anaconda가 삭제되어서 재설치를 했다.
Anaconda에는 PyQt5가 기본 설치가 되어있어서 추가설치는 할 필요가 없었다.


## PyQt5 시작
- - -
 우선은 PyCharm으로 프로젝트를 만들고 [PyQt5 Tutorial](https://wikidocs.net/21920) 참조해서 공부를 시작했다.
첫 번째로 항상 만나는 'Hello, World!'류 코딩을 했다.

``` python
import sys
from PyQt5.QtWidgets import *

app = QApplication(sys.argv)
label = QLabel("Hello PyQt")
label.show()
app.exec_()
```

![Hello,PyQt]({{site.url}}/assets/img/posting0603/HelloPyqt.PNG)
*실행화면*    

기본 구조는 exec_() 메서드를 통해 프로그램이 이벤트 루프로 진입하게 되며, 이벤트 루프는 기본적으로 무한 루프 상태로 들어가는 것이고, 그 상태에서 어떤 이벤트에 대해서 그때그때 처리하는 것을 통해 Application이 작동한다.

## Qt designer
- - -
![Qt Desinger]({{site.url}}/assets/img/posting0603/qtdesinger.png)  
*Qt Desinger 화면*   

이를 편하게 편집하기 위해서 있는 프로그램 Qt Desinger라는 프로그램이 있다.
드래그앤 드롭으로 쉽게 레이아웃 구성가능하다.

Qt Designer 는 ui확장자 파일로 만들어지고 이를 py파일로 바꿔주기 위한 명령어는
```
pyuic5 - x filename.ui -o fimename.py
```
를 통해서 바꿔 줄 수 있다.

이를 통해 Qtdesinger로 기본틀을 만들어주고 그를 이용해 코드 바꿔주는 것을 진행할 수 있다.

## PyQt5 기본 틀 만들어보기

기본 어플리케이션을 만들어 보기 위해 샘플 코드를 가져왔다.

```python
import sys
from PyQt5.QtWidgets import *

class MyApp(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()

    def initUI(self):
        self.setWindowTitle('My app')
        self.move(300, 300)
        self.resize(400,200)
        self.show()

if __name__ == '__main__':

    app = QApplication(sys.argv)
    ex = MyApp()
    sys.exit(app.exec_())
```

![My app]({{site.url}}/assets/img/posting0603/sample.png)

*샘플 실행화면*

우선 class를 이용해서 QtWidgets에 있는 widget들을 활용한다.
class 내에서

```python
  self.setWindowTitle('My app')
  self.move(300, 300)
  self.resize(400,200)
  self.show()
```

이 부분을 확인 하면,
* setWindowTitle() 은 창의 제목 설정
* move(x,y) 는 x,y로 위치 이동시킨다.
* resize(w,h) 는 크기를 w와 h크기로 조절
* show() 는 위젯을 스크린에 보여준다.

```python
app = QApplication(sys.argv)
```
위 코드처럼 어플리케이션 실행시에는 객체를 생성해서 실행해야한다.

- - -
간단하게만 시작 했는데도 시간이 꽤 오래걸렸다. 일단 기본틀을 시작했으니 다음단계부터는 더 빨리 나가자
