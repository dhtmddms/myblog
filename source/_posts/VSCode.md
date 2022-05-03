---
title: "VScode 연동"
categories :
  - settings
tag :
  - vscode
author : "Oh seungeun"
date : 2022-05-03 10:03:00
---
# VSCode를 WSL과 연동시키기

- 우선 VSCode 설치를 진행한다 (설치 후 재부팅 필요)
    
    **[https://code.visualstudio.com/download](https://code.visualstudio.com/download) /** system installer로 다운로드
    
- VSCode를 관리자 모드로 실행해준다
- extension 버튼을 클릭 후 remote wsl을 검색해서 설치한다
- 설치가 끝나면 두 개의 옵션이 나오는데 다 선택하고 mark done을 클릭한다

![](/images/VSCode/.png)

- open folder 클릭→ WSL에서 설치했던 airflow-test 파일을 선택한다
- 해당 프로젝트를 열고 상단 메뉴바의 Terminal을 선택 → 하단의 화살표 클릭 → Ubuntu(WSL) 클릭

![](/images/VSCode/%201.png)

- python 실행을 위해 폴더를 만들고 그 안에 파일을 만든다
- 명령어 python3 파일명를 입력한다 (wsl에서 실행해주며 파일을 저장해주어야 정상적으로 실행된다)

- 참고 : [https://dschloe.github.io/settings/vscode_wsl2/](https://dschloe.github.io/settings/vscode_wsl2/)