---
title: "파이참 가상환경 세팅"
categories :
  - settings
  - 
author : "Oh seungeun"
date : 2022-05-01 13:27:00
---

# 파이참 가상환경 세팅

- file-close project
- new project 열고 browse에서 desktop→ 새 폴더 만들기(미리 만들어 놔도 됨) → 파일명 web 생성→생성된 web 클릭→ create 클릭
- 터미널-깃배쉬
- which python-실행 시 /web/venv/Scripts/python이 경로가 보여야 함
    
    (안될 때는 source venv/Scripts/activate)
    
- python main → [main.py](http://main.py)창에 import numpy as np, print(”numpy version: “,np.__version)→ python [main.py](http://main.py) (실행 안 될 거임, numpy가 설치가 안 되어있기 때문) → pip install numpy (라이브러리를 설치하는 명령) → 다시 python [main.py](http://main.py) 실행

![Untitled](/images/virt_set/Untitled.png)

---

(4/19) vscode-git bash로 가상환경 설치,실행이 안 됨 → 경로에 python 3.10 이 껴있어서

우선 파이참에서 우회해서(파일 만들기, 설치 등등은 파이참으로 진행하라는 말)  vscode로 들어가서 작업해야 됨