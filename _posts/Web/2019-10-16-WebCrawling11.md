---
layout: post
title:  "[개인 프로젝트] 공부 - 웹 크롤링 2"
date:   2019-10-16
excerpt: "나만의 웹 크롤러 만들기, with Django"
tag:
- Hyun
- Web-Crawling
- Python
- Django
- Project
comments: true
---

## 웹 크롤러로 복귀
- - -

장고의 기본적인 부분에 대해서 공부를 했고, 다시 [Django로 크롤링한 데이터 저장하기](https://beomi.github.io/gb-crawling/posts/2017-03-01-HowToMakeWebCrawler-Save-with-Django.html)로 돌아왔다.


## Django로 크롤링한 데이터 저장하기
- - -
파이썬에서 데이터를 체계적으로 관리할 수 있듣 `django`를 이용해 DB를 만들고 데이터를 저장하려 한다.

장고는 이미설치 했고 새로운 웹 서버를 만들어보자.
```console
$ django-admin startproject webserver
```
위와같이 하면 `webserver`폴더가 생기며 구조가 만들어진다.  

### 장고 앱 만들기
- - -
`manage.py` 파일을 통해 `startapp`이라는 명령어를 입력해 앱을 만들어준다.
```console
python manage.py startapp parsed_data
```
`parsed_data`라는 앱이 만들어진다.  
이를 해당 프로젝트의 `settings.py`의 `INSTALLED_APPS`에 추가해 주어야한다.  


### 마이그레이션
- - -
장고의 `python manage.py migrate`라는 명령어로 DB를 migrate 한다.
> **마이그레이션** 은 모델에 대한 변경 사항을 데이터 베이스 스키마로 전파하는 장고의 방식이다.  

```console
$ python manage.py migrate
```
이를 통해 DB 모델 변경내역 히스토리를 관리할 수 있도록 만들어 주었다.이제 DB를 관리해야한다.

- - -
다음 부분을 진행하면서 DB에 대한 처리에 대한 공부가 더욱 필요할 거 같아서 여기까지 하고, DB를 먼저 진행하도록 해야겠다.
