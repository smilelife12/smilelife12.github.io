---
layout: post
title:  "파이썬으로 영화 예매 프로그램2"
date:   2019-08-16
excerpt: "인프런 - 파이썬으로 영화 예매"
tag:
- Hyun
- Web-Crawling
- Python
- AWS
comments: false
---

## 파이썬으로 영화예매 이어서
[인프런 강좌](https://www.inflearn.com/course/%EC%98%81%ED%99%94%EC%98%88%EB%A7%A4-%ED%8C%8C%EC%9D%B4%EC%8D%AC/lecture/20942)에서 공부

### 2-2 알리미에 텔레그램 봇 구축
- - -
만들었던 알리미에다가 텔레그램 봇을 적용하여 주고받도록 한다. 이는 생각보다 간단하다. 단순히, 텔레그램 봇에 보내던 메세지를 아까 알리미에 옮겨서 프린트 되던것을 메세지로 보내도록 지정만 하면된다.

- 예시

```Python
import requests
from bs4 import BeautifulSoup
import telegram

bot = telegram.Bot(token='909220298:AAHOk9LxFNA_biAskOXzEHAt5ZdhIxNOn5M')

url = 'http://www.cgv.co.kr/common/showtimes/iframeTheater.aspx?areacode=01&theatercode=0074&date=20190814'
html = requests.get(url)
#print(html.text)
soup = BeautifulSoup(html.text, 'html.parser')
imax = soup.select_one('span.imax')

for i in bot.getUpdates():
  print(i.message)

if(imax):
    imax = imax.find_parent('div', class_= 'col-times')
    title = imax.select_one('div.info-movie > a > strong').text.strip();
    bot.sendMessage(chat_id=916268458, text=title + ' 예매 있음')
else:
    bot.sendMessage(chat_id=916268458, text='예매 없음')
```


### 3-1 APScheduler설치
- - -
APScheduler를 conda를 이용해서 설치를 해준다. 이는 `conda install -c conda-forge apscheduler`라고 cmd창에 쳐서 설치를 해준다.

APScheduler의 경우는 내가 실행시킨 프로그램에서 어떤 함수를 특정시간에 실행 혹은 특정 시간마다 반복하도록 지정할 수 있는 모듈이다.

### 3-2 알리미에 스케쥴러 추가
- - -
기존에 만들어준 코드에다가 스케쥴러를 만들어서 파이썬을 실행할때만 검사하는 것이 아닌, 우리의 목적인 계속해서 열렸는지 확인해 줄 수 있도록 만들어준다. 이는 간단하게 할 수 있는 데, 기존 코드에다가 설치한 APScheduler 모듈을 가져와서 실행을 해주면 된다. 기존에 메세지를 보내는 부분을 함수화 시켜서 만들어주고, 그 메세지를 보내는 부분을 특정 시간마다 반복하도록 설정해 주면된다.

- 예시

```Python
import requests
from bs4 import BeautifulSoup
import telegram
from apscheduler.schedulers.blocking import BlockingScheduler

bot = telegram.Bot(token='909220298:AAHOk9LxFNA_biAskOXzEHAt5ZdhIxNOn5M')

url = 'http://www.cgv.co.kr/common/showtimes/iframeTheater.aspx?areacode=01&theatercode=0074&date=20190814'
html = requests.get(url)

def job_function():
    soup = BeautifulSoup(html.text, 'html.parser')
    imax = soup.select_one('span.imax')
    if(imax):
        imax = imax.find_parent('div', class_= 'col-times')
        title = imax.select_one('div.info-movie > a > strong').text.strip();
        bot.sendMessage(chat_id=916268458, text=title + ' imax 영화 예매 있음')
    else:
        bot.sendMessage(chat_id=916268458, text='예매 없음')

sched = BlockingScheduler()
sched.add_job(job_function,'interval', seconds=30)
sched.start()
```

### 3-3 조건에 따라 스케쥴러 중단
- - -
 최종적으로 예매가 오픈이 되면 더 이상 스케쥴러는 가동이 될 이유가 없다. 그러므로, 오픈이 되면 스케쥴러가 정지되도록 설정하고, 오픈이 안되었을 때도 메세지가 온다면, 계속해서 오기 때문에 이는 메세지를 보내지 않도록 설정해 놓는다.

 - 예시

 ```python
 import requests
from bs4 import BeautifulSoup
import telegram
from apscheduler.schedulers.blocking import BlockingScheduler

bot = telegram.Bot(token='909220298:AAHOk9LxFNA_biAskOXzEHAt5ZdhIxNOn5M')

url = 'http://www.cgv.co.kr/common/showtimes/iframeTheater.aspx?areacode=01&theatercode=0074&date=20190814'
html = requests.get(url)

def job_function():
    soup = BeautifulSoup(html.text, 'html.parser')
    imax = soup.select_one('span.imax')
    if(imax):
        imax = imax.find_parent('div', class_= 'col-times')
        title = imax.select_one('div.info-movie > a > strong').text.strip();
        bot.sendMessage(chat_id=916268458, text=title + ' imax 영화 예매 있음')
        sched.pause()

sched = BlockingScheduler()
sched.add_job(job_function,'interval', seconds=30)
sched.start()
```

### 4 AWS EC2 구축하고 실행하기
- - -
집에서 단순히 이 프로그램을 계속해서 켜놓는 것은 매우 힘든 일이다. 그렇기 때문에 서버를 생성하는게 좋은데 이는 AWS에서 제공하는 EC2서버를 이용해서 구축하는 것이 편리하다. AWS에서 ubuntu 서버를 열 예정인데, 윈도우에서는 ubuntu가 직접 지원이 안되므로 WSL을 설치하면 된다. 이는 마이크로 소프트 앱 스토어에서 받아도 되고, 다양한 방법이 있다. 이는 [마이크로 소프트 홈페이지](https://docs.microsoft.com/ko-kr/windows/wsl/install-win10)를 참고하여 설치하였다.
- - -
그리고 aws에서 페어키를 생성하고, 서버 인스턴스도 하나 생성해준다. 키파일을 저장한 값을 wsl로 옮겨야 하는데 이는 [aws 홈페이지 docs](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/WSL.html) 여기에 굉장히 친절하게 나와있다. Ubuntu 터미널 창에서 저장된 곳에서 복사할 곳을 지정하면 되는데

``cp /mnt/<Windows drive letter>/path/my-key-pair.pem ~/WSL-path/my-key-pair.pem ``

이런 식으로 하면 키가 복사가 된다. 그리고 나서 이를 이용해서 인스턴스에 연결을 하는 방법은

``sudo ssh -i /path/my-key-pair.pem ubuntu@ec2-198-51-100-1.compute-1.amazonaws.com``

를 이용하면 되는데 사용자의 아이디 같은 경우 만들어준 인스턴스에 따라 다르다고 생각하면 될거같고 \@뒤의 값은 본인이 생성한 인스턴스의 주소를 AWS에서 확인해서 그 값을 넣어주면 된다. 그러면 정상적으로 서버에 접속을 할 수 있다.

- - -
이제 파일을 이 서버로 옮겨야 하는데 가장 쉬운 방법은 FileZilla라는 프로그램을 설치하여 옮기는 것이다. FileZilla는 로컬 파일을 특정 서버에 옮길 수 있는 쉬운 방식이다. [FileZilla](https://filezilla-project.org/) 에서 다운 받아서 실행 후 사이트 관리자에 들어가서 새 사이트 생성 후에 프로토콜을 SFTH, 그리고 호스트를 본인의 아이피 주소, 그리고 키파일을 연결해주면 쉽게 파일을 옮겨 줄수 있다.

- - -
실행 파일을 서버에 모두 옮겼으면 Python 환경도 구축을 해주고 실행을 하면되는데, 여기서 서버를 끄면 그대로 Python파일도 꺼지기 때문에, 이를 방지하기 위해

``nohup python3 실행파일.py &``

를 이용해서 실행 파일이 그대로 진행되도록 설정해 준다. 그러면 내가 서버에서 접속을 끊더라도 백그라운드에서 계속해서 진행되기 때문에 이 값이 실행이 되기 때문에 예매가 오픈이 되면 그대로 메세지가 올 수 있게 설정이 된 것이다.

## 마치며
- - -
웹크롤링을 간단하게 배웠고, 파이썬 내의 스케쥴러 시스템 그리고 AWS도 간단하게 알게 되었다. 이를 이용하면, 재밌는 프로그램들을 만들어 볼 수 있을 것 같다.
