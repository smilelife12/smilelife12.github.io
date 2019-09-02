---
layout: post
title:  "[2017 KAKAO] 1차_추석 트래픽"
date:   2019-08-29
excerpt: "알고리즘 문제풀이"
tag:
- Hyun
- Algorithm
- Python
- wlecomekakao
comments: false
---

## 2017 KAKAO BLIND RECRUITMENT [1차] 추석 트래픽
- - -


### 문제설명
- - -
[문제링크](https://www.welcomekakao.com/learn/courses/30/lessons/17676?language=java)

서버 증설 여부를 결정하기 위해 작년의 로그 데이터를 분석한 후 초당 최대 처리량을 계산하고, 초당 최대 처리량은 요청의 응답 완료 여부와는 상관 없이 임의 시간부터 1초간 처리하는 요청의 최대 개수이다.

입력 형식
- solution 함수에 lines로 배열을 전달하고 N(1<= N <= 2000)개의 로그 문자열이다. 각 로그 문자마다 응답완료시간 S와 처리시간 T가 공백으로 구분
- 응답완료시간 S는 고정길이 `2016-09-15 hh:mm:ss.sss` 형식
- 처리시간 T는 `0.1s`, `0.312s` 등 최대 소수점 셋째 자리까지 기억하고, s로 끝남
- 예시로 `2016-09-15 03:10:33.020 0.011s`는 저 시간부터 시작하여 0.011초 동안 처리된 요청을 말한다.
- 서버는 타임아웃이 3초로 적용, 0.001 <= T <= 3.000
- lines 는 응답완료시간 을 기준으로 오름차순 정렬되어 있다.


### 문제 풀기
- - -
초당 처리량을 구하는 게 목적이고, 각 값들을 str값으로 받아오므로, 이를 기준으로 처리해야한다. 그리고 받아오는 값이 시간순으로 정렬되어 있으므로, 이를 저장하고 처리하는 것이 문제이다.

여기서는 각 초당 처리량을 어떤 기준으로 저장하고, 처리하는지가 가장 큰 문제로 보인다. 계속해서 값은 들어오고, 이 값은 저장되어 있는기간이 모두 다르며, 1초를 기준으로 처리를 해야한다. 결국 구간에 대한 설정을 어떻게 할지를 정해야 한다.

저장형태를 생각해보면, 순서대로 처리되기 때문에 현재시간과 다음시간의 차이를 이용해서 트래픽이 들어있는지를 처리하면 될 것 같다. 그래서 그 값의 차이를 이용해 저장된 시간으로부터 흐른 시간을 확인하고, 처리되었는지 안되었는지를 본다.
정리하자면, 배열에 계속해서 트래픽의 처리시간을 저장하고, 현재시간을 따로 저장하며, 다음 트래픽이 들어오는 시간을 통해 흐른 시간을 설정하고, 배열에서 트래픽의 처리시간을 불러오고, 흐른 시간만큼 값을 뺀 뒤, -값이 나오게 되면 이미 트래픽이 처리되어 빠져나간 것이므로, 배열에서 그 트래픽을 제거한다. 그리고 현재 저장된 배열값이 몇개인지를 계속 max값과 비교하며 최대 트래픽 값을 처리해준다.

처리해야 할 것은 불러온 값에서 날짜는 동일하므로 제외하고, 시간과 처리 시간을 불러온다. 그리고 처리시간 값을 배열에 추가해준다. 값들이 유기적으로 저장되어야 하므로 해당 값은 LinkedList를 사용하거나, 파이썬의 경우 단순 배열을 통해서 정리하면 될 것으로 보인다.

- - -
생각을 잘못한 부분이, 처리를 시작한 시간을 주는 것이 아니고, 처리가 끝난 시간을 주는 것이었다. 이 때문에 계산을 좀 다시해야 할것 같다.

아예 생각을 다르게 해야했다. 그래서 풀이법을 보게되었는데, 전체 시간대를 모두 계산하려면 너무 오랜시간이 걸린다. 그렇기 때문에 처리 수가 변하는 시간, 즉, 각 값의 시작시간의 0.999초 전과 끝난 후 부터 0.999초 후 까지 두번을 체크해서 처리수를 계산해 준다. 각 처리 시간들의 시작시간과 끝나는 시간을 저장하는 값들의 배열을 설정하고, 이 값 전체를 비교하는 것으로 해본다. 그리고 시간에 대한 변화를 줄 때 굳이 시분초를 나눌 필요없이 시,분을 초단위로 계산해서 합쳐서 하는 것이 편했다.


- - -
여기서 주요하게 사용했던것들
```python
String.replace('target','replace')
String.split('target')
round('float 변수','반올림 자리수')

```

### 코드
- - -
<details markdown="1">
<summary>코드내용</summary>
``` python
def solution(lines):
    answer = 1
    s_time = []
    e_time = []
    for t in lines:
        time = t.split(" ")[1]
        done_time = t.split(" ")[2].replace("s", "")
        t_time = cal_time(time)
        e_time += [t_time]
        s_time += [round(t_time-float(done_time)+0.001, 3)]
    for i in range(0, len(s_time)):
        max_s = 0
        max_e = 0
        for j in range(0, len(s_time)):
            if (round(s_time[i]-0.999,3) <= e_time[j])and (s_time[j] <= s_time[i]):
                max_s += 1
            if (e_time[i] <= e_time[j]) and (s_time[j] <= round(e_time[i] + 0.999, 3)):
                max_e += 1
        if max_s > answer:
            answer = max_s
        if max_e > answer:
            answer = max_e
    return answer


def cal_time(time):
    before_h = int(time.split(":")[0])
    before_m = int(time.split(":")[1])
    before_s = float(time.split(":")[2])
    return float(before_h*3600 + before_m*60 + before_s)

```
