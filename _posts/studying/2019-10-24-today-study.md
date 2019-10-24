---
layout: post
title:  "오늘 생각나는대로 공부"
date:   2019-10-24
excerpt: "잡다한 공부들"
tag:
- Hyun
- OS
- Network
- Transaction
- SQL
- Garbage Collection
- Security
- AVL Tree
comments: true
---

## 트랜잭션 격리수준
- - -
1. Read Uncommitted(Dirty Read)
  - Commit이나 Rollback 여부에 상관없이 내용이 보여진다.  
  - 모든 현상 발생
  - 작업 중간에 값이 보여지므로 꼬임
2. Read Committed
  - 대부분 기본적으로 사용되는 수준
  - Commit 완료된 데이터만 조회가능
  - commit이 안된 것은 `undo`영역에 있는 기존 값을 참고해 보여줌
3. Repeatable Read
  - 모든 데이터에 shared lock이 걸려 선행 트랜잭션이 사용하고 있으면, 다른 트랜잭션은 데이터 수정 불가능


## 패리티 비트
- - -
오류 생겼는지 확인하기 위해 사용하는 비트로 짝수, 홀수로 나뉜다. 각가 1의 개수로 판단하는 것이다. 보통 좌측 첫번째에 비트를 추가해서 개수를 맞춘다.  


