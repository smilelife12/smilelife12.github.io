---
layout: post
title:  "JS하고나니 django"
date:   2019-10-23
excerpt: "django 공부"
tag:
- Hyun
- Django
comments: true
---

## Django
- - -
웹크롤링을 목적으로 진행하며 공부했던 Django를 여기로 다시 가져와보자. 중구난방으로 진행중이긴 하지만 복습도 할겸해서 진행하는 거로!
[장고걸스 튜토리얼](https://tutorial.djangogirls.org/ko/) 을 통해서 진행하자!

## 목적
- - -
기존에 첫 장고앱 만들기를 통해서 기본적인 공부를 했는데 좀더 해보자.  

## 설정 변경
- - -
해당 디렉터리의 설정을 고쳐야하는데 시간과 정적파일 경로에 대한 추가를 해준다.  

```python
TIME_ZONE = 'Asia/Seoul'
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR,'static')
```
그리고 애플리켕션 배포시 호스트 이름과 일치하지 않기 때문에
```python
ALLOWED_HOSTS = ['localhost', '127.0.0.1', '[::1]', '.pythonanywhere.com']
```
를 추가해 준다. 비어있는경우 로컬호스트에 대해서 항상 유효하므로 다시 작성할 때 추가해 준다.

## 데이터베이스 설정
- - -
해당 데이터베이스는 이미 설치되어 있고, 이를 콘솔창에서 `migrate`를 통해서 만들어준다.
그리고 나서 이를 실행하기 위해
```console
python manage.py runserver
```
를 통해서 확인해주면 된다.

## 객체
- - -
모델을 제작해주는데 객체 형태로 제작을 한다. 각 모델에 속성과 메서드를 만들어준다. 그래서 블로그에 필요한 게시글 객체를 만들고, 여기에 포함되는 속성으로 제목, 내용, 글쓴이, 작성일, 게시일 이렇게 포함시킨다. 그리고 이 글을 출판하는 메서드를 만들어주자. 이를 위해서 어플리케이션을 만들어주고, `settings.py`에 어플리케이션을 추가해준다.  

```console
$ python manage.py startapp blog
```

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'blog',
]
```

## 모델 만들기
- - -
`Model`이라는 객체는 `blog/models.py`에 만들어 준다. 여기에 다음 코드를 입력하자.  

```python
from django.conf import settings
from django.db import models
from django.utils import timezone


class Post(models.Model):
    author = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    title = models.CharField(max_length=200)
    text = models.TextField()
    created_date = models.DateTimeField(
            default=timezone.now)
    published_date = models.DateTimeField(
            blank=True, null=True)

    def publish(self):
        self.published_date = timezone.now()
        self.save()

    def __str__(self):
        return self.title
```

이제 한줄 한줄 학습해보자.  

`class Post(models.Model)`에서 모델을 정의하는 코드이다.
- `models.Model` 을 통해서 장고 모델임을 의미하고, 장고의 데이터베이스에 저장되어야하는 것을 알린다.  

이제 내부 속성에 대해서 보면,  
`title`,`text`,`created_date`,`published_date`, `author`가 있다. 각 속성을 정의 할때, 어떤 종류의 데이터 타입이 있는지 입력해주는 것이다.
>이 부분은 데이터베이스의 `mySql`에서 `create`하는 부분이랑 비슷한 것으로 보이며 이를 응용한 것 같다. 그러니까 테이블의 속성들에 대해 만들어주고, 이에 대한 속성이 갖는 값을 정의하는 것인데 언어가 다를 뿐인 거로 생각된다.  

- `models.CharField` : 글자 수가 제한된 텍스트 정의해서 사용
- `models.TextField` : 글자 수가 제한이 없는 긴 텍스트를 위한 속성
- `models.DateTimeField` : 날짜와 시간을 의미
- `models.ForeignKey` : 다른 모델에 대한 링크를 의미

나머지 정의들은 [https://docs.djangoproject.com/en/2.0/ref/models/fields/#field-types](https://docs.djangoproject.com/en/2.0/ref/models/fields/#field-types)를 통해서 알 수 있다.


다음으로 메서드를 확인하면, `def publish(self)`를 통해 메서드를 만들어 주었다. 그래서 해당 메서드를 사용하면 `published_date`에 값을 저장해준다. `def __str__(self)`를 통해서 문자열이 출력되도록한다.  

## 테이블 만들기
- - -
이제 만든 모델을 이용해 테이블을 만들어 주기 위해 콘솔창에
```console
$ python manage.py makemigrations blog
```
를 입력해준다. 이렇게 하면 데이터베이스에 반영할 수 있도록 migration file을 생성하고, 다음 데이터베이스 모델 추가를 반영한다.  
```console
python manage.py migrate blog
```


## 관리자 설정
- - -
관리자 화면을 변경하기 위해 `settings.py`에 `LANGUAGE_CODE=ko`로 바꿔준다. 그리고 `python manage.py createsuperuser`를 통해서 슈퍼유저를 만들어주고 [http://127.0.0.1:8000/admin/](http://127.0.0.1:8000/admin/)를 접속해서 관리자페이지를 들어갈 수 있다.


## 배포
- - -
우선 git을 통해 만든 파일들을 업로드 해놓는다.
그리고 `PythonAnywhere` 에서 배포를 한다. [https://www.pythonanywhere.com/](https://www.pythonanywhere.com/)에서 회원 가입을 한다. 무료버전으로 가입 후 사이트에서 console에서 bash를 선택해서 git을 이용한 클론을 생성한다. 그리고 나서 python과 같은 필수 요소들을 설치해준다. 그 후에 다시 홈으로 돌아와서 웹을 들어가고, 웹 앱을 설정을 수동 설정으로 만들어주고 가상환경에 대해 경로를 설정해준다.

다음으로 WSGI 파일을 설정해주는데, 이는 장고가 `WSGI 프로토콜`을 사용하기 때문이다. Web에서 code를 보면`/var/www/<your-username>_pythonanywhere_com_wsgi.py` 부분이 있고 여기를 아래와 같이 수정하면 표현이 된다.
```python
import os
import sys

path = '/home/<your-PythonAnywhere-username>/my-first-blog'  # PythonAnywhere 계정으로 바꾸세요.
if path not in sys.path:
    sys.path.append(path)

os.environ['DJANGO_SETTINGS_MODULE'] = 'mysite.settings'

from django.core.wsgi import get_wsgi_application
from django.contrib.staticfiles.handlers import StaticFilesHandler
application = StaticFilesHandler(get_wsgi_application())
```

드디어 웹사이트 하나가 만들어 졌다. 짞짞짞!
