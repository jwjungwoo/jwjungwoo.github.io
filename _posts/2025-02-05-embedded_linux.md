![dif](https://github.com/user-attachments/assets/f5416d20-177d-4141-8202-aa50dc73604b)---
layout: single
title:  "임베디드 리눅스 시스템 프로그래밍"
categories: autoever
tag:
author_profile: false #true면 글 안에서 내 프로필 보여줌
sidebar:
    nav: "autoever"
#search: false
---

# 리눅스 기본
## linux
✅ 리눅스의 정확한 의미   
<img src="https://github.com/user-attachments/assets/cc1d0c76-d4a4-4660-bac6-a6f11756b51b" width="600" height="400">   
리눅스는 서버 PC의 OS로 가장 많이 사용된다. 무료에 성능도 좋기 때문이다.

## ubuntu 설치
윈도우에서 우분투를 다운받은 뒤 가상머신에서 우분투 이미지를 넣어 새로운 환경을 만들었다. 오류가 났을 때 돌아갈 수 있는 스냅샷의 기능도 연습해봤다.   
<img src="https://github.com/user-attachments/assets/930aaa19-0604-48cc-8b0a-dd63a10bcff8" width="600" height="400">   

✅ 명령어 & 단축키   
```c
ctrl + alt + t : 터미널창 열기
ctrl + u : 한줄 지우기
ctrl + l 또는 $clear : 터미널창 초기화
ctrl + s : 터미널창 정지
ctrl + q : 정지된 터미널창 정지 풀기
$sudo apt purge 프로그램 : 프로그램 삭제
$pwd : 현재 디렉토리가 나옴
$cd / : 바로 루트 디렉토리로 가져서 일일히 다 $cd .. 할 필요가 없음
$touch "프로그램명" : 파일 만들기
$mkdir "디렉토리명" : 폴더 만들기
$mv ./abc ./dd : 현재 디렉토리에 있는 abc파일을 현재 디렉토리에 있는 dd폴더로 옮김
$mv ./abc .. : 현재 디렉토리에 있는 abc파일을 상위 디렉토리로 옮김
$mv "파일" "파일이름" : 파일 이름 변경
$rm -r "디렉토리명" : rm "디렉토리명"을 할 때 안에 파일이 있으면 삭제가 안되나 이 명령어는 삭제 가능
```
<img src="https://github.com/user-attachments/assets/dc706425-eeac-4ced-8823-2e884fa56fc0" width="600" height="400">   

## shell
<img src="https://github.com/user-attachments/assets/4260ecc2-d8c1-4842-887a-a8abb8c9e67c" width="600" height="400">   
<img src="https://github.com/user-attachments/assets/b306d01f-a8b7-4b53-bc6e-c6fc1858b814" width="600" height="400">   

## dir
```c
.   : 현재 디렉토리
..  : 상위 디렉토리
~   : 홈 디렉토리
/   : root 디렉토리 ex) $cd /를 치면 최상위 디렉토리가 나옴.
윈도우는 최상위 디렉토리가 c, d, e 같은 거다.
```

## 백그라운드 실행
<img src="https://github.com/user-attachments/assets/85bf2d4f-6592-4189-aa88-e545568b5e27" width="600" height="400">   

# vi 에디터와 gcc
## test editor
GUI의 대표 에디터 : gedit   
CLI의 대표 에디터 : vi   
   
아래의 코드를 하면 hello.c 파일을 실행하는 걸 ./hello로 지정한다.   
```c
$gcc -o ./hello ./hello.c
$./hello
28
배정우
```

## 다른 이름으로 저장
<img src="https://github.com/user-attachments/assets/de2b16ec-5831-4e4b-bc36-63631fbb7f08" width="600" height="400">   

## 저장 안하고 종료
<img src="https://github.com/user-attachments/assets/128e52dd-8def-4656-a512-b7aa3ca775a1" width="600" height="400">   

## vi에서 복사하기
✅ 마우스 사용하기   
마우스로 더블 클릭해서 선택하면, 복사가 된다.   
휠 키를 눌러서 붙여 넣기 한다.   

## 찾기
insert모드 하기전에 /를 치고 n을 누르면서 위에서부터 찾을 수 있다.   
<img src="https://github.com/user-attachments/assets/437163ed-35ea-4fc4-9518-e1e221d1c161" width="600" height="600">   

## 설정
<img src="https://github.com/user-attachments/assets/ddf70269-83dc-4e94-8d91-65cada939823" width="600" height="600">   
근데 나갔다 들어오면 세팅이 다 사라진다. 디폴트로 설정하려면 .vimrc 파일을 만들면 된다.   
<img src="https://github.com/user-attachments/assets/98ccbb5e-8805-4375-8bbc-fd07ca62e4ba" width="600" height="600">   

# 원격접속과 뭔한설정
# Make Build System
# System Call
# Thread
# 라즈베리파이
# Character Device Driver 개발
