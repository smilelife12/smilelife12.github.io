---
layout: post
title:  "[개인 프로젝트] 공부 - 웹 크롤링"
date:   2019-09-26
excerpt: "나만의 웹 크롤러 만들기, selnium"
tag:
- Hyun
- Web-Crawling
- Python
- Selenium
- Project
comments: true
---

## 웹크롤링 공부
- - -
조금 긴 시간동안 웹크롤링을 못했는데, 다시 시작하려 한다.
[나만의 웹 크롤러 만들기](https://beomi.github.io/gb-crawling/) 에서 웹크롤링을 시도했었고, 이제 selenium 라이브러리를 이용하고, 추가적으로 Chrome WebDriver를 이용해 접근할 예정이다.

## Selenium으로 무적 크롤러 만들기
- - -

우선 selenium설치를 위해서
```shell
conda install -c conda-forge selenium
```
를 이용해 설치를 한다.

그리고 잠시 Selenium이 무엇인지 알아보자.
**Selenium이란?**
- - -
주로 웹 앱을 테스트하는데 이용하는 프레임워크다. `webdriver`라는 API를 통해 운영체제에 설치된 브라우저를 제어한다. 브라우저를 직접 동작시킨다는 것은 JS를 통해 비동기적 혹은 뒤늦게 불러와지는 컨텐츠들을 가져올 수 있다는 것이다
- - -

다음으로 Selenium의 `webdriver`를 이용하여 디바이스에 설치된 브라우저들을 제어하는데, Chrome을 활용하기 위해서 Chrome WebDriver를 설치한다. [Chrome WebDriver 설치](https://sites.google.com/a/chromium.org/chromedriver/downloads)에서 버전에 맞게 설치한다.


#### Selenium으로 사이트 브라우징
- - -

Selenium은 `webdirver api`를 통해 브라우저 제어한다.
그래서 제어를 위해 webdriver를 적용시키면.
```python
from selenium import webdriver

driver = webdriver.Chrome('C:/Users/file_path/chromedriver')

driver.implicitly_wait(3)

driver.get('https://google.com')
```
위와 같이 사용할 수 있다. driver에 ChromeDriver의 설치 위치를 적용해서 `webdriver.Chrome('파일위치')`로 작성한다. 그리고 나서 모든 자원이 로드 되는걸 기다리기 위해 `driver.implicitly_wait(3)`을 이용해 3초를기다리도록 한다. 그리고 특정 url에 접근하기 위해 `dirver.get('url')`을 작성해서 접근한다.

여기서 Selenium은 다양한 메소드를 제공하는데
- URL에 접근하는 api
  - get('url')
- 페이지의 단일 element에 접근하는 api
  - find_element_by_name('HTML_name')
  - find_element_by_id('HTML_id')
  - find_element_by_xpath('/html/body/some/xpath')
- 페이지의 여러 elements에 접근하는 api
  - find_element_by_css_selector('#css > div.selector')
  - find_element_by_class_name('some_class_name')
  - find_element_by_tag_name('h1')

등이 있다. 위 메소드 사용시 굳이 Python과 BeautifulSoup을 사용하지 않아도 된다.


#### 네이버 로그인 하기
- - -
네이버는 request를 이용해 로그인하는 건 힘들다. 프론트 단에서 JS를 처리하며 로그인하기 때문이다. 하지만, Selenium을 이용하면 쉽게 로그인을 할 수 있다.


```python
from selenium import webdriver

driver = webdriver.Chrome('/Users/beomi/Downloads/chromedriver')
driver.implicitly_wait(3)
## url에 접근한다.
driver.get('https://nid.naver.com/nidlogin.login')
```

이를 통해서 로그인 창의 정보를 얻으면 아이디의 name은 `id`, 비밀번호의 name은 `pw`인 것을 알 수 있다.

그래서 이를 이용하기 위해 추가적으로
```python
driver.find_element_by_name('id').send_keys('naver_id')
driver.find_element_by_name('pw').send_keys('mypassword1234')
```
를 이용해준다. 그러면 자동으로 값이 입력이 되는 것을 볼 수 있다.(신기했다)

그 다음으로 이 값을 입력하고 로그인을 클릭하기 위해 버튼을 누르는 갓을 실행한다.
```python
driver.find_element_by_xpath('//*[@id="frmNIDLogin"]/fieldset/input').click()
```
(이부분은 정확히 어떻게 얻어왔는지는 모르겠다.)

그대로 진행해 보면 자동입력방지를 위한 창이 뜨기 때문에, 똑같이 진행하긴 어려워 보인다.

역시, 네이버는 철저하다. 그리고 나서 로그인이 됬다는 가정하에, 로그인 시에만 보이는 페이지를 들어가는 방식으로 한다.

이제 예를 들어 네이버 페이의 url인 `https://order.pay.naver.com/home` 에 접근한다. 그래서 페이지 소스를 불러와 BeautifulSoup를 활용해 보자.

```python
from selenium import webdriver
from bs4 import BeautifulSoup

driver = webdriver.Chrome('/Users/beomi/Downloads/chromedriver')
driver.implicitly_wait(3)
driver.get('https://nid.naver.com/nidlogin.login')
driver.find_element_by_name('id').send_keys('naver_id')
driver.find_element_by_name('pw').send_keys('mypassword1234')
driver.find_element_by_xpath('//*[@id="frmNIDLogin"]/fieldset/input').click()

# Naver 페이 들어가기
driver.get('https://order.pay.naver.com/home')
html = driver.page_source
soup = BeautifulSoup(html, 'html.parser')
notices = soup.select('div.p_inr > div.p_info > a > span')

for n in notices:
    print(n.text.strip())
```

`driver.page_source`를 이용해서 페이지 소스값을 얻어와 html에 저장하고 soup에 html값을 BeautifulSoup를 통해 parser를 적용해 값을 정제한다.

Selenium을 통해 한단계한단계씩 접근하는 BeautifulSoup와는 달리 렌더링 후 DOM결과물에 접근이 가능한 것이다. 함께 사용을 통해 좀더 간결한 코드를 만들 수도 있다.


## Django로 크롤링한 데이터 저장하기
- - -

웹에서 얻어온 데이터들을 체계적으로 관리하려면 DB가 필요하고, 이 DB를 만들고 관리하는 웹프레임워크인 `django`의 Database ORM을 이용해 DB를 만들고 데이터를 저장해 보려 한다.

크롤링을 통해 나온 결과물을 Django ORM으로 Sqlite DB에 저장해 보는 것까지 다룬다.
- - -
여기서 잠시 멈추고 Django를 공부하러 가보자.
