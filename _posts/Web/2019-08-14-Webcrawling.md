---
layout: post
title:  "파이썬으로 영화 예매 프로그램"
date:   2019-08-14
excerpt: "인프런 - 파이썬으로 영화 예매"
tag:
- Hyun
- Web-Crawling
- Python
- AWS
comments: false
---

## 파이썬으로 영화 예매 프로그램 만들기

파이썬과 aws, 그리고 텔레그램을 이용해서 영화 예매 프로그램을 만들어 주는 과정을 기록하도록 하겠습니다.
이 과정은 [인프런 강좌](https://www.inflearn.com/course/영화예매-파이썬/)에서 공부를 시작합니다.

### 0-1 웹크롤링 준비하기
- - -
웹크롤링을 하는 방법은 bs4를 이용해서 진행할 예정이고, 이를 설치한다. bs4는 간단하게 anaconda를 이용해서 설치를 마쳤다. 설치방법은

`conda install -c anaconda beautifulsoup4`

를 cmd창에서 작성하면 설치가 된다.


### 0-2 request로 정보 가져오기
- - -
cgv홈페이지에서 크롤링을 해보는걸 테스트 해본다. cgv 홈페이지를 들어가서 여기서는 용산을 사용했지만, 나같은 경우는 왕십리를 이용하기로 했다. 그래서 크롬을 이용해 개발자도구로 들어가서 iframe영역에 시간표부분을 가져온다. 그를 이용해서 시간표 부분의 url 만 따온다.

- 예시는

```python
import requests
from bs4 import BeautifulSoup

url = 'http://www.cgv.co.kr/common/showtimes/iframeTheater.aspx?areacode=01&theatercode=0074&date=20190823'
html = requests.get(url)
print(html.text)
```

와 같이 진행한다.


### 1-1 bs4로 상영시간표 영화 제목 크롤링하기.

공식 문서를 이용해 bs4를 공부하는게 제일 좋고, 여기서 find()와 select()를 주로 이용할 예정이다. 이 중 select를 주로 사용할 예정이다. 이는 css 문법을 이용하면 그대로 사용할 수 있다.

이번에 제목만 가져오는 걸 해볼건데 가장 쉬운 방법은 개발자 도구를 가져오는 것이다. 거기서 간단하게 selector를 복사해서 붙여넣으면 된다. 하지만 이는 단 하나를 가져오게 되므로 다시 설정을 해보면 info-moive 클래스 안에 a태그에 제목이 들어온다. 즉, info-movie에 영화 리스트 들이 다 들어있다. 그래서 이 클래스를 select를 이용해 가져오고 그 값들을 for문을 통해서 a 태그에 들어있는 제목들만 뽑아 낼 수 있도록 설정해주면 영화 제목들만 출력이 된다. 그리고 나서 text값만 가져오고 strip을 통해서 빈 공간을 제거해 주면 제목만 출력되는 것을 볼 수 있다.

- 예시

```python
import requests
from bs4 import BeautifulSoup

url = 'http://www.cgv.co.kr/common/showtimes/iframeTheater.aspx?areacode=01&theatercode=0074&date=20190823'
html = requests.get(url)
soup = BeautifulSoup(html.text, 'html.parser')
title = soup.select('div.info-movie')
for i in title:
    print(i.select_one('a > strong').text.strip())
```

### 1-2 imax 영화 예매 오픈 여부 크롤링
- - -
그 전처럼 받아온 시간표 웹에서 imax 부분에 갖다 대면, spam 에서 imax class 가 지정 되어 있는것을 볼 수 있다. 이를 이용해서 있는 날은 이 class 값이 출력이 되고, 없는 경우 None 으로 출력이 된다. 이를 이용해 if문을 사용해서 예매 가능 불가능을 지정할 수 있다.

- 예시

```python
import requests
from bs4 import BeautifulSoup

url = 'http://www.cgv.co.kr/common/showtimes/iframeTheater.aspx?areacode=01&theatercode=0074&date=20190825'
html = requests.get(url)
#print(html.text)
soup = BeautifulSoup(html.text, 'html.parser')
imax = soup.select_one('span.imax')
if(imax):
    print('예매 있음')
else:
    print('예매 없음')
```

### 1-3 예매 오픈된 제목 크롤링
- - -
지금까지 사용한 방법을 이용해서, imax 예매를 오픈한 제목을 출력하는 방법을 찾는다.
여기서 그전에 사용했던 코드에서 imax 값에서 parent를 찾아간다. 이를 통해서 span.imax가 포함된 클래스를 찾고 여기서 다시 한번 info-movie, 그리고 처음 제목을 받아온것 처럼 a에서 strong 태그를 이용해 받아오면 된다.
`지금 까지 과정을 봤을 때 select내에서 `\>의 경우는 해당 태그로의 이동을 시켜주는 것으로 보인다 즉, 현재 클래스나 태그 내의 이동을 시켜주는 방법으로 보인다.`

- 예시

```python
import requests
from bs4 import BeautifulSoup

url = 'http://www.cgv.co.kr/common/showtimes/iframeTheater.aspx?areacode=01&theatercode=0074&date=20190814'
html = requests.get(url)
#print(html.text)
soup = BeautifulSoup(html.text, 'html.parser')
imax = soup.select_one('span.imax')
if(imax):
    imax = imax.find_parent('div', class_= 'col-times')
    title = imax.select_one('div.info-movie > a > strong').text.strip();
    print(title + ' 예매 있음')
else:
    print('예매 없음')
```

### 2-1 텔레그램 봇 구축하기
- - -
텔레그램을 사용하는 이유는 간단히 구축 가능하고 선톡 가능하며 여러 사용자가 이용이 가능하다. 텔레그램에서 봇을 이용하기 위해 봇을 생성해준다.
우선 텔레그램에 가입해서 bot을 만들어 주는 과정을 거친다. 만드는 과정은 찾기에서 botfather를 찾고 그 안에서 /start 를 해서 시작을 한 후, /newbot을 통해 새로운 봇을 만들고 이름을 지정한후 bot으로 끝나는 이름을 다시 설정해준다. 그러고 나면 http api를 주는데 이 값을 가지고 이제 파이썬에서 봇을 컨트롤 할 수 있다.

이제 파이썬에서 텔레그램을 컨트롤 하기 위해서 python-telegram을 설치해야 한다. conda 환경에서는 `conda install -c conda-forge python-telegram-bot `를 타이핑 해서 설치해 준다. 이를 통해 import를 할 수 있게 되었고, 메세지가 업데이트 된 값을 알 수도 있고, 봇에다가 메세지를 보낼 수도 있다.
```python
bot = telegram.Bot(token='http api값')
```
를 통해서 봇을 지정해 줄 수 있으며, Bot내의 getUpdates()라던가, sendMessage()를 이용해서 메세지를 주고 받을 수 있다.

-예시
```python
import telegram

bot = telegram.Bot(token='909220298:AAHOk9LxFNA_biAskOXzEHAt5ZdhIxNOn5M')

for i in bot.getUpdates():
  print(i.message)
bot.sendMessage(chat_id=916268458, text="테스트중.")
```
