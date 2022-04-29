---
title: "오라클 설치"
categories :
  - oracle
tag :
  - oracle
  - sql
author : "Oh seungeun"
date : 2022-04-29 21:35:00
---


# 오라클 설치

### 오라클 설치

- 오라클 데이터베이스 다운로드 (19c 버전)

: [https://www.oracle.com/kr/database/technologies/oracle19c-windows-downloads.html](https://www.oracle.com/kr/database/technologies/oracle19c-windows-downloads.html)

- 다운받은 오라클 데이터베이스 파일 (WINDOWS.X64_193000_db_home) 압축을 푼다
- 압축 푼 파일 → data → setup.exe 우클릭 → 관리자 권한으로 실행후 다음과 같이 선택해준다
    - 오라클 SQL과 PL/SQL을 다루는 기술 교재 20-22p 참고

![](/images/oracle/Untitled.png)

![](/images/oracle/Untitled 1.png)

![](/images/oracle/Untitled 2.png)

→ 전역 데이터 베이스 이름 : myoracle, 비번 : 1234, ‘컨테이너 데이터베이스로 생성’ 옵션 해제

- 설치하려는데 다음과 같은 오류 발생 - 일단 창 닫으니 설치가 진행되고 완료가 되었음

![](/images/oracle/Untitled 3.png)

![](/images/oracle/Untitled 4.png)

- 설치가 끝났으면 SQL Plus로 들어간다 (관리자 권한으로 실행)
- 테이블스페이스를 생성해준다 (oradata-MYORACLE이 있는 경로를 확인한다 → 나의 경우sql_lecture에 있음)

```jsx
CREATE TABLESPACE myts DATAFILE 'C:\sql_lecture\oradata\MYORACLE\myts.dbf' SIZE 100M AUTOEXTEND ON NEXT 5M;
```

- 그 다음 사용자를 생성한다 (여기서 ‘ohse’는 내 이름의 이니셜이다)

```jsx
CREATE USER ora_user IDENTIFIED BY ohse DEFAULT TABLESPACE MYTS TEMPORARY TABLESPACE TEMP;
```

- 롤을 부여한다

```jsx
GRANT DBA TO ora_user;
```

- 사용자 계정의 로 DB에 접속하기
    - 연결을 먼저 해준다

```jsx
connect ora_user/ohse;
```

- 접속후 ‘select user from dual;’ 을 입력하면 현재 로그인한 사용자 이름이 출력된다
    - 여기까지 완료되면 테이블 스페이스와 사용자가 제대로 생성된 것

---

### SQL Developer 설치

      : [https://www.oracle.com/tools/downloads/sqldev-downloads.html](https://www.oracle.com/tools/downloads/sqldev-downloads.html)

- 설치 후 압축을 푼다
- 파일로 들어가 sqldeveloper 응용프로그램을 관리자 권한으로 실행
- 파일 → 새 파일 만들기 → 아래와 같이 입력 후 test 시행
    - 이 때 사용자 이름은 위에서 IDENTIFIED에 입력한 ora_user를 입력하고 비밀번호는 1234가 아니라 ohse를 입력하니까 실행되었음

![](/images/oracle/Untitled 5.png)

- 테스트가 성공했으면 <접속>을 클릭 (혹은 <접속> 버튼이 없으면<저장> 후 접속하면 됨)
- 도구 → 환경설정 → 시간 기록 형식 (P)에 YYYY/MM/DD HH24:MI:SS로 바꾼다
- 질의 입력창에 SELECT user from DUAL;을 실행하고 ora_user가 뜨는지 확인해본다
- c드라이브에 backup 폴더를 생성하고 expall.dmp와 expcust.dmp 파일을 넣어준다

      파일 다운로드 링크 : [https://github.com/gilbutITbook/006696/blob/master/01장 환경설정/](https://github.com/gilbutITbook/006696/tree/master/01%EC%9E%A5%20%ED%99%98%EA%B2%BD%EC%84%A4%EC%A0%95)

- cmd(명령어 프롬프트)를 열고 경로를 backup 폴더로 잡아준다(cd 이용)
- 아래 코드 입력
    - imp ora_user/ohse file=expall.dmp log=empall.log ignore=y grants=y rows=y indexes=y full=y
    - imp ora_user/ohse file=expcust.dmp log=expcust.log ignore=y grants=y rows=y indexes=y full=y
- SQL Developer 질의 입력 창에 ‘SELECT table_name FROM user_tables;’ 입력후 정상적으로 실행되는 지 확인

![](/images/oracle/Untitled 6.png)

---

### 오라클 삭제

- 설치과정에 오류가 발생했을 때, 제일 좋은 해결책은 삭제하고 다시 설치하는 것!
- 우선 오라클 서비스를 중시 시킨다
    - [제어판] → [시스템 및 보안] →[관리도구] → [서비스] 메뉴를 실행해 Oracle로 시작되는 모든 서비스를 중지시킨다
- 삭제 파일시동하기
    - 오라클 메인 폴더로 가서 (나의 경우 c드라이브-sql_lecture) [deinstall.bat] 파일을 관리자 권한으로 실행한다
- 삭제 진행하기
    - Oracle 홈에 구성된 데이터베이스 이름목록을 지정하라고 나오는데 [MYORACLE]을 입력하고 엔터를 누른다
- 삭제 확인하기
    - 오라클 메인 폴더로 가서 파일이 남아 있는지 확인하고, 남아있다면 삭제한다