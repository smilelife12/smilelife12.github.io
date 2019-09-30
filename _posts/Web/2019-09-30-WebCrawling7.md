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
