---
layout: post
title:  "Python의 GUI, PyQt5 공부(1)"
date:   2019-06-03
excerpt: "Python, PyQt5 공부하기"
tag:
- Hyun
- Game
- Python
- PyQt5
comments: false
---

[PyQt5 Tutorial](https://wikidocs.net/book/2165)
 지난 공부에 이어서 기초 공부 진행!
 - - -

## 창 띄우기
- - -
#### setGeometry
 창의 위치와 크기를 표현하는 하는 메서드로
 ```python
 self.setGeometry(300,300,300,200)
 ```
 라고 설정하면 앞의 두 값은 x,y 위치, 뒤의 두 값은 너비와 높이를 결정하고, move()와 resize() 메서드를 합친 효과가 있다.

## 버튼 만들기
- - -
 예제로 quit버튼을 만들기가 있는데,
```python
from PyQt5.QtWidgets import QPushButton
btn = QPushButton('Quit', self)
btn.move(50, 50)
btn.resize(btn.sizeHint())
btn.clicked.connect(QCoreApplication.instance().quit)
```
 btn이라는 변수에 QPushButton이라는 메서드를 이용해서 버튼 값을 생성해주고, move(), resize()를 이용해 위치 설정해 준다.
 그리고 나서 btn.clicked.connect()를 이용해서 버튼을 눌렀을 때에 대해 어떤 행동을 취하는지를 설정해준다.

## 툴팁 나타내기
- - -
 어떤 위젯에 마우스를 가져다 댔을 때 말풍선형태로 나타나도록하는 방법이다.
```python
QToolTip.setFont(QFont('font명',size))
btn.setToolTip('tooltip 작성')
```
setFont를 이용해서 사용할 font를 지정한다. 그리고 나서 setToolTip()메서드를 이용해서 원하는 툴팁을 작성해준다.
## 각 종 바 설정하기
- - -
![Main Window]({{site.url}}/assets/img/posting0606/toolbar.png)*QMainWindow 공식문서*

 MenuBar, Toolbars, DocWidget, StatusBar같은 고유의 레이아웃을 갖고 있다.
 기존에 QWidget을 사용하던 것대신 QMainWindow를 전달해서 사용해주어야 한다.

### 상태바 만들기
- - -
```python
self.statusBar().showMessage('Ready')
```
* 상태바에 나타나게 하려면 코드처럼 statusbar의 showMessage메서드를 이용한다.
* 텍스트를 사라지게 하려면 clearMessage()를 이용하거나 showMessage() 메서드에 표시되는 시간을 설정한다.
* 현재 상태바 텍스트를 가져오려면 currentMessage() 메서드 사용

### 메뉴바 만들기
- - -
[QMenuBar 공식문서](https://doc.qt.io/qt-5/qmenubar.html)

예제
```python
exitAction = QAction('Exit',self)
exitAction.setShortcut('Ctrl+Q')
exitAction.setStatusTip('Exit application')
exitAction.triggered.connect(qApp.quit)

menubar = self.menuBar()
menubar.setNativeMenuBar(False)
fileMenu = menubar.addMenu('&File')
fileMenu.addAction(exitAction)
```
 메뉴바를 만들고 exit이라는 선택지를 통해 종료시키는 기능을 만들어 줬다.

```python
exitAction = QAction('Exit',self)
exitAction.setShortcut('Ctrl+Q')
exitAction.setStatusTip('Exit application')
exitAction.triggered.connect(qApp.quit)
```
* QAction()메서드를 이용해서 액션을 지정해주고, 이 메서드에는 QAction(QIcon(이미지파일),'Action의 이름',QObject값)을 저장해준다. 아이콘 이미지와 이름은 생략이 가능하다.
* setShortcut()을 이용해서 shortcut을 지정해 줄수 있다.
* setStatusTip()을 이용해 마우스 올렸을 때 상태바에 설명 나오도록 설정
* triggered.connect()을 이용해 해당 메뉴바를 클릭했을 때 일어날 이벤트를 지정해준다.

```python
menubar = self.menuBar()
menubar.setNativeMenuBar(False)
fileMenu = menubar.addMenu('&File')
fileMenu.addAction(exitAction)
```
* menuBar()를 이용해 메뉴파를 생성한다.
* menuBar.addMenu()를 이용해서 file메뉴를 만들어주고, '&'를 이용하면 단축키를 간단하게 설정할 수 있다. 해당문자 앞에 '&'를 써주면 단축키로 바로 설정된다. 지금 사용해준 경우는 'Alt+F'로 자동 설정이 되었다.
* addAction()을 통해서 아까 만들어준 Action을 하도록 지정해준다.

### 툴바 만들기
- - -
menuBar 만들기와 거의 유사한 모습을 보이고, 거기에 icon을 추가한다.
```python
exitAction1 = QAction(QIcon('exit.png'),'Exit', self)
exitAction1.setShortcut('Ctrl+Q')
exitAction1.setStatusTip('Save application')
exitAction1.triggered.connect(qApp.quit)

self.toolbar = self.addToolBar('Exit')
self.toolbar.addAction(exitAction1)
```
메뉴바와 거의 유사한 모습을 보이고
```python
QAction('아이콘','설명',Qobject값)
```
이런식으로 사용하면된다. 그리고 이후에 추가를 menubar대신 toolbar를 이용하면 된다.

### 코드 정리 및 결과
- - -
<details markdown="1">
<summary>코드내용</summary>
```python

import sys
from PyQt5.QtWidgets import *
from PyQt5.QtGui import QFont,QIcon
from PyQt5.QtCore import QCoreApplication
class MyApp(QMainWindow):

    def __init__(self):
        super().__init__()

        self.initUI()

    def initUI(self):
        exitAction = QAction('Exit',self)
        exitAction.setShortcut('Ctrl+Q')
        exitAction.setStatusTip('Exit application')
        exitAction.triggered.connect(qApp.quit)

        exitAction1 = QAction(QIcon('exit.png'),'Exit', self)
        exitAction1.setShortcut('Ctrl+Q')
        exitAction1.setStatusTip('Save application')
        exitAction1.triggered.connect(qApp.quit)

        self.toolbar = self.addToolBar('Exit')
        self.toolbar.addAction(exitAction1)

        menubar = self.menuBar()
        menubar.setNativeMenuBar(False)
        fileMenu = menubar.addMenu('&File')
        fileMenu.addAction(exitAction)


        self.statusBar().showMessage('Ready')

        QToolTip.setFont(QFont('SansSerif',10))

        btn = QPushButton('Quit', self)
        btn.setToolTip('This is a <b>QPushButton</b> widget')

        btn.move(100, 100)
        btn.resize(btn.sizeHint())
        btn.clicked.connect(QCoreApplication.instance().quit)
        self.setWindowTitle('My app')
        self.setGeometry(300,300,300,200)
        self.show()

if __name__ == '__main__':

    app = QApplication(sys.argv)
    ex = MyApp()
    sys.exit(app.exec_())
```
</details>
![결과 화면]({{site.url}}/assets/img/posting0606/result.png)
