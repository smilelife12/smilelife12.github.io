---
layout: post
title:  "[개인 프로젝트] 공부 - Django 4"
date:   2019-10-10
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

## 3-2 404 에러 메세지
- - -
### 404 예외처리(Raising a 404 error)
- - -
`polls/view.py`뷰 파일안의 디테일 뷰를 다음과 같이 수정한다.
```python
from django.http import Http404
from django.shortcuts import render

from .models import Question
# ...
def detail(request, question_id):
    question = Question.objects.get(pk=question_id)

    return render(request, 'polls/detail.html', {'question': question})
```

그리고 임시로 `polls/templates/polls/detail.html` 이라는 임시 템플릿 파일을 만들어 준다. 그리고
```html
{{ question}}
```
이라고 작성해 준다. 다음으로 예외처리를 `polls/views.py`의 디테일 뷰에 코드 추가한다.

```python
from django.http import Http404
from django.shortcuts import render

from .models import Question
# ...
def detail(request, question_id):
    try:
        question = Question.objects.get(pk=question_id)
    except Question.DoesNotExist:
        raise Http404("Question does not exist")
    return render(request, 'polls/detail.html', {'question': question})
```
이렇게 하면 `question` 아이디가 없는경우 예외가 발생하도록 설정이 된다.  
 이 방식 말고 `get()` 함수를 이용해서 발생 시킬 수도 있다. 여기서 사용하는 숏컷으로 `get_object_or_404()` 직관적으로 object를 받아오거나 404를 리턴하게 되는 것이다.


### 템플릿 시스템 이용하기
- - -
이제 앱의 `detail()` 뷰를 이용해서 `polls/detail.html` 템플릿에 `question` 오브젝트를 전달한다. 이를 표시하기 위해 새로운 코드를 템플릿에 추가한다.
```html
<h1>{{question.question.text}}</h1>
<ul>
{% for choice in question.choice_set.all %}
    <li>{{ choice.choice_text }}</li>
{% endfor %}  
</ul>
```
for문의 구문에 대해서는 [참조:docs](https://docs.djangoproject.com/en/1.10/ref/templates/builtins/#std:templatetag-for) 를 보면 되고, 템플릿 에 대해서는 [참조:docs](https://docs.djangoproject.com/en/1.10/topics/templates/)를 보면 될 것 같다.


### 템플릿에서 하드코딩 지우기
- - -
현재 하드코딩 된 부분은 `polls/index.html` 의 부분이 있다.
```html
<li><a href="/polls/{{ question.id }}/">{{ question.question_text }}</a></li>
```
여기서 `href="/polls/{{question.id}}/"` 가 하드코딩되어 있다. 여기서 `"polls.urls"`의 `"url()"` 함수 안에 name 파라미터를 정의하면 편리하게 할 수 있다.
```html
<li><a href="{% url 'detail' question.id %}">{{ question:question_text}}</a></li>
```
처럼 사용 할 수 있다. 위 코드에서 `polls.urls`에서 "name= 'detail'"이라고 되어 있는 부분을 찾는다. 그러면 찾을 수 있게 된다. 만약 디테일 뷰의 URL을 `polls/specifics/12`처럼 바꾸고 싶으면, `polls/urls.py`에 다음과 같이 추가하면된다.
```python
url(r'^specifics/(?P<question_id>[0-9]+)/$', views.detail, name='detail')
```
과 같이 작성하면된다.  


### 네임스페이싱 URL 이름
- - -
다양한 앱을 갖게될 경우 URL이름을 구분하기 위해서 네임스페이스를 정의한다. 그러기 위해서 `polls/urls.py`파일에 `app_name`이라는 변수를 이용해 어플리케이션의 네임스페이스를 정의한다.  

```python
from django.conf.urls import url

from . import vies


app_name = 'polls'
urlpatterns = [
  url(r'^$', views.index, name='index'),
  url(r'^(?P<question_id>[0-9]+)/$', views.detail, name='detail'),
  url(r'^(?P<question_id>[0-9]+)/results/$', views.results, name='results'),
  url(r'^(?P<question_id>[0-9]+)/vote/$', views.votes, name='vote'),
]
```
이렇게 수정한 뒤에 `polls/templates/polls/index.html` 에서
```html
<li><a href="{% url 'polls:detail' question.id %}">{{question.question_text}}}</a></li>
```
이렇게 수정해준다.  
