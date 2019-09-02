---
layout: post
title:  "[2017 KAKAO] 3차_방금그곡"
date:   2019-08-28
excerpt: "알고리즘 문제풀이"
tag:
- Hyun
- Algorithm
- Java
- wlecomekakao
comments: false
---

## 2017 KAKAO BLIND RECRUITMENT [3차] 방금그곡
- - -


### 문제설명
- - -
[문제링크](https://www.welcomekakao.com/learn/courses/30/lessons/17683)

기억한 멜로디를 가지고 방금그곡이라는 프로그램을 이용해서 노래를 찾는다. 이때, 음악을 반복 재생할 수 있어 네오가 기억하는 멜로디는 음악 끝과 처음이 이어서 재생된 멜로디일 수도 있다. 기억한 멜로디를 재생 시간과 제공된 악보를 직접 보며 비교하려 한다.
가정은 이와 같다.

- 방금그곡 서비스는 음악 제목, 재생 시작과 끝 시각, 악보 제공
- 기억하는 멜로디와 악보에 사용되는 음은 C, C#, D, D#, E, F, F#, G, G#, A, A#, B 이렇게 12개이다.
- 각 음은 1분에 1개씩 재생, 음악은 반드시 처음부터 재생되고, 음악 길이보다 재생 시간이 길경우 끊김 없이 반복 재생된다. 재생 시간이 짧은 경우는, 시작부터 재생 시간만큼만 재생된다.
- 음악이 00:00을 넘겨서 재생되는 일은 없다.
- 조건에 일치하는 음악이 여러개이면, 라이도에서 재생된 시간이 가장 긴 음악 제목을 반환한다. 재생시간이 같은 경우, 먼저 입력된 음악 제목을 반환한다.
- 조건에 맞는 음악이 없을 경우 "\`(None)\`"을 반환한다.

입력은 기억하는 멜로디 문자열 m과 방송곡 정보를 갖는 배열 musicinfos가 주어진다.

- m 은 음 1개 이상 1439개 이하 구성
- musicnfos 100개 이하 곡정보가 있으며 각각 시작시간, 끝난시간, 음악 제목, 악보 정보를 쉼표(,)로 구분된 문자열을 갖는다.

### 문제 풀기
- - -
우선 문제는 같은 문자열을 찾는 것이고, 주요 부분은 주어진 문자열이 반복되는 값을 처리하는 방식이다. 주어지는 음악의 정보에서 노래 멜로디 부분을 따로 저장해서 처리하는 것이 좋아보인다. 그래서 그 멜로디 부분에 대한 값을 저장하는 멜로디를 저장한다.

그리고 탐색을 실행하는데, 여기서 시작부분이 일치하는 값들을 각 info에서 찾아주고, 그 뒷부분도 일치하는지 탐색을 해준다.만약 이 멜로디의 길이를 넘어간 만큼 앞으로 돌아와서 탐색을 실행한다.
그리고 보면서 조금 첨부해야할 부분은 기억하는 멜로디 길이인 m보다 노래의 재생시간이 짧은 경우 굳이 저장을 하지 않아도 된다.

구분을 하는 과정에 있어서, 뒤에 #을 붙이는 경우를 어떤식으로 구분할지에 대해서 정해야한다. 그냥 진행할 경우 #이 붙었는데 안붙은 것과 같은 처리가 되는 경우가 생기게된다.

그리고 추가적으로 구분할 것은, 멜로디에 반복이 있는 것은 저 위의 확인을 진행할 필요가 없다.

문제를 풀고 체점까지 진행했는데, 두가지 경우에서 fail이 나와서 어느부분을 고쳐야 할지를 모르겠어서, 한참 고민했다. 아무래도 플레이 시간에 따라 입력된 음표가 다르기 때문에 그부분을 처리해야할 것 같다.
근데 이를 해결하기 위해서 내가 푼 코드는 뜯어 고쳤야한다. 기본적으로 단순하게 길이를 지나게 되면 다시 앞으로 돌아와서 찾는 방식으로 했기 때문에 시간이 별로 의미가 없었다. 단순히 들은 음의 길이보다 짧으면 종료되도록 했기 때문이다. 하지만 여기서 바라는 것은 `#` 이붙은 값들에 대한 대체를 처리하고, 연속성을 갖는 String을 만드는 것을 원한 것 같다.

그래서 그렇게 바꾸면서 처리해주니 오히려 그 안에 값이 있는지 확인하는 걸 ``String.contains()`` 함수를 이용할 수 있게 되어서 더욱 간단하게 처리할 수 있게 되었다. 토큰의 처리가 이 문제풀기에서 가장 중요한 부분이었던것 같다.
- - -
여기서 주요하게 사용했던것들
```java
String.replace('target','replace');
String.split('target');
String.contains('String')
```

### 코드
- - -
<details markdown="1">
<summary>코드내용</summary>
``` java
import java.util.Scanner;
import java.util.*;

public class kakao2017_3rd_music {
    public static String solution(String m, String[] musicinfos){
        String answer = "(None)";
        int m_len = -1;
        m = m.replace("C#","c").replace("D#","d").replace("F#","f").replace("G#","g").replace("A#","a");
        int count = 0;
        int len = m.length();
        for (String temp : musicinfos){
            String [] arr = temp.split(",");
            String start = arr[0];
            String end = arr[1];
            int time = calTime(start, end);
            if(time < len){
                continue;
            }
            String melody = arr[3];
            melody = melody.replace("C#","c").replace("D#","d").replace("F#","f").replace("G#","g").replace("A#","a");
            String t_m = "";
            for(int i = 0 ; i < time; i++){
                t_m += melody.charAt(i%melody.length());
            }
            if(t_m.contains(m)){
                if(time > m_len){
                    answer = arr[2];
                    m_len = time;
                }
            }
        }
        return answer;
    }
    public static int calTime(String S, String E){
        int S_H = Integer.parseInt(S.split(":")[0]);
        int S_M = Integer.parseInt(S.split(":")[1]);

        int E_H = Integer.parseInt(E.split(":")[0]);
        int E_M = Integer.parseInt(E.split(":")[1]);

        int sum = E_M - S_M;
        if(sum < 0){
            sum += 60;
            E_H -=1;
        }
        sum += (E_H - S_H)*60;

        return sum;
    }
    public static void main(String[] args) {
        String m = "ABC";
        String [] musicinfos = {"12:00,12:14,HELLO,C#DEFGAB", "13:00,13:05,WORLD,ABCDEF"};
        String ans = solution(m,musicinfos);
        System.out.println(ans);
//        System.out.println(calTime("15:40", "16:20"));
    }
}

```
