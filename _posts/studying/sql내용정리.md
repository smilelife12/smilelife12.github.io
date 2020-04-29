# 쿼리문 정리

## join

- db연결해 데이터를 검색하는 방법

1. INNER JOIN
    - 쉽게 정리해 교집합
    - 기준테이블과 Join한 테이블의 중복된 값 보여줌
    - 기준 테이블은 From, 조인하는 테이블은 Inner join 다음 테이블
2. LEFT OUTER JOIN
    - 기준 테이블 값과 대상 테이블의 중복된 값

## \<include refid="id" /\>

- sql문에서 반복되는 조건에 대해서 id로 지정한 값을 그대로 가져오는 것

## WHERE 원하는 컬럼 IN (조건1, 조건2, 조건3, ...)

- 해당 컬럼에 여러 조건들이 포함되는 값을 원할 때 사용

## Mybatis 리턴 값

- Select : Select 해당 결과
- Insert : 1
- Update : Update된 행의 개수 반환(없으면 0)
- Delete : Delete된 행의 수 (없다면 0)

## DSD CODE

- 회사 별로 뽑아내는 특정 코드를 말함
- 이를 이용해 회사별로 정책설정

## CLOB

- 오라클 에서 구조화 되지 않은 데이터를 저장할 때 사용하는 데이터타입
- 그중 NCLOB은 오라클에서 정의되는 National Charter Set을 따르는 객체 타입
- 이 타입의 경우는 데이터 사이즈의 한계를 늘려 사용하다 보니, ORDER BY/ GROUP BY를 사용할 수가 없어서 이를 변환해서 사용

```SQL
SELECT TO_CHAR(DBMS_LOB.SUBSTR(ADDTAG,4000)) "태그명", COUNT(TO_CHAR(DBMS_LOB.SUBSTR(ADDTAG,4000))) "사용횟수"
    FROM FILESYNC_TAG_LOG
    GROUP BY TO_CHAR(DBMS_LOB.SUBSTR(ADDTAG,4000));
```

- 위와 같이 해당 내용을 `TO_CHAR(DBMS_LOB.SUBSTR(컬럼, 4000))` 을 이용해서 CHAR형태로 변환을 시켜서 사용

## Mybatis foreach 구문 사용

- 배열 파라미터를 집어넣기 위해서 사용하는 foreach구문

```sql
<foreach item="item" index="index" collection="list"
    open="(" separator="," close=")">
        #{item}
    </foreach>
```

기본 형태로 where 절에 in 을 쓸 때 주로 사용한다

- collection : 전달받은 인자 값
- item : 전달받은 인자 값 다른 이름으로 대체
- open : 해당 구문이 시작할 때
- close : 해당 구문 종료할 때
- seporator = 한번이상 반복시 반복되는 사이에 해당 값을 넣어줌

## 구문 순서

FROM / WHERE / GROUP BY / HAVING / ORDER BY / LIMI

## ORACLE에서 limit

limit을 oracle에서 지원을 하지 않으므로 사용 불가능

## Join을 이용하면 중복을 세는 것을 할 수 있을듯

Join을 이용해서 자신이 원하는 것들을 중복하게 선택되게 한 후 group by를 통해서 숫자를 셀 수 있는 것으로 보인다.