---
layout: post
title:  "[개인 프로젝트] 공부 - Django 5"
date:   2019-10-14
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


## Part 4-1. 간단한 유저폼 만들기
- - -
### 간단한 유저폼 만들기
- - -
`polls/detail.html`을 업데이트해서 템플릿에 변화를 준다.

```html
<h1>{{ question.question_text}}</h1>

{% if error_message %}<p><strong>{{ error_message }}</strong></p>{% endif %}

<form action="{% url 'polls:vote' question.id %}" method="post">
  {% csrf_token %}
  {% for choice in question.choice_set.all %}
    <input type="radio" name="choice " id="choice{{ forloop.counter }}" value"{{ choice.id }}" />
    <label for="choice{{ forloop.counter }}">{{ choice.choice_text }}</label><br />
   {% endfor %}
 <input type="submit" value="Vote" />
</form>
```
코드에 대해 해석해보면
- 질문에 대해서 `radio` 버튼으로 나타나게 한다. 각 버튼의 `value`는 Choice 오브젝트의 id가 되도록 만들었다. 각 버튼의 name은 `"choice"`로 한다. 그러면 choice=#으로 POST 데이터가 전송된다.  
- 폼의 `action`속성에 `{% url 'polls:vote' question.id %}`으로 `method` 속성을 `post`로 설정했고, 서버데이터를 변경하는 폼이므로 method를 `post`로 사용하는 것은 아주 중요하다. `get`은 반대로 사용하는 것이므로 웹 개발시 중요한 요소이다.  
- `forloop.counter`는 `for` 루프가 몇 번째 루프를 도는지 나타낸다.  
- POST 폼을 만들 때는 Cross Site Request Forgeries 를 유의해야 한다. 이는 백엔드 서버를 보호해야 하므로 모든 POST폼에 내부 URL을 티켓으로 갖는 `{%csrf_token %}`템플릿 태그를 사용하면 된다.  

이제 임시로 설정했던 `vote()` 함수를 제대로 만들어준다. `polls/views.py`를 수정하자.
```python
from django.shortcuts import get_object_or_404, render
from django.http import HttpResponseRedirect, HttpResponse
from django.urls import reverse

from .models import Choice, Question
# ...
def vote(request, question_id):
  question = get_object_or_404(Question, pk=question_id)
  try:
    selected_choice = question.choice_set.get(pk=request.POST['choice'])
  except (KeyError, Choice.DoesNotExist):
    # 에러메세지와 함께 다시 디스플레이 한다.
    return render(request, 'polls/detail.html',{
    'questoin': question,
    'error_message': "You didn't select a choice.",
    })
  else:
    selected_choice.votes += 1
    selected_choice.save()
    return HttpReponseRedirect(reverse('polls:results', args=(question.id,)))
```
- `request.POST`는 파이썬 딕셔너리와 비슷한 오브젝트로 POST를 통해 전달된 데이터를 키이름으로 엑세스한다. 위의 경우 `request.POST['choice']`로 선택된 choice 오브젝트의 ID를 문자열로 리턴한다. `request.POST`의 값들은 항상 문자열이다.
- Choice 카운트를 증가시킨 후, 일반적인 `HttpResponse`가 아닌 `HttpResponseRedirect`를 리턴하는데 이는 유저가 리디렉팅되는 URL을 인자로 받는다. 위 코드의 주석에서 설명한 것과 같이, POST 데이터를 성공적으로 핸들링했을 때는 항상 `HttpResponseRedirect`를 리턴해야 한다. 이것은 모든 웹개발에서 적용된다.
- 위의 예제는 `HttpResponseRedirect` 생성자 안에서 `reverse()` 함수를 사용햇고, 뷰 안에서 하드코딩을 피할 수 있게 도와준다. 이 함수를 통해 보고싶은 뷰의 이름과 이 뷰를 가르키는 URL 패턴의 일부인 변수를 전달 받는다.

