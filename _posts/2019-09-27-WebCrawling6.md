---
layout: post
title:  "[개인 프로젝트] 공부 - Django 2"
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

인프런의 [나의 첫 Django 앱 만들기](https://www.inflearn.com/course/%EB%82%98%EC%9D%98-%EC%B2%AB-django-%EC%95%B1-%EB%A7%8C%EB%93%A4%EA%B8%B0#description/)에서 이어서 공부

- - -

## OT
- - -
 이 학습은 djangoproject.com 에서 제공하는 "Writing your first Django app"의 번역을 통해서 진행이 된다.

## Part 2-1. 데이터베이스
- - -

### 데이터베이스 셋업
- - -
데이터베이스 셋업을 위해 `mysite/settings.py`를 편집한다. 장고의 디폴트 옵션은 SQLite설정을 가지고 있다. SQLite의 경우 파이썬에 포함되어 있으므로 다른 설치가 필요하지않다. 이 외의 다른 데이터베이스도 사용가능하니 적절히 설치하여 변경하면 된다.  
- `ENGINE` : 자신의 데이터베이스에 맞게 입력
  - `django.db.backends.sqlite3`, `django.db.backends.postgresql`,'django.db.backends.mysql',`django.db.backends.oracle` 등이 있다.
- `NAME` : 자신의 데이터 베이스 이름이다. SQLite 사용시 데이터베이스는 하나의 파일로 컴퓨터에 저장된다.  

만약 다른 데이터베이스를 사용하게 되면 `USER`, `PASSWORD`,와 `HOST` 등의 키 값이 추가되여야 한다. [참조: DATABASE](https://docs.djangoproject.com/en/1.10/ref/settings/#std:setting-DATABASES)  
> **QLite 이외의 데이터베이스 사용 방법**  
SQLite 이외를 사용하고자 한다면, 데이터베이스 프로그램의 프롬프트에서 다음 명령어 실행
```console
"CREATE DATABASE database_name;"
```
 또한 `mysite/settings.py`에 설정한 권한을 주어야한다. SQLite의 경우는 자동 생성되므로 아무것도 하지 않아도 된다.

`mysite/settings.py` 파일 수정시 `TIME_ZONE`을 설정하는데 한국의 경우 `TIME_ZONE = 'Asia/Seoul'` 행을 추가한다.

이제, `INSTALLED_APPS` 셋팅을 확인한다. 기본적으로 디폴트 앱을 포함한다.
- `django.contrib.admin` : 어드민 사이트
- `django.contrib.auth` : 인증 시스템
- `django.contrib.contenttypes` : 컨텐츠 타입을 위한 프레임워크
- `django.contrib.sessions` : 세션 프레임워크
- `django.contrib.messages` : 메세지 프레임워크
- `django.contrib.staticfiles` : 스태틱 파일 관리를 위한 프레임워크

일단 테이블을 만들 필요가 있으므로 다음 명령어를 실행한다.
```console
$ python manage.py migrate
```
`migrate` 명령어는 `INSTALLED_APPS`를 확인 후, `mysite/sttings.py` 파일과 기본 어플리케이션이 가지고 있는 데이터 베이스 마이그레이션 파일에 따라 필요한 테이블을 만든다.

### 모델 만들기
- - -
> **장고의 철학**  
모델은 데이터를 데이터 베이스에 저장하기 위한 데이터의 기초이고, 장고는 [DRY Principle](https://docs.djangoproject.com/en/1.10/misc/design-philosophies/#dry)이라는 중복제거 원리를 따른다.  
모델은 마이그레이션 파일 포함하며, Ruby On Rails와는 다르게 모델 파일을 통해 마이그레이션 파일을 만든다. 기본적으로 이 마이그레이션 파일은 데이터베이스 모델에 맞춰 업데이트를 하기위한 기록 정도이다.

이제 투표 앱을 위해 `Question`과 `Choice`라는 모델을 만들어 보자. `Question`의 경우 질문과 공개 날짜 필드를 갖고있다. `Choice`는 선택과 투표라는 필드를 가진다. 그리고 각 `Choice`모델은  하나의 `Question` 모델과 관계가 있다. 이를 `polls/models.py` 파일을 수정하여 만든다.
```python
from django.db import models

# Create your models here.
class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')

class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
```

각 모델은 클래스 변수를 포함하고, 각각 클래스 변수는 모델 데이터가 저장될 데이터베이스의 필드와 같다. 데이터 베이스 필드는 모델 클래스 안에 정의 되는 `Field`클래스의 오브젝트와 같다. (ex. 캐릭터 필트들 위한 `CharField`, 날짜를 위한 `DateTimeField`). 이것들이 장고에 각 필드에 어떤 형식의 데이터가 저장될지 알려준다. 그리고 이를 지칭하는 변수명이 각 필드의 데이터베이스 필드의 이름이된다. 나는 이 이름을 파이썬 코드에서, 데이터베이스는 컬럼 이름으로 사용을 하는 것이다. 그리고 `models.DataTimeField('data published')` 처럼 추가적으로 처음 봤을 때 헷갈릴 수 있는 변수에 대한 설명을 추가해 줄 수 있다. 그리고 `models.CharField(max_length=200)` 처럼 필수 인수를 필요로 하는 것도 있다. 그리고 `models.ForeignKey(Question, on_delete=models.CASCADE)`처럼 두 클래스 간의 관계를 표현하는 것도 있다.

### 모델 활성화
- - -
우선 `mysite/settings.py`를 수정하는데, `INSTALLED_APPS`에 `polls.apps.PollsConfig`를 추가해준다.
```console
$ python manage.py makemigrations polls
```
이를 실행하면
```
Migrations for 'polls':
  polls/migrations/0001_initial.py:
    - Create model Chooice
    - Create model Question
```
와 같이 나타난다. `makemigrations`의 경우는 새로 추가한 파일이나 변경된 파일이 있음을 알려주고, 변경된 내용을 migration 파일로 써 저장하도록 한다. 이를 `/migrations/0001_initial.py`에서 확인할 수 있다. 아직 데이터베이스 스키마는 만들지 않았고 이를 자동으로 관리해주는 명령어는 `migrate`인데 이는 나중에 하고, `sqlmigrate` 명령어는 실제 SQL명령어를 실행하지는 않고 실행될 내용만 보여준다.
```console
$ python managae.py sqlmigrate polls 0001
```
를 실행해 보면
```
BEGIN;
--
-- Create model Question
--
CREATE TABLE "polls_question" (
  "id" integer NOT NULL PRIMARY KEY AUTOINCREMENT,
  "question_text" varchar(200) NOT NULL,
  "pub_date" datetime NOT NULL);
--
-- Create model Choice
--
CREATE TABLE "polls_choice" (
  "id" integer NOT NULL PRIMARY KEY AUTOINCREMENT,
  "choice_text" varchar(200) NOT NULL,
  "votes" integer NOT NULL,
  "question_id" integer NOT NULL REFERENCES "polls_question" ("id") DEFERRABLE INITIALLY DEFERRED);

CREATE INDEX "polls_choice_question_id_c5b4b260" ON "polls_choice" ("question_id");
COMMIT;
```
이러한 결과물이 나온다.

이제 만들어진 데이터베이스를 사용하기위해서
```console
$ python manage.py migrate
```
를 실행해준다. 실행되지 않은 모든 migration을 적용해준다. 이 부분에서 기억해야될 3단계는
- 모델변경(models.py)
- `python manage.py makemigrations`를 실행해 마이그레이션 파일 생성
- `python manage.py migrate`를 싱행해 변경 내용을 데이터 베이스에 적용


### API 사용하기
- - -
이제 이를 이용하기 위해 장고에서 제공하는 파이썬 쉘을 열어보자.
```console
$ python manage.py shell
```
단순히 "python" 명령어를 실행하지 않고, 위으 명령어를 실행하는 이유는 `manage.py`를 이용해 `DJANGO_SETTINGS_MODULES`의 환경변수를 셋팅하고 장고에 `mysite/settings.py`파일의 파이썬 임포트 패스를 제공하기 위해서이다.

이를 이용해서 모델클래스를 임포트하고 쿼리를 생성할 수 있다.
```
>>> from polls.models import Question, Choice   # 우리가 만든 모델 클래스를 임포트 합니다.

# No questions are in the system yet.
>>> Question.objects.all()
<QuerySet []>

# 새로운 Question 오브젝트 생성.
# 타임존 서포트가 디폴트 settings 파일에 활성화되어 있으므로,
# pub_date에 datetime.datetime.now()이 아닌
# timezone.now()을 사용하여 tzinfo를 포함하고 있는 datetime을 대입하여 주십시오.
>>> from django.utils import timezone
>>> q = Question(question_text="What's new?", pub_date=timezone.now())

# 위에서 클래스 오브젝트를 생성하였지만 메모리상에만 만든것이고 아직 데이터베이스에 저장되지는 않았습니다.
# save() 명령어를 실행하여 데이터베이스에 저장을 해봅시다.
>>> q.save()

# 이제 오브젝트가 ID를 가진 것을 볼 수 있습니다.
# 데이터베이스에 따라 "1"가 아니고 "1L"를 출력하는 경우가 있습니다.
# 이것은 사용하고 있는 데이터베이스가 integetr를 long integer 오브젝트로 리턴하기 때문이니 아무 문제 없습니다.
>>> q.id
1

# 파이썬 속성을 통해 모델의 필드에 엑세스해 보십시오.
>>> q.question_text
"What's new?"
>>> q.pub_date
datetime.datetime(2012, 2, 26, 13, 0, 0, 775217, tzinfo=<UTC>)

# 파이썬 속성을 변경하여 필드 값을 변경한 후, save() 함수를 실행하여 주십시오.
>>> q.question_text = "What's up?"
>>> q.save()

# objects.all() 클래스 메소드는 데이터베이스에 있는 모든 question 오브젝트를 보여줍니다.
>>> Question.objects.all()
<QuerySet [<Question: Question object>]>
```

이런식으로 확인하면, `<Question: Question object>`라고 전달되므로 아무 정보가 없다 그래서 각각 의 `__str__()` 메서드를 추가해준다. 그리고 추가적으로 시간에 관한 값을 활용하기 위해 `timezone` 을 이용한다.
```python
import datetime

from django.db import models
from django.utils import timezone

class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')


    def was_published_recently(self):
        return self.pub_date >= timezone.now() - datetime.timedelta(days=1)
    def __str__(self):  # __unicode__ on Python 2
        return self.question_text


class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)

    def __str__(self):  # __unicode__ on Python 2
        return self.choice_text
```

이를 편집하고 쉘에서 다음과 같은 동작을 할 수 있다.
```
>>> from polls.models import Question, Choice

# 조금 전에 추가한 __str__() 메소드가 출력한 내용을 확인하십시오.
>>> Question.objects.all()
<QuerySet [<Question: What's up?>]>

# 장고는 키워드 인수를 이용한 강력한 데이터베이스 검색 API 를 제공합니다.
>>> Question.objects.filter(id=1)
<QuerySet [<Question: What's up?>]>
>>> Question.objects.filter(question_text__startswith='What')
<QuerySet [<Question: What's up?>]>

# 올해 개제된 질문을 검색해봅시다.
>>> from django.utils import timezone
>>> current_year = timezone.now().year
>>> Question.objects.get(pub_date__year=current_year)
<Question: What's up?>

# 존재하지 않는 ID를 검색하면 에러 메세지가 출력됩니다.
>>> Question.objects.get(id=2)
Traceback (most recent call last):
    ...
DoesNotExist: Question matching query does not exist.

# 데이터베이스의 기본키로 검색하는 것이 가장 보편적이기에
# 장고는 기본키 검색 쇼트컷 (shortcut) 을 제공합니다.
# 밑의 코드는 Question.objects.get(id=1)와 같습니다.
>>> Question.objects.get(pk=1)
<Question: What's up?>

# 이번에는 우리가 만든 커스텀 메소드가 잘 실행되는지 확인합시다.
>>> q = Question.objects.get(pk=1)
>>> q.was_published_recently()
True

# Question에 2개의 Choice를 추가해보죠. create 콜은 새로운 Choice 오브젝트를 만들고,
# 데이터베이스의 INSERT 구문의 역할을 하며, 새로운 choice를 기존의 choice 세트에 추가 하고,
# 새로 만든 Choice 오브젝트를 리턴합니다. (한 문장이 엄청 기네요;;)
# 장고는 외래키 관계의 반대편 오브젝트의 세트를 가지고 있습니다.
# (e.g. question이 가지고 있는 choice 세트) 이것들은 API를 통해 액세스할 수 있습니다.
>>> q = Question.objects.get(pk=1)

# 기본키가 1인 Question 오브젝트가 관계하고 있는 choice 세트를 출력해 봅시다. -- 아직 아무것도 없습니다.
>>> q.choice_set.all()
<QuerySet []>

# 3개의 choice를 추가해보죠.
>>> q.choice_set.create(choice_text='Not much', votes=0)
<Choice: Not much>
>>> q.choice_set.create(choice_text='The sky', votes=0)
<Choice: The sky>
>>> c = q.choice_set.create(choice_text='Just hacking again', votes=0)

# Choice 오브젝트는 자신이 관계하고 있는 Question 오브젝트에 액세스할 수 있는 API를 가지고 있습니다.
>>> c.question
<Question: What's up?>

# 반대로 Question 오브젝트도 Choice 오브젝트에 액세스할 수 있는 API를 가지고 있습니다.
>>> q.choice_set.all()
<QuerySet [<Choice: Not much>, <Choice: The sky>, <Choice: Just hacking again>]>
>>> q.choice_set.count()
3

# API는 자동으로 모든 관계를 추적합니다.
# 더블 언더스코어를 사용하여 관계를 연결합니다.
# 이것은 제한 없이 원하는 만큼 연결할 수 있습니다.
# pub_date가 올해인 모든 Choice 오브젝트를 검색하여 봅시다.
# (위에서 만든 변수인 'current_year'를 다시 사용합시다.
>>> Choice.objects.filter(question__pub_date__year=current_year)
<QuerySet [<Choice: Not much>, <Choice: The sky>, <Choice: Just hacking again>]>

# delete()을 사용하여 이 중 하나의 choice를 삭제해 보죠.
>>> c = q.choice_set.filter(choice_text__startswith='Just hacking')
>>> c.delete()
(1, {u'polls.Choice': 1})
>>> q.choice_set.all()
<QuerySet [<Choice: Not much>, <Choice: The sky>]>
```
