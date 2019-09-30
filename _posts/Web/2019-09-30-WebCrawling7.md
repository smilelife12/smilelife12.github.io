---
layout: post
title:  "[개인 프로젝트] 공부 - Django 3"
date:   2019-09-30
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

## Part 2-2 admin 사이트 만들기
- - -

스탶이나 클라이언트가 컨텐츠 추가, 변경 삭제를 하기 위한 어드민 사이트를 만드는 일은 창조성이 필요하지 않으므로 장고는 모델의 어드민 인터페이스를 자동으로 만들어 준다. 컨텐츠를 업데이트 하는 "컨텐츠 퍼블리셔(content publishers)"와 독자가 컨텐츠를 볼 수 있는
"퍼블릭 사이트(public site)"가 확실히 분리되어 있는 곳에서 뉴스회사에서 처음 사용 되었다. 여기서 기사들을 업데이트 하고 게재하는 걸 하나의 인터페이스로 묶어, 컨텐츠를 한곳에서 수정하도록 만들었다. 어드민 사이트는 사이트 관리자를 위한 기능이다.


**어드민 유저 만들기**
루트 디렉토리에서
```console
$ python manage.py createsuperuser
```
를 입력하면 원하는 유저네임 입력, 이메일 주소 입력, 그리고 패스워드 입력까지 진행된다.

![진행창]({{src.url}}/assets/img/posting0930/img1.png)