이제 `vote()` 뷰는 질문의 투표결과를 보여주는 result 페이지로 리디렉팅 한다. 이 결과를 보여주는 뷰를 만들어보자.
```python

def results(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    return render(request, 'polls/results.html', {'question': question})
```
이를 보여줄 `polls/results.html` 템플릿도 만들어보자.
```html
<h1>{{ question.question_text }}</h1>

<ul>
{% for choice in question.choice_set.all %}
    <li>{{ choice.choice_text }} -- {{ choice.votes }} vote{{ choice.votes|pluralize }}</li>
{% endfor %}
</ul>

<a href="{% url 'polls:detail' question.id %}">Vote again?</a>
```
이를 이용해 choice들이 나타나고 해당 값들이 얼마나 투표되었는지 볼 수 있다.

## Part 4-2. 함수 뷰 대신에 클래스 형식의 제네릭 뷰 사용하기.
- - -
### 제네릭 뷰 사용하기 : Less cod is better
- - -
`detail()`과 `results` 뷰는 비슷한 구조를 가지며 `index()`와도 비슷하다. 이러한 일반적인 코드의 반복을 없애주는 숏컷을 제공한다. 이를 진행하기 위해서
1. URLconf를 수정한다.
2. 필요없는 뷰 함수를 삭제한다.
3. 장고의 제너릭 뷰를 사용한다.

### URLconf 수정
- - -
`polls/urls.py`를 다음과 같이 수정한다.
```python
from django.conf.urls import url

from . import views

app_name = 'polls'
urlpatterns = [
  url(r'&$', views.IndexView.as_view(), name='index'),
  url(r'^(?<pk>[0-9]+)/$', views.DetailView.as_view(), name='detail'),
  url(r'^(?<pk>[0-9]+)/results/$', views.ResultsView.as_view(), name='results'),
  url(r'^(?<pk>[0-9]+)/vote/$', views.vote(), name='vote'),
]
```
`<question_id>`를 `<pk>` 로 바꾸고, view로 연결하는 부분이 조금씩 바뀌었다.

### 뷰 수정
- - -
각각 `index`,`detail`,`results` 뷰를 지우고, 제너릭 뷰를 대치해 만들어준다.
```python
from django.shortcuts import get_object_or_404, render
from django.http import HttpResponse, HttpResponseRedirect
from django.urls import reverse
from django.template import loader
from django.http import Http404
from django.views import generic

from .models import Question, Choice

class IndexView(generic.ListView):
    template_name = 'polls/index.html'
    context_object_name = 'latest_question_list'

    def get_queryset(self):
        """Return the last five published questions."""
        return Question.objects.order_by('-pub_date')[:5]


class DetailView(generic.DetailView):
    model = Question
    template_name = 'polls/detail.html'


class ResultsView(generic.DetailView):
    model = Question
    template_name = 'polls/results.html'
```
`view()`는 수정하지 않고 그대로 둔다. 위에서 `ListView`와 `DetailView`라는 2개의 제너릭 뷰를 사용한다. `ListView`는 오브젝트 리스트를 보여주고, `DetailView`는 특정오브젝트의 디테일을 보여준다.
- 제너릭뷰에서 모델을 어떤걸 설정할지 정하면, "model"속성으로 전달된다.
- `DetailView`제너릭 뷰는 URL에서 캡처된 기본키 'pk'를 전달받는다. 그렇기 때문에 `question_id`에서 `pk`로 변환한 것이다.

`DetailView` 제너릭 뷰는 기본적으로 `<app name>/<model name>_detail.html` 이름의 템플릿을 사용한다. 위의 예제는 `template_name` 속성을 사용해 지정했지만 지정하지 않으면 `polls/question_detail.html`를 사용하게 될 것이다. `template_name`을 사용하면 원하는 이름을 지정할 수 있다. `ListView`의 경우는 `context_object_name`을 이용해 자동으로 값을 생성해 주지만, 위에 처럼 직접 변수를 지정할 수도 있다.

이로써 한글로 해석해준 강좌는 끝났다 여기서 이어서 하기 위해 장고 공식홈페이지에서 진행할 수 있다.
