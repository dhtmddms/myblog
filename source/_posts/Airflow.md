---
title: "에어플로 설치"
categories :
  - settings
tag :
  - airflow
author : "Oh seungeun"
date : 2022-05-03 09:39:00
---

# Airflow 세팅

- airflow 설치를 위해 pip을 설치한다

```jsx
$ sudo apt install python3-pip
```

- virtualenv 라이브러리를 설치한다

```jsx
$ sudo pip3 install virtualenv
```

- c드라이브에 airflow-test 파일을 생성 후 해당 디렉토리로 들어간다

```jsx
$ cd mnt
$ cd c
$ cd airflow-test
```

- 가상환경을 생성한다

```jsx
$ virtualenv venv
```

- 가상환경에 접속한다 (<> 가상환경에서 나오는 거는 deactivate 입력)

```jsx
$ source venv/bin/activate
```

- .bashrc 파일을 수정한다

```jsx
$ vi ~/.bashrc
```

- 아래와 같이 입력되었는지 확인

```jsx
export AIRFLOW_HOME=/mnt/c/airflow-test
```

- 참고  :  vi  편집기를 수정했으면 꼭 source로 업데이트 해줘야 함

```jsx
$ source ~/.bashrc
```

- 꼭 에코로 잘 들어가 있는 지 확인해본다

```jsx
$ echo $AIRFLOW_HOME
```

- pwd(현재 경로를 알려줌)을 쳐서 같은지 확인

```jsx
$ pwd
```

- PostgreSQL, Slack, Celery 패키지를 동시에 설치하는 코드를 작성한다

```jsx
$ pip3 install 'apache-airflow[postgres, slack, celery]'
```

- 에어플로 실행을 위해 DB를 초기화 (이때 가상환경에 접속되어 있어야함, 안 되어 있음 다시 접속 )

```jsx
$ airflow db init
```

- 계정을 생성한다

```jsx
$ airflow users create --username airflow --password airflow --firstname evan --lastname airflow --role Admin --email your_email@some.com
```

- vscode의 cfg 파일에 load_examples=False 로 바꿔준다
- airflow db reset
- airflow webserver -p 8080
- ubuntu에서 dags 파일을 만든다, 데이터 파일도 만든다

```jsx
$ mkdir dags
```

```jsx
$ mkdir data
```

- 데이터 디렉터리로 들어간다

```jsx
$ cd data
```

- step01_writecsv.py를 vi 편집기로 들어간다

```jsx
$ vi step01_writecsv.py
```

- step01_writecsv.py에 내용을 추가한다

```jsx
from faker import Faker
import csv
output=open('data.csv','w')
fake=Faker()
header=['name','age','street','city','state','zip','lng','lat']
mywriter=csv.writer(output)
mywriter.writerow(header)
for r in range(1000):
    mywriter.writerow([fake.name(),fake.random_int(min=18, max=80, step=1), fake.street_address(), fake.city(),fake.state(),fake.zipcode(),fake.longitude(),fake.latitude()])
output.close()
```

- 저장하고 나와서 파일을 확인해본다

```jsx
$ ls
```

- dags 디렉토리로 가서 csv2json.py파일을 생성한다

```jsx
$ cd dags
$ vi csv2json.py
```

- 내용을 입력한다

```jsx
import datetime as dt
from datetime import timedelta

from airflow import DAG
from airflow.operators.bash import BashOperator
from airflow.operators.python import PythonOperator

import pandas as pd

def csvToJson():
    df=pd.read_csv('data/data.csv')
    for i,r in df.iterrows():
        print(r['name'])
    df.to_json('fromAirflow.json',orient='records')

default_args = {
    'owner': 'evan',
    'start_date': dt.datetime(2020, 3, 18),
    'retries': 1,
    'retry_delay': dt.timedelta(minutes=5),
}

with DAG('MyCSVDAG',
         default_args=default_args,
         schedule_interval=timedelta(minutes=5),      # '0 * * * *',
         ) as dag:

    print_starting = BashOperator(task_id='starting',
                               bash_command='echo "I am reading the CSV now....."')
    
    csvJson = PythonOperator(task_id='convertCSVtoJson',
                                 python_callable=csvToJson)

print_starting >> csvJ
```

- 웹 서버랑 스케쥴러를 실행한다
- 가상환경 tip: export AIRFLOW_HOME = “$(pwd)”
- vscode는 파일 열기를 통해 파일을 바꿔준다

