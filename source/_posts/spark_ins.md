---
title: "스파크 설치"
categories :
  - settings
tag :
  - spark
author : "Oh seungeun"
date : 2022-05-03 09:25:00
---


- 우선 자바를 먼저 설치한다
    
    설치 링크 : 아래에서 window installer 64bit를 설치한다
    
    **[Java SE 8 Archive Downloads (JDK 8u211 and later)](https://www.oracle.com/java/technologies/javase/javase8u211-later-archive-downloads.html)**
    
- 다운로드 후 파일을 관리자 권한으로 실행한다
- next 버튼 클릭 후, 아래 화면이 나오면 경로를 변경해준다 ( 경로에 공백이 있으면 나중에 문제 됨)
- change 클릭 → c드라이브 → 파일명 jdk로 바꾼다

![](/images/spark_ins/.png)

![](/images/spark_ins/%201.png)

- ok 클릭 후 자바의 런타임 환경 폴더도 경로를 바꿔준다
- c 드라이브에 새폴더 만들기 클릭 → 파일명  jre 로 지정해주면 끝
- 다음은 스파크를 설치한다
    
    설치 링크 : **[https://www.apache.org/dyn/closer.lua/spark/spark-3.2.0/spark-3.2.0-bin-hadoop3.2.tgz](https://www.apache.org/dyn/closer.lua/spark/spark-3.2.0/spark-3.2.0-bin-hadoop3.2.tgz)**
    

- HTTP 아래 링크 클릭하여 다운로드

![](/images/spark_ins/%202.png)

- tgz 파일을 열기 위해 WinRAR 프로그램 다운로드
    
    **[https://www.rarlab.com/download.htm](https://www.rarlab.com/download.htm)** - 컴퓨터 사양에 맞는 것 (win-64) 설치
    
- spark-3.2.0-bin-hadoop3.2 폴더 내 모든 파일을 복사 → C드라이브 하단에 spark 폴더를 생성 → 붙여넣기
- spark 파일 안에 conf 파일 클릭 → [log4j.properties](http://log4j.properties) 파일을 연결 프로그램-메모장으로 연다
- `log4j.rootCategory=INFO, console` 에서 INFO를 ERROR로 바꾼다

![](/images/spark_ins/%203.png)

- winutils 파일 설치
    
    설치 링크 : https://github.com/cdarlint/winutils → 설치한 버전 (**[hadoop-3.2.0](https://github.com/cdarlint/winutils/tree/master/hadoop-3.2.0))** 을 클릭 → winutils.exe을 다운로드 받는다
    
- C드라이브에서 차례로 winutils 폴더 생성 → bin 폴더 생성 → 다운받은 winutils.exe 파일을 옮겨준다
- C드라이브에 차례로 tmp 폴더 → hive 폴더를 생성한다
- 파일 사용 권한을 얻기 위해 CMD(명령어 프롬프트)를 켜서 아래 명령어를 차례로 입력한다
    
    `cd c:\winutils\bin`  `winutils.exe chmod 777 \tmp\hive` 
    
- 시스템 환경변수 설정 → 환경변수 → 새로 만들기
- SPARK_HOME 환경변수를 설정
    
    변수이름 : SPARK_HOME / 변수 값 : C:\spark → c드라이브 폴더의 spark로 가서 경로 복사
    
- JAVA_HOME 환경변수를 설정
    
    변수이름 : JAVA_HOME / 변수 값 : C:\jdk → c드라이브 폴더의 jdk로 가서 경로 복사
    
- HADOOP_HOME 환경변수를 설정
    
    변수이름 : HADOOP_HOME / 변수 값 : C:\winutils → c드라이브 폴더의 winutils로 가서 경로 복사
    
- PATH → 편집 클릭 → 아래 코드를 추가
    
    • %SPARK_HOME%\bin
    
    • %JAVA_HOME%\bin
    
- 파이썬 환경설정 추가
    
    변수 이름 : PYSPARK_PYTHON / 변수 값 : python
    
    (옵션) - 주피터 노트북 추가
    
    변수 이름 : PYSPARK_DRIVER_PYTHON / 변수 값 : jupyter
    
    변수 이름 : PYSPARK_DRIVER_PYTHON_OPTS / 변수 값 : notebook
    
- 스파크가 잘 설치되었는지 확인
- cmd 열어서 c:\spark\pyspark 로 경로 설정
- 아래 코드가 실행되는지 확인

```
>>> rd = sc.textFile("README.md")
>>> rd.count()
109
```

출처: **[Spark Installation on Windows 10](https://dschloe.github.io/python/python_edu/00_settings/spark_installation_windows_10/)**