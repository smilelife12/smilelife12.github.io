---
layout: post
title:  "[개인 프로젝트] 공부 - Django"
date:   2019-09-26
excerpt: "나만의 웹 크롤러 만들기, Django"
tag:
- Hyun
- Web-Crawling
- Python
- Django
- Project
comments: true
---

## Django 공부
- - -

Django의 공부는 인프런의 [나의 첫 Django 앱 만들기](https://www.inflearn.com/course/%EB%82%98%EC%9D%98-%EC%B2%AB-django-%EC%95%B1-%EB%A7%8C%EB%93%A4%EA%B8%B0#description/)를 이용해서 시작을 해보려 한다.

- - -

## OT
- - -
 이 학습은 djangoproject.com 에서 제공하는 "Writing your first Django app"의 번역을 통해서 진행이 된다.

## Part 1. 장고 처음 배우기
- - -

### django 설치
- - -
 장고의 설치의 경우 conda를 이용해 설치하기 위해
 ```console
 conda install -c anaconda django
 ```
 를 이용해서 설치를 해준다.

### 프로젝트 만들기
- - -
장고를 초기셋업을 통해 실행을 해보자. 프로젝트를 실행할 폴더를 생성하고
```console
django-admin startproject mysite
```
를 통해서 `mysite`라는 이름의 디렉터리를 만들어 준다.

그러면
```
mysite/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        wsgi.py
```
와 같이 생성이 된다. 각각
- manage.py : 장고 프로젝트와 다양한 방법으로 커뮤니케이션 하는 커맨드 라인 유틸리티.
- myste/ : 이 디렉터리는 실제 프로젝트의 파이썬 패키지.
- mysite/\__init__.py : 아무것도 들어있지 않은 빈 파일이며, 현재 디렉터리가 파이썬 패키지임을 알려준다.
- mysite/setting.py : 장고 프로젝트의 셋팅과 설정이 포함. [Django setting](https://docs.djangoproject.com/en/1.10/topics/settings/) 에 자세한 내용이 있음.
- mysite/urls.py : 장고 프로젝트 안의 URL을 선언하는 곳. 장고 사이트의 컨탠츠 목록. [URL dispatcher](https://docs.djangoproject.com/en/1.10/topics/http/urls/) 에서 자세한 내용 확인.
- mysite/wsgi.py : WSGI 프로토콜을 사용하는 웹서버가 프로젝트의 페이지를 보여주기 위해 사용하는 파일. [How to deploy with WSGI](https://docs.djangoproject.com/en/1.10/howto/deployment/wsgi/)에 자세한 내용 확인.

### 개발용 서버 사용하기
- - -
이제 프로젝트가 실행이 가능한지에 대한 확인을 위해 manage.py 파일이 있는 디렉터리에서
```console
python manage.py runserver
```
를 실행한다. 현재 `sqlparse`가 설치되지 않아 실행이 안되는데,
```console
conda install -c anaconda sqlparse
```
를 통해서 설치 후 다시 실행한다. 그렇게 하면 성공했다는 메세지가 나타나고 [http://127.0.0.1:8000/](http://127.0.0.1:8000/) 에서 서버가 실행 된다. 그리고 접속을하면

![결과창]({{site.url}}/assets/img/posting0926/img1.png)

과 같은 결과창이 뜨게된다. 여기서 만약 포트를 바꾸고 싶다면
```console
python manage.py runserver 'portnumber'
```
portnumber위치에 원하는 포트 번호를 작성해주면 된다. 또한 ip도 변경 가능한데
```console
python manage.py runserver 'ipaddress':'portnumber'
```
를 작성하면 된다. 이 내용은 [runserver](https://docs.djangoproject.com/en/1.10/ref/django-admin/#django-admin-runserver)에 자세히 있다.

### 투표 앱 만들기
- - -
이 프로젝트를 이용해 웹 어플리케이션을 만들어보자. 장고는 코드를 만드는 것에 열중할 수 있도록 기본 앱 디렉터리 구조를 자동으로 생성해주는 유틸리티를 제공한다.
> **Projects vs apps**
앱은 어떠한 기능을 하는 웹 어플리 케이션을 말한다. 프로젝트란, 특정 웹사이트의 어플리케이션들과 그 구성의 집합을 말한다. 프로젝트는 여러개의 앱을 포함할 수 있고, 하나의 앱은 여러개의 프로젝트에 포함될 수 있다.

투표 앱을 `mysite`의 서브 모듈이 아닌 `manage.py` 바로 옆에 만들어 최상 모듈로 임포트 되도록 만들어 준다. 새로운 앱을 만들기 위해 `managa.py`가 있는 디렉터리로 이동해
```console
python manage.py startapp polls
```
를 입력한다.

그렇게 하면
```
polls/
  __init__.py
  admin.py
  apps.py
  migrations/
      __init__.py
  models.py
  tests.py
  views.py
```
위와 같은 구성을 가지는 `polls` 디렉터리가 생성되게 된다.

### 첫 뷰 만들기
- - -
`polls/view.py`를 열어 다음 코드를 입력한다.

```python
from django.http import HttpResponse


def index(request):
    return HttpResponse("Hello, world. You're at the polls index.")
```
이 뷰를 이용하기 위해선 URLconf를 만들어 뷰를 URL에 맵핑해야한다. polls 디렉터리 안에 URLconf를 만들기 위해서 `urls.py`라는 파일을 만든다. 그리고
```python
from django.conf.urls import url

from . import views

urlpatterns = [
url(r'&$',views.index,name='index'),
]
```
를 입력해준다.

> #### 정규표현식(Regular Expression) URLConf
[참조](https://wayhome25.github.io/django/2017/03/18/django-ep2-regx/)
>##### 정규 표현식
예시
- `^(시작)`,`$(끝)`을 사용하여 해당 패턴에 완전히 일치하는 것 찾음
- `r(raw)` 로 시작하여 이스케이프 문자를 한번 더 쓰지 않아도 된다.
예시
```
r"[0-9]{1,3}" or r"\d{1,3}" # 최대 3자리 숫자
r"010[1-9]\d{6,7}" # 10자리 혹은 11자리 휴대폰 번호.
```
>##### URLConf
[참조](https://docs.djangoproject.com/en/2.2/topics/http/urls/)app을 위해 디자인하기 위해서 만들어주는 모듈을 말한다. 이 모듈은 view와 url간의 매핑을 위해서 사용하는 것이다.

이 스텝은 루트 URLconf 가 `polls.urls` 모듈을 보게 해준것이다. 그리고 상위 의 `mysite/urls.py`에서  `urlpatterns`리스트에도 임포트 해주자.

```python
from django.conf.urls import include, url
from django.contrib import admin

urlpatterns = [
    url(r'^polls/', include('polls.urls')),
    url(r'^admin/', admin.site.urls),
]
```
`include()`함수는 루트 URLconf가 다른 URLconf를 참조할 수 있도록 해준다. `include()`함수의 정규 표현식은 문자열 마지막을 표시하는 `$`가 아닌 `/`를 마지막에 가지고 있다. 이는 매칭되는 부분만 잘라내고, 나머지 문자열을 `include()`함수가 지정한 다른 URLconf로 보내는 역할을 하는데, 이는 URL을 간단히 plug-and-play할 수 있도록 하기 위함이다. 투표 앱은 자신의 URLconf인 `polls/urls.py`안에 자신만의 URL을 가지므로 `/polls`나 '/fun_poll/' 등 관리자가 원하는 어떤 `root path`로도 설정할 수 있다.
이제 이를 실행해 보자. 그리고 'http://localhost:8000/polls/'에 들어가면 입력한 내용이나타난다.


**`url()`: regex**
정규표현식을 나타내면서 문자열 패턴 매칭을 위한 구문이다. request 요청이 들어오고 URL과 매칭된 정규 표현식을 찾을때 까지 모든 정규 표현식을 확인한다.정규 표현식은 GET과 POST의 파라미터와 도메인 이름은 확인하지 않는다. 너무 복잡하게 사용하지 말고 어느정도만 알고 있으면 된다.

**`url()``: view**
장고가 정규 표현식으로 일치하는 문자열을 찾고, 인수로 전달된 특정 view 함수를 실행한다. `HttpRequest` 오브젝트를 첫번째 인수로, 정규 표현식으로 캡쳐한 값을 두번째 ㅇㄴ수로 전달한다.

**`url()`: kwargs**
임의로 만들어진 키워드 인수로, 파이썬 사전으로 타겟 뷰로 전달.

**`url()`: name**
URL에 이름을 지정하고, 장고 앱 어딘가에서 특히 탬플릿과 같은 파일에서 참조가 가능, 이 기능을 사용하면 파일 하나만 수정하여 앱 전체 글로벌 적용이 가능하다.
