# Java 함수들 정리

## StringUtils.isBlank(CharSequence cs)

- 해당 값이 whitespace<" ">, 공백<"">, null 인 경우 true

## String.trim()

- 스트링값 공백제거

## Assert

- jUnit에서 제공하는 단위 테스트 클래스

## Locale 클래스

- 각 지역의 언어, 나라 정보를 담은 클래스
- 언어코드와 나라코드를 통해서 인스턴스화 시킬 수 있음

## 정규식

- 정규식으로 값을 넣을 때는 `java.util.regex.Matcher`와 `java.util.regex.Pattern`을 이용한다.
- `Pattern`을 이용해서 정규식으로 지정한 값을 집어넣어주고, 이를 `Matcher`를 통해서 값을 확인해준다.

| 표현식 | 설명 |
|:---------:|:-----|
| ^ | 문자열의 시작 |
| $ | 문자열의 종료 |
| . | 임의의 한문자(문자종류 안가림), 단, \는 넣을수 없음 |
| * | 앞 문자가 없을 수도 무한정 많을 수도 있음 |
| + | 앞 문자가 하나 이상 |
| ? | 앞 문자가 없거나 하나있음 |
| [ ] | 문자의 집합이나 범위, 두 문자 사이는 - 기호로 범위를 나타내며 [ ] 내에서 ^가 선행하여 존재하면 not을 나타냄 |
| { } | 횟수 또는 범위를 나타냄 |
| ( ) | 소괄호 안의 문자를 하나의 문자로 인식 | 
| \| |  패턴 안에서 or연산 수행시 사용 |
| \s | 공백 문자 |
| \S | 공백 문자가 아닌 나머지 문자 |
| \w | 알파벳이나 숫자 |
| \W | 알파벳이나 숫자를 제외한 문자 |
| \d | 숫자 [0-9]와 동일 |
| \D | 숫자를 제외한 모든 문자 |
| \ | 정규표현식 역슬래시는 확장 문자로, 일반 문자가 오면 특수문자, 특스문자가 오면 그 문자 자체를 의미 |
| (?i) | 앞 부분에 이 옵션을 넣으면 대소문자를 구분하지 않음 |

- 날짜 포맷 별 정규식

```java
public static final String YYYYMMDD = "(19|20)\\d{2}(0[1-9]|1[012])(0[1-9]|[12][0-9]|3[01])";
public static final String YYYYMMDDHH = "(19|20)\\d{2}(0[1-9]|1[012])(0[1-9]|[12][0-9]|3[01])(0[0-9]|1[0-9]|2[0-3])";
public static final String YYYYMMDDHHMI = "(19|20)\\d{2}(0[1-9]|1[012])(0[1-9]|[12][0-9]|3[01])(0[0-9]|1[0-9]|2[0-3])([0-5][0-9])";
public static final String YYYYMMDDHHMISS = "(19|20)\\d{2}(0[1-9]|1[012])(0[1-9]|[12][0-9]|3[01])(0[0-9]|1[0-9]|2[0-3])([0-5][0-9])([0-5][0-9])";
public static final String HH= "(0[0-9]|1[0-9]|2[0-3])";
public static final String MMDD = "(0[1-9]|1[012])(0[1-9]|[12][0-9]|3[01])";
public static final String HHMI = "(0[0-9]|1[0-9]|2[0-3])([0-5][0-9])";
```

## Calender 클래스

- 날짜와 시간을 손쉽게 처리하기 위해 제공하는 추상 클래스
- 보통 getInstance()를 이용해서 현재 시간을 받고 해당 시간을 다양한 메소드를 이용해서 처리하는 방식.

## 형태소 분석기(<https://shineware.tistory.com/entry/KOMORAN-ver-20-beta-%EC%9E%90%EB%B0%94-%ED%95%9C%EA%B5%AD%EC%96%B4-%ED%98%95%ED%83%9C%EC%86%8C-%EB%B6%84%EC%84%9D%EA%B8%B0>, <https://docs.komoran.kr/>)

- KORMAN 형태소 분석기 2.0을 이용해서 형태소 분석을 진행해 보았다.
- 이를 이용해서 분석을 하게 되면 전체 내용을 분석해 주게되고 first와 second를 내누어서 각 단어를 정리해준다.
- first에는 입력한 값에 대한 내용이 나타나고, second에는 어떤 품사를 갖는지에 대한 내용이 나온다.
- N으로 시작하는 것이 체언(명사, 대명사)
- V로 시작하는 것이 용언(동사, 형용사, 보조용언, 지정사)
- M으로 시작하는 것이 수식언(관형사, 부사)
- I로 시작하는 것이 독립언(감탄사)
- J로 시작하는 것이 관계언(격조사, 보조사, 접속조사)
- E로 시작하는 것이 어미
- X로 시작하는 것이 접두 접미사 어근
- S로 시작하는 것은 부호
    1. SF - 마침표, 물음표, 느낌표
    2. SP - 쉼표, 가운뎃점, 콜론, 빗금
    3. SE - 줄임표
    4. SO - 붙임표
    5. SL - 외국어
    5. SH - 한자
    6. SN - 숫자
    7. SW - 기타기호
    8. N으로 시작하는 추정범주들