## SQL join
- - -
<참조> [위키백과](https://ko.wikipedia.org/wiki/Join_(SQL\))
1. Join
  - 2개 이상의 테이블을 연결해 데이터 검색
  - 공통된 값을 이용해서 조인

2. 종류
  - INNER JOIN
  - OUTTER JOIN
  - LEFT JOIN
  - RIGHT JOIN

3. 교차 조인
  - `CROSS JOIN`은 곱집합을 반환
```SQL
SELECT *
FROM employee CROSS JOIN department;
```

4. 내부 조인
  - 기본 조인 형식으로 간주
  - 2개의 테이블의 컬럼 값을 결합해 새로운 결과 테이블을 생성
  - 일치되는 결과 열을 찾기 위해 A 테이블의 각 열을 B 테이블의 각 열과 비교하고 조인 구문이 충족되면 일치된 각 열의 컬럼 값은 결과 열로 결합
  - `명시적 조인 표현`으로 JOIN 키워드를 사용
```SQL
SELECT *
FROM employee INNER JOIN department
  ON employee.DepartmentID = department.DepartmentID;
```
  - 암시적 표현은 SELECT 구문의 FROM 절에서 그것들을 분리하는 컴마를 이용해 여러 테이블을 나열해 사용하고, WHERE 절을 이용해 추가적인 필터를 적용
```SQL
SELECT *
FROM employee, department
WHERE employee.DepartmentID = department.DepartmentID;
```
  - 동일 조인
    - 비교자 기반의 조인으로 동등비교만을 사용
    - 위의 두가지가 동일 조인의 예시와 같음
  - 자연 조인
      - 동일 조인의 한 유형으로 조인 구문
      - 모든 컬럼을 비교하며, 동일한 이름을 가진 컬럼의 각 쌍에 대한 단 하나의 컬럼만 포함
      - 위험한 것이므로 비권장

```SQL
SELECT *
FROM employee NATURAL JOIN department;
```

5. 외부조인
  - 왼쪽 외부조인
```SQL
SELECT *
FROM employee LEFT OUTER JOIN department
  ON employee.DepartmentID = department.DepartmentID;
```
  - 오른쪽 외부조인
```SQL
SELECT *
FROM employee RIGHT OUTER JOIN department
  ON employee.DepartmentID = department.DepartmentID;
```
    - 두개의 조인은 비슷하게 작동하고, 왼쪽의 테이블을 기준으로 할지 오른쪽의 테이블을 기준으로 하는지를 설정하고 그 위치를 기준으로 존재하는 컬럼들을 공통된 개체 외에도 기준 위치 테이블에 남은 개체를 추가한다.
  - 완전 외부 조인
```SQL
SELECT *
FROM employee FULL OUTER JOIN department
  ON employee.DepartmentID = department.DepartmentID;
```
    - 위의 조인을 합친 것과 같은 기능을 한다.


## 가비지 컬렉션(Garbage Collection)
- - -
<참조>[위키백과](https://ko.wikipedia.org/wiki/%EC%93%B0%EB%A0%88%EA%B8%B0_%EC%88%98%EC%A7%91_(%EC%BB%B4%ED%93%A8%ED%84%B0_%EA%B3%BC%ED%95%99))
- 메모리 관리 기법 중 하나로 동적으로 할당했던 메모리 영역중 필요 없는 역역을 해제하는 기능
- 자바, C 등에서 사용
- 장점은 다음 버그들을 막음
  - 유효하지 않은 포인터 접근 : 해제된 메모리에 접근하는 버그를 가르킨다.
  - 이중 해제 : 이미 해제된 메모리를 또다시 해제하는 버그
  - 메모리 누수 : 더이상 필요하지 않은 메모리가 해제되지 않고 남아있는 버그
- 단점
  - 어떤 메모리 해제할지 결정하는데 비용이 든다.
  - 일어날 타이밍 혹은 점유 시간을 미리 예측하기 어렵다.
  - 할당된 메모리가 해제되는 시점을 알 수 없다.  


## 해킹
- - -
1. Dos 공격
  - 수용 능력 이상의 정보나 용량을 초과시켜서 정상적으로 작동하지 못하게 하는 공격
2. 스푸핑(Spoofing)
  - 속이다 라는 의미로 IP 주소, 호스트 이름, MAC 주소 등 여러가지를 속일 수 있고 이를 이용해 공격
  - 정보를 얻거나 시스템 마비를 하는 공격
3. 세션 하이재킹 공격
  - 인증 받아 세션을 생성해 유지하는 연결을 빼앗는 공격
4. 스니핑
  - 데이터 속에서 정보를 찾아내는 것으로 막는 것이 어려움

## 네트워크 보안
- - -
1. 방화벽
  - 네트워크의 외부와 내부 사이를 지나는 패킷을 정해 놓은 규칙에 따라 차단이나 보내는 기능
  - 주요 기능
    - 접근제어, 로깅, 감사추적, 인증, 데이터 암호화
  - 스크리닝 라우터
    - 3계층 네트워크와 4계층 전송에서 실행되며, IP주소와 포트에 대한 접근제어 가능
    - 외부와 내부 네트워크의 경계선
    - 방화벽 역할을 수행
  - 단일 홈 게이트웨이
    - 강력한 보안 정책 실행
    - 방화벽 손상되면 무조건적인 접속 허용
    - 원격 로그인 정보가 노출되어 방화벽에 대한 제어권을 공격자가 얻게되면 내부 네트워크 보호 불가
  - 이중 홈 게이트웨이
    - 효율적인 트래픽 관리
    - 우회 불가
  - 스크린된 호스트 게이트웨이
    - 단일 홈
      - 스크리닝 라우터와 단일 홈 게이트웨이 합친 것
      - 융통성이 좋음
      - 구축비용이 많이 듬
    - 이중 홈
      - 안정성을 더 높임
      - 3계층과 4계층에 대한 접근 제어
  - 스크린된 서브넷 게이트웨이
    - 단일 홈
      - 외부와 내부 사이에 DMZ를 놓음
      - 다른 방화벽의 모든 장점을 갖고 융통성 뛰어남
      - 설치 어렵고, 가격이 비쌈
    - 이중홈
      - 좀 더 빠르며, 강력한 보안
  - 패킷 필터링
    - 허용할 서비스를 확인
    - 보안 문제점과 허용에 대한 타당성 검토
    - 서비스 형태 확인후 규칙 결정 후 방화벽에 실제 적용
  - NAT
    - 공인 주소 부족문제 때문에 개발된 기술
    - 내부에서는 사설 주소를 소유, 외부로 접근시 라우팅 가능한 외부 공인주소를 할당 받음


## AVL Tree
- - -
<참조> [https://ratsgo.github.io/data%20structure&algorithm/2017/10/27/avltree/](https://ratsgo.github.io/data%20structure&algorithm/2017/10/27/avltree/)

이진 트리 중 서브트리의 높이를 제어해 전체 트리가 균형있게 만들어진 트리이다. 이진탐색트리의 계산 복잡성을 줄이기 위해 만들어졌다.  

핵심 개념은 `Balance Factor`이고, 이 값은 왼쪽 서브트리의 높이에서 오른쪽 서브트리의 높이를 뺀 것이다. 이 값이 -2 이하 혹은 2 이상인 경우 rotation을 해준다.

**single rotation**
삽입 연산시 루트노드가 2가 되어버리기 때문에 왼쪽 자식노드의 오른쪽 자식노드를 루트노드의 오른쪽 자식노드의 왼쪽 자식노드로 옮겨주고 왼쪽 자식노드를 루트로 올려주는 방식을 취한다.
![single rotation]({{src.url}}/assets/img/posting1024/img1.png)

이는 반대 쪽에 추가했을 때는 반대로 돌려주며 이를 left/right rotation 이라 한다.
![single rotation]({{src.url}}/assets/img/posting1024/img2.gif)

**double rotation**  
한번의 rotation으로 결과가 나오지 않는 경우 수행한다. 이는 결국 single rotation을 두번 해주는 것이고 상황에 맞게 사용해야 한다. 결국 상황의 기준은 추가되는 노드의 부모 트리를 어느 방향으로 하느냐에 달려있다.

**시나리오별 rotation**  
총 4개의 시나리오를 정리해보면
![시나리오]({{src.url}}/assets/img/posting1024/img3.png)  

- 시나리오1 : U의 왼쪽 자식노드의 왼쪽 서브트리 A에 새 노드 삽입 : single right rotation
- 시나리오2 : U의 왼쪽 자식노드의 오른쪽 서브트리 B에 새 노드 삽입 : double rotation(left-right)
- 시나리오3 : U의 오른쪽 자식노드의 왼쪽 서브트리 C에 새 노드 삽입 : double rotation(right-left)
- 시나리오4 : U의 오른쪽 자식노드의 오른쪽 서브트리 D에 새 노드 삽입 : single left rotation


## 수식 표기법
- - -
- 중위 표기를 후위 표기로
  1. 연산 우선순위에 따라 괄호로 묶는다.
  2. 연산자를 해당 괄호의 뒤로 옮긴다.
  3. 괄호를 제거한다.
- 중위 표기를 전위 표기로
  1. 연산 우선순위에 따라 괄호로 묶는다.
  2. 연산자를 해당 괄호의 앞으로 옮긴다.
  3. 괄호를 제거한다.
