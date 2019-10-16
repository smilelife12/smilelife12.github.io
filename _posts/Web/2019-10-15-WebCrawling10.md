---
layout: post
title:  "[개인 프로젝트] 공부 - Django 6"
date:   2019-10-15
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

장고 홈페이지의 [Writing your first Django app, part5](https://docs.djangoproject.com/en/3.0/intro/tutorial05/)를 이어서 공부해보자.

- - -

## 자동 테스트에 대한 소개
- - -
### 자동 테스트란?
- - -
테스트는 코드가 제대로 작동되는지 확인하는 간단한 루틴이다.   
테스트는 다른 레벨에서 작동한다. 어떤 것은 작은 디테일한 부분을 테스트하기도하고, 어떤 것은 소프트웨어 전체의 작동에 대해 테스트하기도 한다. 이건 [Tutorial 2](https://docs.djangoproject.com/en/1.10/intro/tutorial02/) shell을 이용해 진행했던 것과 비슷하다.  
자동 테스트의 다른점은 테스트 작업이 시스템에서 작동을 해준다는 점이다. 테스트 셋을 하나 만들어 놓으면 앱을 수정할 때 코드가 제대로 작동하는지 체크하며, 수동 테스트와 달리 시간을 잡아먹지 않는다.


### 왜 테스트를 만들어야 하는가?
- - -
지금까지 진행한 것으로 적당히 배웠고, 더 배우는게 불필요해 보일 수 있다. 왜냐면 투표 앱은 잘 작동하기 때문이다. 그리고 더 이상 투표앱에 변화를 줄것 도 없어보인다. 하지만, 이 투표앱을 만드는 것에서 끝마칠 것이 아니라면 더욱 배워야 한다.  
**테스트는 시간을 단축 시켜줄 것이다.**  
 테스트는 '이 것이 제대로 작동하는지 체크'를 만족하는 것이다. 하지만, 복잡한 문제를 풀게 되면 요소들의 복잡한 상호작용들이 수십개는 있을 것이다.  
한 요소의 변화는 생각지도 못한 어플리케이션의 변화를 일으킬 수도 있다. 이 작업이 여전히 작동하는 것을 체크하는 것은 함수들이 20여개의 다른 다양한 방법으로 여전히 작동하고 문제없는 것을 확인해야한다. 이는 시간 낭비이다.  
자동 테스트가 시간을 줄여주는 건 명백한 사실이다. 만약 뭐가 잘못되면, 테스트가 코드의 오작동 부분을 찾아낼 수 있을 것이다.  
가끔 이 일은 잡일 같아보이고 창조적이고 생산적이지 않아보일 수 있지만, 만들고 있는 코드가 제대로 작동하는지 확인하기위해 필요한 작업이다. 또한, 일일이 수작업으로 찾아 테스트해서 어디서 잘못이 생겼는지 찾는거보단, 훨씬 시간이 단축된다.  

**또한, 테스트는 단순히 문제를 찾는 것이 아닌, 그를 예방해준다.**  
개발을 할 때, 테스트를 부정적인 측면으로만 생각하는 것은 잘못된 생각이다.  
테스트없이는 제대로 작동하는지, 의도한 대로 작동하는지 알 수 없다. 자신이 만든 코드라 하더라도 때때로 어떤식으로 작동하는지 찾아나가야 하는 경우가 있다.  
테스트는 코드 내부로부터 확인해 나가기 때문에 정확히 파악할 수 있다. 심지어 어떻게 잘못돌아가고 있는지 파악하지 못하는 상황에서도 말이다.  

**테스트는 코드를 더 매력적으로 만든다.**  
아무리 코드를 잘 만들어도 다른 개발자들이 그 코드의 테스트가 부족하면 거부할수 있다. 만든 코드가 정확히 돌아가는지 확인을 해야 다른 개발자들도 사용하기 편하다.  

**테스트는 팀이 같이 일할 수 있도록 돕는다.**  
혼자 개발하는게 아닌 팀으로 개발을 하게 될 경우 테스트는 굉장한 이점을 갖는다. 다른 사람들이 혹은 내가 타인의 코드에 접근할 때 테스트를 거치고 접근해야 하므로 정확히 알아야한다.  

### 기본 테스트 전략
- - -
어떤 프로그래머들은 `test-driven development`를 따른다. 테스트를 하고 나서 그들의 코드를 작성하는 방식이다. 이는 반직관적이지만, 대부분의 사람이 하는 방식이다. 문제에 대해 설명하고, 코드를 만들어 해결하는 방식이다.  
대부분은 테스트를 하는 사람들은 새로운 코드를 만들고, 나중에 이 코드가 테스트가 필요하다고 느낀다. 테스트를 빠르게 하는 것은 좋지만, 시작을 하는 것만으로도 너무 늦지는 않다.  
가끔 테스트를 어떻게 쓸지 어려운 경우가 있다. 만약 파이썬 코드를 수천줄 쓴 상태라면 고르기 쉽지 않을 것이다. 이런 경우 도움되는 테스트 코드를 먼저 작성하고, 추후 수정을 하며 새로운 것들에 대해 처리한다.  


### 첫 번째 테스트 작성
- - -

#### 버그를 구분하자
- - -
`polls` 어플리케이션에는 작은 버그가 있다. `Question.was_published_recently()` 메소드가 `True`를 리턴하는 경우는 `Question`이 최근에 만들어 진 경우이다. 하지만 미래의 값을 갖게 되는 경우도 `True`를 리턴하는 오류가 발생한다.
버그의 존재를 확인해 보기위해서 쉘을 이용해보자.
```shell
>>> import datetime
>>> from django.utils import timezone
>>> from polls.models import Question
>>> # create a Question instance with pub_date 30 days in the future
>>> future_question = Question(pub_date=timezone.now() + datetime.timedelta(days=30))
>>> # was it published recently?
>>> future_question.was_published_recently()
True
```
위의 `recent`는 맞지 않는 경우이다.  

#### 버그를 찾아낼 테스트를 만들자.
- - -
이 테스트 파일을 작성 위해 `tests.py`을 수정해주자. `polls` 디렉터리 안의 파일을 다음과 같이 수정한다.
```python
import datetime

from django.utils import timezone
from django.test import TestCase

from .models import Question


class QuestionMethodTests(TestCase):

    def test_was_published_recently_with_future_question(self):
        """
        was_published_recently() should return False for questions whose
        pub_date is in the future.
        """
        time = timezone.now() + datetime.timedelta(days=30)
        future_question = Question(pub_date=time)
        self.assertIs(future_question.was_published_recently(),False)
```
`django.test.TestCase`의 서브클래스의 메소드들을 `Quetions` 인스턴스를 이용해 만든다. 그리고 `was_published_recently()`가 False인지 확인하도록 만든다.  

#### 테스트 작동시키기
- - -
작성 후에 콘솔에 다음과 같이 입력한다.
```console
$ python manage.py test polls
```
이와 같이 하면 테스트를 진행하고 성공 실패를 보여준다. 이 과정에서

- `python manage.py test polls` 에서 `polls` 어플리케이션에 있는 tests를 진행한다.
- 여기서 `django.test.TestCase` 클래스를 발견했고
- 테스트를 위한 데이터베이스를 만들어준다.
- 테스트 메소드를 진행한다
- `test_was_published_recently_with_future_question`에서 `Question` 인스턴스를 만들고 `pub_date`가 30일 지난 걸 만들어 준다.
- 그리고 `assertIs`를 이용해서 `True`로 리턴되는 걸확인하고, 우리가 원하는 결과는 `False`이므로 틀렸다는 것을 전달한다.


#### 버그 픽스하기
- - -
문제가 `Question.was_published_recently()`에 있다는 걸 알 수 있다. 이 값이 `pub_date`가 미래를 나타내면 `False`를 리턴해야 한다. 이를 바꾸기 위해 단순히 접근해 `models.py`에서 변경을 해준다.
```python
def was_published_recently(self):
    now = timezone.now()
    return now - datetime.timedelta(days=1) <= self.pub_date <= now
```
위와 같이 수정하고 테스트를 진행하면 성공했다고 나오나.


#### 좀더 포괄적인 테스트
- - -
두개의 테스트 메서드를 추가해서 테스트 해보자.
```python
def test_was_published_recently_with_old_question(self):
    """
    was published_recently() returns False for questions whose pub_date
    is older than 1day
    """
    time = timezone.now() - datetime.timedelta(days=1, seconds=1)
    old_question = Question(pub_date=time)
    self.assertIs(old_question.was_published_recently(), False)

def test_was_published_recently_with_recent_question(self):
    """
    was published_recently() returns False for questions whose pub_date
    is within the last day.
    """
    time = timezone.now() - datetime.timedelta(hours=23, minutes=59, seconds=59)
    recent_question = Question(pub_date=time)
    self.assertIs(recent_question.was_published_recently(), True)
```
이를 통해 총 3개의 메서드가 생겼고 이는 과거, 최근, 미래에 대한 테스트를 하는 것들이다.


### 뷰를 테스트하자
- - -
poll 어플리케이션은 정해진 것이 없다. 어떤 question 도만들 수 있고, `pub_date`도 마음대로 작성할 수 있다. 이를 우리는 바꾸어야 한다.

#### 뷰에 대한 테스트
- - -
버그를 픽스할 때, 테스트를 작성하고, 코드를 고친다. 사실 이는 test-driven 개발 방식이고 이는 우리가 하는 일에 중요한 요소는 아니다.  
첫 테스트에서 우리는 코드의 내부 행동에 대해 주목했다. 테스트를 위해 우리는 코드의 작동을 체크 하기를 원했고, 이게 웹브라우저에서 유저가 겪을 일이기 때문이었다.


#### 장고 테스트 클라이언트
- - -
장고는 테스트 `Client`를 제공한다. 유저가 코드를 뷰레벨에서 본다고 시뮬레이션 하는 것이다. 이를 우리는 `tests.py`나 쉘에서 해볼 수 있다.
우선 쉘을 통해서 진행해보자.
```shell
>>> from django.test.utils import setup_test_envrionment
>>> setup_test_envrionment()
```
`setup_test_envrionment()`은 일시적인 renderer를 설치한다. 이 renderer는 우리가 추가적인 반응에 대한 행동을 실행한다. `response.context`같은 반응들이다. 이 메서드를 테스트 데이터베이스에 설치하지 않으면, 실제 만들어 놓은 것에 맞춰서 반응하게 된다. 그래서 기대하지 않았던 결과들이 나타나게 된다.  
다음으로 우리는 클라이언트 클래스가 필요하다
```shell
>>> from django.test import Client
>>> client = Client()
>>> # get a response from '/'
>>> response = client.get('/')
Not Found: /
>>> # we should expect a 404 from that address; if you instead see an
>>> # "Invalid HTTP_HOST header" error and a 400 response, you probably
>>> # omitted the setup_test_environment() call described earlier.
>>> response.status_code
404
>>> # on the other hand we should expect to find something at '/polls/'
>>> # we'll use 'reverse()' rather than a hardcoded URL
>>> from django.urls import reverse
>>> response = client.get(reverse('polls:index'))
>>> response.status_code
200
>>> response.content
b'\n    <ul>\n    \n        <li><a href="/polls/1/">What&#x27;s up?</a></li>\n    \n    </ul>\n\n'
>>> response.context['latest_question_list']
<QuerySet [<Question: What\'s up?>]>
```
이런 식의 요청에 대한 답변을 받아올 수 있다.  


#### view를 개선하기
- - -
 poll의 리스트들은 아직 만들어지지 않은 것들을 보여준다.(`pub_date`가 미래인 것들)

이를 수정하기 위해 `views.py`를 수정한다.

```python
from django.utils import timezone

def get_queryset(self):
    """Return the last five published questions."""
    return Question.objects.filter(pub_date__lte=timezone.now()).order_by('-pub_date')[:5]
```
`Question.objects.filter(pub_date__lte=timezone.now())`를 이용해서 쿼리셋의 `pub_date`가 `timezone.now`와 작거나 같은 걸 찾는다.


#### 새로운 뷰에 대한 테스트
- - -
이제 `runserver`를 통해 진행하면 `Questions`를 만들 때 과거와 미래의 날짜를 가진 질문을 만들고, 그것들을 확인하는 것을 통해 예상대로 동작하는 것을 알 수 있다.  
`polls/test.py` 파일을 수정해주자

```python
def create_question(question_text, days):
    """
    Create a question with the given 'question_text' and published the
    given number of 'days' offset to now (negative for questions published
    in the past, positive for questions that have yet to be published).
    """
    time = timezone.now() + datetime.timedelta(days=days)
    return Question.objects.create(question_text=question_text, pub_date=time)


class QuestionIndexViewTests(TestCase):
    def test_no_question(self):
        """
        If no questions exist, an appropriate message is displayed.
        """
        response = self.client.get(reverse('polls:index'))
        self.assertEqual(response.status_code, 200)
        self.assertContains(response, "No polls are available.")

        self.assertQuerysetEqual(response.context['latest_question_list'], [])

    def test_past_question(self):
        """
        Questions with a pub_date in the past are displayed on the index page.
        """
        create_question(question_text="Past question.",days=30)
        response= self.client.get(reverse('polls:index'))
        self.assertContains(response, "No polls are available.")
        self.assertQuerysetEqual(response.context['latest_question_list'], [])

    def test_future_question_and_past_question(self):
        """
        Even if both past and future questions exist, only past questions
        are displayed.
        """
        create_question(question_text="Past question.", days=-30)
        create_question(question_text="Future question.", days=30)
        response = self.client.get(reverse('polls:index'))
        self.assertQuerysetEqual(response.context['latest_question_list'], ['<Question: Past question.>'])

    def test_two_past_questions(self):
        """
        The questions index page may display multiple questions.
        """
        create_question(question_text="Past question1.", days=-30)
        create_question(question_text="Past question2.", days=-5)

        response = self.client.get(reverse('polls:index'))
        self.assertQuerysetEqual(response.context['latest_question_list'], ['<Question: Past question 2.>','<Question: Past question 1.>'])
```

위의 `create_question`을 통해서 question들을 임의로 생성할수 있게 해주고, `test_no_questions`는 questions을 만들지 않았다는 이야기이다. 그래서 메세지가 "No polls are available." 그리고 `latest_question_list`가 비어있다는것을 보여야 한다. 이를 위해 `assertContains`와 `assertQuerysetEqual`를 사용한다.  
`test_past_question`을 통해서는 question이 이미 존재했다는 것을 나타내야한다.  
`test_future_question`은 미래시간으로 question을 생성해서 그 값이 저장이 안되는 것을 확인하는 것이다. 그리고 저장 순서에 대해서도 체크한다.


#### DetailView 에 대한 테스트
- - -
`DetailView` 역시 테스트를 해야하는데 미래의 questions 에 대해 index가 나타나지 않고, 유저가 URL에 접근할 수 있어야 한다. 그래서 `polls/views.py` 에서 수정을 약간해준다.
```python
def get_queryset(self):
    """
    Excludes any questions that aren't published yet.
    """
    return Question.objects.filter(pub_date__lte=timezone.now())
```
위와 같은 함수를 추가해준다.  
그리고 이제 test를 위해 `polls/tests.py`파일을 수정한다.
```python
class QuestionDetailViewTests(TestCase):
    def test_future_question(self):
        """
        The detail view of a question with a pub_date in the future
        returns a 404 not found.
        """
        future_question = create_question(question_text='Future question.', days=5)
        url = reverse('polls:detail', args=(future_question.id,))
        response = self.client.get(url)
        self.assertEqual(response.status_code, 404)

    def test_past_question(self):
        """
        The detail view of a question with a pub_date in the past
        displays the question's text.
        """
        past_question = create_question(question_text='Past Question.', days=-5)
        url = reverse('polls:detail', args=(past_question.id,))
        response = self.client.get(url)
        self.assertContains(response, past_question.question_text)
```

#### 더 많은 테스트에 대한 아이디어들
- - -
`get_queryset`을 이용해 `ReslutView`와 test 클래스를 만들어 줄 수 있을 것이다. 지금까지 만든 것과 비슷한 방식으로 만들 수 있을 것이고, 반복이 많이 나올것이다.  
그리고 `Questions`이 `Choices`없이 만들어 질 수도 있다. 이를 확인하기 위해 `Quesions`을 `Choices`없이 만들고 이를 만들지 못하도록 하고, 있게 만들면 만들수 있도록 한다.  
아마 admin 유저는 만들어지지 않은 `Questions`들을 볼 수 있을 것이다. 하지만 대부분의 방문객에게는 보여지지 않는다.


### 테스트는 많을 수록 좋다.
 - -
 테스트를 만들 때 모든 경우를 생각해도 좋다. 이렇게 테스트를 거친 프로그램들이 유용하고 계속해서 쓰일 수 있을 것이다.