이 후 이를 확인하기 위해 서버를 구동하고 "/admin/"을 추가해서 들어가보면 나타난다. [http://127.0.0.1:8000/admin/](http://127.0.0.1:8000/admin/) 로 접근하면 로그인 페이지가 나타나며, 유저 목록을 확인해 볼 수 있다.  

![결과창]({{src.url}}//assets/img/posting0930/img2.png)
- - -

이제 여기서 투표앱을 적용시키기 위해 `Question` 오브젝트가 어드민 인터페이스를 갖도록 해야한다. `polls/admin.py` 파일을 다음과 같이 수정한다.

```python
from django.contrib import admin
from .models import Question

admin.site.register(Question)
```

그러면 모델이 추가되는 것을 확인할 수 있다.
![결과창]({{src.url}}//assets/img/posting0930/img3.png)

- - -

이제 여기서 각 모델들의 편집을 진행해 줄 수 있다. 만들어준 폼으로 인해 자동으로 수정 가능한 것들이 보여진다. 그래서 `models.py`를 수정하지 않아도 해당 admin을 통해서 각 값들의 수정이 이루어진다.


## Part 3-1. 뷰(View) 만들기
- - -

### 장고 철학
- - -

뷰는 장고 어플리케이션이 사용하는 웹페이지의 한 타입이다. 일반 적으로 특정 함수를 실행하고, 특정 템플릿을 갖고있다. 블로그 어플리케이션이면,
- 블로그 홈페이지
- 싱글 포스트 페이지
- 일 년간 아카이브 페이지
- 한 달간 아카이브 페이지 등..

이 나타난다.

투표 어플리케이션은 4가지 뷰를 갖게된다.
- Question "인덱스"페이지 - 최신 질문들을 보여주는 페이지
- Question "디테일"페이지 - 투표 할 수 있는 유저폼과 함께 하나의 질문을 보여주는 페이지
- Question "결과"페이지 - 특정 질문에 대한 결과를 보여주는 페이지
- 투표 액션 - 특정 질문에 대해 투표를 핸들링.


### 뷰 추가하기
- - -
`polls/views.py`에 몇개의 뷰를 추가해 보자. request 외에도 다른 인수도 추가적으로 인수를 받아올 예정이다.

```python
def detail(request, question_id):
    return HttpResponse("You're looking at question %s." % question_id)

def results(request, question_id):
    response = "You're looking at the results of question %s."
    return HttpResponse(response % question_id)

def vote(request, question_id):
    return HttpResponse("You're voting on question %s." % question_id)
```

그리고 이 뷰를 `url()` 함수를 이용해 `polls.urls` 모듈에 추가해 보자.

```python
from django.conf.urls import url

from . import views

urlpatterns = [
    # ex: /polls/
    url(r'^$',views.index,name='index'),
    # ex: /polls/5/
    url(r'^(P<question_id>[0-9]+)/$', views.detail, name='detail'),
    # ex: /polls/5/results/
    url(r'^(?P<question_id>[0-9]+)/results/$', views.results, name='results')
    # ex: /polls/5/vote/
    url(r'^(?P<question_id>[0-9]+)/vote/$', views.vote, name='vote'),

]
```
이는 정규식을 이용하기 때문에 정규식에 대한 내용을 한번 공부하는게 좋을 것 같다.

우선 이를 이용해서 `localhost:8000/polls/34/` 를 입력하면, `detail()` 메소드를 호출해 전달된 ID를 표시해 준다. 그 다음 `localhost:8000/polls/34/results/`나 `localhost:8000/polls/34/votes/`를 입력하면 그에 맞는 결과가 나타난다. 장고에서는 `mysite.urls`의 파이썬 모듈을 로딩하는데 이는 `settings.py`파일의 `ROOT_URLCONF`에 정의되어있기 때문이다. 그다음 `urlpatterns` 라는 이름의 변수를 찾아 정규 표현식을 확인한다. 그 다음 `^polls/`에 매칭되는 패턴을 찾으면, 매칭된 문자열(`"polls/"`)를 제외한 나머지 문자열인 `"34/"` URLconf인 `polls.urls`에게 보내 다음 프로세스를 진행한다. `polls/urls`는 `r^(?P<question_id>[0-9]+)/$`를 찾고 다음과 같이 `detail()` 뷰 함수를 호출한다.
```python
detail(request=<HttpRequest object>, question_id='34')
```
처럼 불러진다. 여기서 `question_id='34'`부분의 `34`는 `(?P<question_id>[0-9]+)`에서 가져온 숫자이다. 소괄호로 묶인 패턴이 캡처한 문자는 인자로써 뷰 함수에 전달된다. `?P<question_id>`부분은 문자를 담을 인수의 이름을 정하고, `[0-9]+`은 정규 표현식의 숫자 매치를 의미한다. URL 패턴이 정규표현식을 사용하기 때문에 아무 제한없이 모든 문자열을 캡처할 수 있다. URL에 `.html` 같은 익스텐션도 필요 없다.


### 제대로된 뷰(View) 만들기
- - -
각 뷰는 두개중 하나의 역할을 한다.
- 요청 페이지에 대한 컨텐츠를 포함한 `HttpResponse` 오브젝트를 리턴
- `Http404`와 같은 에러페이지를 출력하는 역할  
뷰는 데이터베이스의 레코드를 읽는 경우도 있다. 템플릿을 사용하거나, 외부(third-party) 템플릿을 사용하는 경우도 있다. 파이썬 라이브러리를 사용해 PDF, XML, ZIP등 원하는 걸 만들 수 있다. 결국 장고는 뷰로부터 `HttpResponse`나 에러페이지 둘 중하나만 리턴 받으면 된다. 데이터베이스 API를 뷰안에서 사용해보자. 인덱스 뷰를 수정해 데이터베이스의 저장된 5개의 질문을 보여주는 코드를 보자.
```python
from django.http import HttpResponse

from .models import Question


# Create your views here.
def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    output = ', '.join([q.question_text for q in latest_question_list])
    return HttpResponse(output)
```
그전에 만든 질문 "What's up?"이라는 오브젝트가 보인다. 이 때 페이지의 디자인을 변경하려면, 파이썬 코드를 수정해야한다. 이를 해결하기 위해 템플릿 시스템을 이용해 디자인과 파이썬 코드를 분리해보자. `polls` 디렉토리에 `templates` 디렉토리를 만들어주자. 장고는 이 디렉터리에서 템플릿을 찾는다. 프로젝트 셋팅 안의 `TEMPLATES`은 장고가 어떻게 템플릿을 로드하고 보여주는지에 대한 설정이 있다. 기본 템플릿 백엔드인 `DjangoTemplates`의 `APP_DIRS`옵션 이 `True`이다. `DjangoTemplates`백엔드는 기본적으로 `INSTALLED_APPS`에 등록되어 있는 각 어플리케이션 에서 "templates"라는 이름의 서브디렉터리를 찾는다. 지금 `templates`안에 `polls`라는 디렉토리를 만들고 `index.html`이란 이름의 템플릿을 만들자. 그리고 `app_directories` template loader는 위에 설명한것처럼 작동하므로 `polls/index.html`라는 경로를 사용해 간단히 참조할 수 있다.

다음 코드를 넣어주자
```html
{% if latest_question_list %}
    <ul>
    {% for question in latest_question_list %}
        <li><a href="/polls/{{ question.id }}/">{{ question.question_text }}</a></li>
    {% endfor %}
    </ul>
{% else %}
    <p>No polls are available.</p>
{% endif %}
```
그리고 나서
`views.py`에
```python
from django.http import HttpResponse
from django.template import loader

from .models import Question


def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    template = loader.get_template('polls/index.html')
    context = {
        'latest_question_list': latest_question_list,
    }
    return HttpResponse(template.render(context, request))
```
라고 수정해주자. 그러면 해당 템플릿이 작동하며 링크가 생성된다.

#### 숏컷 : `render()`
- - -
`HttpResponse`대신 `render()`를 이용해 `index()`를 수정하자.
```python
from django.http import HttpResponse
from django.shortcuts import render

from .models import Question

def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    context = {'latest_question_list': latest_question_list}
    return render(request, 'polls/index.html', context)
```
위와 다르게 `loader`와 `HttpResponse`를 임포트할 필요가 없음을 알 수 있다.
