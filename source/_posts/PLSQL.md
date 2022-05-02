---
title: "PL/SQL basic"
categories :
    - PL/SQL
tag :
  - PL/SQL
author : "Oh seungeun"
date : 2022-05-02 17:36:00
---
# PL/SQL

- 기본 구조
    - 블록
        - 선언부 ,실행부, 예외 처리부로 구성됨
    - 이름부 : 블록의 명칭이 오는 자리, 생략하면 익명 블록이 됨
    - 선언부 : DECLARE로 시작, 각종 변수, 상수, 커서 등을 선언
    - 실행부 : 실제 로직을 처리하는 부분, 일반 SQL문, 조건문, 반복문 등이 오는 자리
    - 예외 처리부 : EXCEPTION 절로 시작, 실행부에서 오류가 발생하면 처리한 내용을 기술하는 부분, 생략이 가능
    - *주의* : 문장 끝마다 세미콜론(;)을 붙여야 오류 없이 실행 됨
- 익명 블록
    - 이름이 없는 블록
        - vi_num이란 NUMBER형 변수 선언
        - PL/SQL에서 값의 할당은 등호가 아닌 ‘:=’로 처리
        - vi_num에 100 할당
        - DBMS_OUTPUT 패키지는 괄호 안에 있는 매개변수 값을 출력해줌
    
    ```jsx
    DECLARE
    	vi_num NUMBER;
    BEGIN
    	vi_num := 100;
    
    	DBMS_OUTPUT.PUT_LINE(vi_num)
    END;
    ```
    
    - 출력하려면 ‘SET SERVEROUTPUT ON’ 을 맨 앞에 추가해야함
    - 총 소요시간은 ‘SET TIMING ON’ 명령어를 실행
    
    ```jsx
    SET SERVEROUTPUT ON
    SET TIMING ON
    DECLARE
    	vi_num NUMBER;
    BEGIN
    	vi_num := 100;
    
    	DBMS_OUTPUT.PUT_LINE(vi_num)
    END;
    ```
    
- PL/SQL 구성요소
    - 변수
        - 변수는 다른 프로그래밍 언어에서 사용하는 변수와 개념이 같음
        - 선언부에서 변수 선언을 하고 실행부에서 사용하는 방식
        - 변수명 데이터 타입 := 초깃갑;
            - 이때 초깃값을 할당하지 않으면 NULL이 됨
        - 변수로 선언 가능한 데이터 타입에는  SQL데이터 타입과 PL/SQL 데이터 타입이 존재
            - 정확히 말하면 PL/SQL에 SQL 데이터 타입이 포함
            - PL/SQL에만 있는 데이터 타입
                - BOOLEAN
                - PLS_INTEGER
                - BINARY_INTEGER
    - 상수
        - 변수와 달리 한 번 값을 할당하면 변하지 않음
        - 상수면 CONSTANT 데이터 타입 := 상수값;
        - 상수 선언시 반드시 CONSTANT를 붙여 변수와 구분함
    - 연산자
        
        ![](/images/PLSQL/Untitled.png)
        
    - DML문
        - 실행하거나 출력하는 것이 아닌 조작하는 언어
        - 사원 테이블에서 특정 사원의 이름과 부서명을 가져와 출력하는 코드
        
        ```jsx
        DECLARE
        	vs_emp_name VARCHAR2(80); --사원명 변수
        	vs_dep_name VARCHAR2(80); --부서명 변수
        BEGIN
        		SELECT a.emp_name, b.department_name
        		 INTO vs_emp_name, vs_dep_name
            FROM employees a, departments b
        		WHERE a.department_id = b.department_id
        	    AND a.employee_id = 100;
        DBMS_OUTPUT.PUT_LINE (vs_emp_name || '-' || vs_dep_name);
        END;
        ```
        
        - *중요 : 테이블에 있는 데이터를 선택해 변수에 할당할 때는 SELECT 문에서 INTO 절을 사용한다
        - 따로 변수 타입을 선언하기 번거로울 때는 %TYPE 키워드로 대체 가능
        
        ```jsx
        DECLARE
        	vs_emp_name employees.emp_name%TYPE; --사원명 변수
        	vs_dep_name departments.department_name%TYPE; --부서명 변수
        BEGIN
        		SELECT a.emp_name, b.department_name
        		 INTO vs_emp_name, vs_dep_name
            FROM employees a, departments b
        		WHERE a.department_id = b.department_id
        	    AND a.employee_id = 100;
        DBMS_OUTPUT.PUT_LINE (vs_emp_name || '-' || vs_dep_name);
        END;
        ```
        

- 라벨
    - PL/SQL 코드가 길어질 때 특정 블록 단위로 라벨을 붙이면 가독성이 좋아져서 유용
    - 반복문에 라벨을 붙이면 조건에 따라 EXIT 문으로 해당 반복문 빠져 나오는거 쉬움
    
- 조건문 (IF문)
    - 급여가 3500이상이면 고연봉자, 2000이상이면 중간 연봉자, 그 외에는 저연봉자로 구분하는  조건문
    
    ```jsx
    IF v_sal >= 3500 THEN
            DBMS_OUTPUT.PUT_LINE('high salary');
        ELSIF v_sal >= 2000 THEN
            DBMS_OUTPUT.PUT_LINE('middle salary');
        ELSE
            DBMS_OUTPUT.PUT_LINE('low salary');
        END IF;
    ```
    
- 반복문
    - 가장 기본적인 형태는 LOOP문
    - 구구단
    
    ```jsx
    DECLARE
        v_count number(10):= 0;
    BEGIN
        LOOP
            v_count := v_count +1;
            DBMS_OUTPUT.PUT_LINE('2 x ' || v_count || ' = ' || 2 * v_count);
          EXIT WHEN v_count = 9;
        END LOOP;
    END;
    ```