### KOMORAN 사용법

[komoran 사이트 v2.0](https://shineware.tistory.com/entry/KOMORAN-ver-20-beta-%EC%9E%90%EB%B0%94-%ED%95%9C%EA%B5%AD%EC%96%B4-%ED%98%95%ED%83%9C%EC%86%8C-%EB%B6%84%EC%84%9D%EA%B8%B0)에서 jar파일로 되어있는 라이브러리들과, data file인 models.zip을 다운받은 뒤에 해당 jar파일 들은 build path에 external jar파일로 추가를 해주고 모델은 추후에 사용된다.

우선적으로 model dictionary를 주소로 받는 생성자를 만들어야하고, 이 부분은 앞서 다운로드 받은 model을 경로로 지정해준다. 

```java
Komoran komoran = new Komoran(Path);
```

와 같은 형태로 작성하면 된다. 그리고 여기서 `analyze()`함수를 이용하면 형태소 분석 결과를 리턴해준다. 해당 리턴 타입은 `List\<List\<[TOKEN]\>\>` 형태로 나타나며, 여기서 `TOKEN`값은 `Pair\<String, String\>`으로 나타나게 된다. 그래서 예시로는

```java
String s = "아버지가방에들어가셨다.";
Komoran komoran = new Komoran("D:\\komoran\\models-light");
List<List<Pair<String, String>>> result = komoran.analyze(s);
List<String> wordList = new ArrayList<String>();
for(List<Pair<String, String>> ejeolResult : result){
    for(Pair<String, String> wordMorph : ejeolResult){
        String pos = wordMorph.getSecond();
        String word = wordMorph.getFirst();
        System.out.println("first : " + word+" second : " + pos}
    }
}
```

와 같이 사용하게 되면 

```txt
first : 아버지 second : NNG
first : 가방 second : NNG
first : 에 second : JKB
first : 들어가 second : VV
first : 시 second : EP
first : 었 second : EP
first : 다 second : EF
first : . second : SF
```

와 같은 형태로 나타나며 해당 second에서 나타나는 형태소는 21세기 세종계획의 품사 기준을 따른다. (<https://docs.komoran.kr/firststep/postypes.html>)

3.0 버전에서는 Pair가 Token으로 바뀌었는데 이 3.0의 경우는 1.8버전 이상을 호환하므로 해당 내용은 추후에 적용하여 사용

### 꼬꼬마프로젝트 형태소 분석기

[꼬꼬마 프로젝트 홈페이지](http://kkma.snu.ac.kr/documents/index.jsp)에서 해당문서를 확인할 수 있다.

#### 내려받기 및 사용

다운로드 페이지에서 jar파일을 다운을 받고 해당 파일을 build path에 적용하는 것으로 적용을 완료해준다.

#### 형태소 품사

형태소 품사의 경우 조금 다르지만 `N`으로 시작하는 것을 체언으로 사용하는 것과 대부분의 내용은 비슷하다. 여기서 숫자나 외국어의 경우가 `OL`, `ON`으로 전환 된 것을 볼 수 있다.

#### 예제

사용 예제의 경우는 앞서 사용한 komoran과는 조금 다르다

```java
String s = "아버지가방에들어가셨다.";
MorphemeAnalyzer ma = new MorphemeAnalyzer();
ma.createLogger(null);
List ret = ma.analyze(s);
ret = ma.postProcess(ret);
ret = ma.leaveJstBest(ret);
List stl = ma.divideToSentences(ret);
for(int i = 0 ; i < stl.size(); i++){
    Setence st = stl.get(i);
    System.out.println("===> " st.getSentence());
    for(int j = 0 ; j< st.size(); j++){
        System.out.println(st.get(j));
    }
}
ma.closeLogger()
```

위와 같이 사용하며 `MorphemeAnalyzer()`를 생성하여 해당 형태소 분석기를 로거로 실행하고, 이를 이용해 측정을해준다.
그러면 결과는
```txt
아버지가방에	=> [0/아버지/NNG+3/가방/NNG+5/에/JKM]
들어가셨다.		=> [6/들어가/VV+9/시/EPH+10/었/EPT+10/다/EFN+11/./SF]
```

여기서 이유는 정확히 모르지만 `closeLogger()`를 사용하면 오류가 난다. 안써주면 로그가 계속 돌아가고 있는 정도의 현상이 발생하는 것으로 보인다.

위에 이어서 키값을 추출 하는 방법은 

```java
String title = "아버지가방에들어가셨다.";
KeywordExtractor ke = new KeywordExtractor();
KeywordList kl = ke.extractKeyword(title, false);
for(Keyword k : kl){
    System.out.println("get string : " + k.getString() + "\tcnt : " + k.getCnt());
}
```

위와 같이 사용하게 되면 앞서 나오는 내용과 달리 분류된 값에 대한 단어 값들만 보여주게 된다.

```txt
get string : 아버지	cnt : 1
get string : 아버지가방	cnt : 1
get string : 가방	cnt : 1
get string : 에	cnt : 1
get string : 들어가	cnt : 1
get string : 시	cnt : 1
get string : 었	cnt : 1
get string : 다	cnt : 1
get string : .	cnt : 1
```

와 같은 내용으로 나타나게 된다. 명사만 나올 수도 있게 할 수 있는데 `extractKeyword(title, true)` 로 바꿔주면 명사만 나타나게 된다.

### OKT 형태소 분석기(Open Korean Text)

해당 부분을 적용을하려 하는데 자바 버전을 1.8이상을 호환하는 것으로 보이며 이 부분에서 문제가 생겨 진행이 되질 않는다. 해당 현재 버전이 2.3인데 이를 1.2버전으로 받아서 실행해 보았지만 제대로 실행되지 않음

### 은전한닢 형태소 분석기(seunjeon)

java 기반의 형태소분석기중 하나로 일본어에 사용되던 형태소 분석기를 개량해서 만들어진 형태소분석기이다.

#### 설치

<https://jar-download.com/?search_box=seunjeon> 에서 은전한닢 jar 파일을 다운받는데 1.3.1 까지가 자바 1.7을 호환하므로 1.3.1을 다운받아서 사용

#### 오류

1.8 버전으로 호환을 주로 하여서 1.7버전으로 바꿔서 사용해 보려 했지만 input length 라던가 해당 값들에 대한 적용 방법들이 다른 것으로 보인다.

### 아리랑 형태소 분석기(<https://yujoonote.tistory.com/27>)

위 사이트에서 사용하는 대로 한번 적용을 해 보았으며, 해당 아리랑 형태소 분석기를 사용해 보았다.

#### 설치

<https://cafe.naver.com/korlucene> 에서 5.0 버전으로 설치하였으며 jar파일을 이용하였다.

#### 사용방안

아리랑 형태소 분석기의 예제로는 

```java
MorphAnalyzer ma = new MorphAnalyzer();
StringBuilder res = new StringBuilder();
ArrayList<String> noun = new ArrayList<String>();

StringTokenizer stok = new StringTokenizer(title, " ");
while(stok.hasMoreTokens()){
    String token = stok.nextToken();
    
    try {
        List<AnalysisOutput> outList = ma.analyze(token);
        for(AnalysisOutput o : outList){
            res.append(o).append(" ");
            if(o.getPos() == 'N'){
                noun.add(o.getStem());
            }
        }
    } catch (MorphException e) {
        // TODO Auto-generated catch block
        e.printStackTrace();
    }
}
System.out.println(res.toString());
System.out.println(noun);
```

이런 식으로 사용이 가능하며 token화 시킨 값을 명사인지 아닌지를 체크를 하는 것으로 보이는데 해당 내용에 대해서 토큰화 시킨 값에대해 판별한다
해당 내용은 띄워쓰기를 통해서 구분한 것인데, 이를 구분하는데 있어서 앞서 사용했었던 정규식으로 적용하면 나아질 수 있을 것 같다.

그래서 split에 들어가는 정규식을 `[^0-9a-zA-Z가-힣\\.]` 를 통해서 영어, 한글, 숫자, `.`이 아닌경우 나누는 것으로 했다.

한글의 경우는 굉장히 형태소 분석이 준수하게 잘 되는 것으로 보이고, 외국어나 숫자가 나오는 경우 명사로 인식해서 처리되어서 괜찮은 분석기로 보인다.

단점은 숫자나 영어가 함께 있으면 그대로 명사로 인식해버려서 그대로 적용이 되어버리는 경우이다.

### 형태소 분석기 비교(<http://konlpy.org/ko/latest/morph/#comparison-between-pos-tagging-classes>)

KoNLPy 기준이기는 하지만, 각각의 비교 내용이 기록

해당 내용을 봤을 때는 꼬꼬마 분석기가 제일 적합해 보일 수 있으나, 영어와 관련된 처리하는 것이 힘들어 보인다. 명사를 뽑아내는 색인어 추출기를 통과하게 되면, 해당 영어 들이 제거되어 나타나기 때문에 직접 값을 입력해야한다.

여기서 komoran이 그대로 `아버지가방에들어가신다` 라고 해석한다고 나왔지만 실제로는 `아버지` `가방` `에` `들어가` `시` `ㄴ다` `.` 으로 구분이 된다.

### list 안에 해당 string 포함 되어있는지 확인

```java
String [] list ={"aa", "bb", "cc"};
if(ArrayUtils.contains(list,"aa")){
    System.out.println("contain aa");
}
```

위와 같이 `ArrayUtils(list, word)` 형태로 진행하면 해당 단어가 리스트 안에 포함했는 지 판별

### String 빈 값 확인

```java
String nu = ""
if(!TextUtils.isEmpty(nu)){
    System.out.println("is not empty String");
}
```

위와 같이 `TextUtils.isEmpty(String)` 메소드를 이용하면 해당 값의 널체크를 진행