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

## Part 2-2 admin 사이트 만들기
- - -

### 어드민 사이트 소개 및 유저 만들기
- - -
스탶이나 클라이언트가 컨텐츠 추가, 변경 삭제를 하기 위한 어드민 사이트를 만드는 일은 창조성이 필요하지 않으므로 장고는 모델의 어드민 인터페이스를 자동으로 만들어 준다. 컨텐츠를 업데이트 하는 "컨텐츠 퍼블리셔(content publishers)"와 독자가 컨텐츠를 볼 수 있는
"퍼블릭 사이트(public site)"가 확실히 분리되어 있는 곳에서 뉴스회사에서 처음 사용 되었다. 여기서 기사들을 업데이트 하고 게재하는 걸 하나의 인터페이스로 묶어, 컨텐츠를 한곳에서 수정하도록 만들었다. 어드민 사이트는 사이트 관리자를 위한 기능이다.


**어드민 유저 만들기**
루트 디렉토리에서
```console
$ python manage.py createsuperuser
```
를 입력하면 원하는 유저네임 입력, 이메일 주소 입력, 그리고 패스워드 입력까지 진행된다.

![진행창]({{src.url}})
