---
layout: post
title:  "[개인 프로젝트] 공부 - 웹 크롤링"
date:   2019-09-05
excerpt: "나만의 웹 크롤러 만들기"
tag:
- Hyun
- Web-Crawling
- Python
- Project
comments: false
---

## 웹크롤링 공부
- - -
프로젝트에 사용할 수 있도록 웹 크롤링을 공부해보려고 사이트를 찾아다녔다. 그러던 중, 간단하게 접근해 볼 수 있는 사이트를 찾았다.

[나만의 웹 크롤러 만들기](https://beomi.github.io/gb-crawling/) 에서 웹크롤링을 시도해 볼 수 있을 것으로 보이고, 기존에 영화 예매를 복습하며 공부하려 한다.

### requests와 BeautifulSoup 사용
- - -
웹크롤링을 위해서 위의 두 라이브러리를 설치하는데 이는 지난 번에 모두 설치했다.

### Session을 이용해 로그인 하기
- - -

여기서 Session이라는 것을 공부하게 되었다.
> ### 쿠키? 세션?
> - 쿠키는 유저가 웹 사이트를 방문할 때 사용자의 브라우저에 있는 작은 파일로, Key-Value를 로컬 브라우저에 저장하고, 서버는 이 정보를 읽어 HTTP요청에 대해 브라우저 식별
> - 로컬에 저장되는 점 때문에 악의적 사용자가 쿠키를 변조하거나 탈취해 정상적이지 않은 쿠키로 서버에 요청 보내는 경우 있음.
> - 세션은 위의 부분을 해결하기 위해 서버측에서 이용. 브라우저가 웹 서버에 요청한 경우 서버 내에 세션 정보를 파일이나 DB에 저장하고 클라이언트의 브라우저에 session-id라는 임의의 긴 문자열 줌.
> - 정리하자면, 방문자 컴퓨터의 메모리에 저장 하는 것이 쿠키, 세션은 웹서버가 서비스가 돌아가고 있는 서버에 저장하는 것
즉, 방문자가 웹서버에 접속해 있는 상태를 하나의 단위로 보고 세션이라고 칭한다는 것.

그래서 requests를 이용해서 session을 받아오는데
```python
import requests

s = requests.Session()
```
을 사용하여 세션을 받아온다.

- - -
그리고 클리앙이라는 사이트에 로그인을 파이썬을 통해서 하는 방법을 알려준다.
크롬의 개발자 툴을 이용해서 각각의 selector나 form 형식을 알 수 있다.

여기서는 로그인 버튼을 클릭하는 부분이
```html
<button type="button" onclick="auth.login()" name="로그인하기" class="button_submit">로그인</button>
```
와 같이 type이 button으로 이루어졌고, 클릭을 통해서 `auth.login()` 을 실행해서 로그인 가능 여부를 판별하는 방식이다. `auth.login()`을 분석하고, 이를 이용해 아이디와 패스워드를 전송하는 방식이다.

이걸 보고 다른 사이트에서 적용해보려고 했는데, 다른 사이트 들은
```html
<button type="submit">로그인</button>
```
처럼 버튼의 type이 submit인 경우가 많고 이는 다른 js파일에다가 데이터를 전송하는 방식이 많았고, 이는 다른 분석을 통해 데이터 전송을 해야했다... 아직 많이 헷갈리는 상태
- - -
그리고 나서 이를 이용해서 원하는 글을 읽어오는데, 해당 세션을 저장한 상태이므로 로그인이 유지된 상태로 진행되어, 로그인이 필요한 페이지를 접근할 수 있다. 그리고 나서, 원하는 글을 크롬을 이용해 selector를 복사하여 출력 한다.

```python
post_one = s.get('https://www.clien.net/service/board/rule/10707408')
soup = bs(post_one.text, 'html.parser')
title = soup.select('#div_content > div.post-title > div.title-subject > div')
contents = soup.select('#div_content > div.post.box > div.post-content > div.post-article.fr-view')
print(title[0].text)
print(contents[0].text)
```
이런 식으로 s에 저장된 세션을 이용해서 새로운 html 값을 받아오고, selector()를 이용해 원하는 부분의 값을 가져온다.

그리고 마지막 부분을 보니, 브라우저를 직접 다뤄 로그인 하듯 하는 방법을 다음에 알려준다고 한다.
