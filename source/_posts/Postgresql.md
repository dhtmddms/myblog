---
title: "Postgresql 설치"
categories :
  - setting
tag :
  - postgresql
author : "Oh seungeun"
date : 2022-05-04 14:23:00
---


- 기본적으로 업데이트와 업그레이드를 진행해준다

```jsx
$ sudo apt-get update
$ sudo apt-get upgrade
```

- WSL2에서 PostgreSQL을 설치한다
- 설치가 끝나면 항상 버전을 확인해준다

```jsx
$ sudo apt install postgresql postgresql-contrib
$ psql --version
```

- Database를 접근가능하도록 활성화해준다

```jsx
$ sudo service postgresql status
```

- Postgresql을 시작한다

```jsx
$ sudo service postgresql start
```

- 활성화되었는지를 확인해본다

```jsx
$ sudo service postgresql status
```

- 데이터베이스를 종료시킨다

```jsx
$ sudo service postgresql stop
```

- 사용자 계정 password를 설정해준다

```jsx
$ sudo passwd postgres

# 이 때 비밀번호 입력하는 란이 뜨는 데 웬만하면 1234 로 한다
```

- 처음이라면 pdAdmin pw를 만들어준다
- 우선 Service가 활성화를 시켜줘야 한다

```jsx
$ sudo service postgresql start
```

- 지정할 패스워드 입력 (postgres로 설정해준다)

```jsx
$ sudo -u postgres psql -c "ALTER USER postgres PASSWORD 'postgres';"
```

- 서버 생성 (General Tab에는 test 라고 입력/connection tab에는 host name을 127.0.0.1, pw는 postgres입력)
- DB 생성 (database 우클릭 → create → 새로운 데이터 베이스 명을 dataengineering으로 입력
- dataengineering → skema → table → 우클릭 create → general tab의 name: users 입력 →columns은 아래와 같이 생성 후 save 로 저장

![Untitled](/images/Postgresql/Untitled.png)

- 이번에는 psql에 접속 후 dataengineering 테이블이 조회되는지 확인

```jsx
$ sudo -u postgres psql
postgres=# \l
```

- dataengineering DB에 연결 후, 테이블을 조회하도록 한다.

```jsx
postgres=# \c dataengineering
dataengineering=# \dt
```

- 더이상 안 쓸시에 멈추는 명령어 : sudo service postgresql stop으로 종료해줌 !! → 과부화 및 과금 방지
- 가상환경 활성화 코드

```jsx
$ source venv/bin/activate
```

- 참고 : [https://dschloe.github.io/sql/postgreslq_wsl2/](https://dschloe.github.io/sql/postgreslq_wsl2/)