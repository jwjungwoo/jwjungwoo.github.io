---
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
<img src="https://github.com/user-attachments/assets/f5416d20-177d-4141-8202-aa50dc73604b" width="600" height="400">   

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

# 원격접속과 권한설정
## make 사용하는 이유
```c
dog.c
cat.c
tiger.c

gcc -o ./dog ./dog.c
gcc -o ./cat ./cat.c
gcc -o ./tiger ./tiger.c

// 파일이 많아지면 Shell Script로 빌드하면 좋다.
// 근데 문법도 복잡한 make는 왜 사용하는가?
// 이유는 바로 증분빌드$cd / : 바로 루트 디렉토리로 가져서 일일히 다 $cd .. 할 필요가 없음
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
<img src="https://github.com/user-attachments/assets/f5416d20-177d-4141-8202-aa50dc73604b" width="600" height="400">   

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

# 원격접속과 권한설정
## make 사용 이유
```c
dog.c
cat.c
tiger.c

gcc -o ./dog ./dog.c
gcc -o ./cat ./cat.c
gcc -o ./tiger ./tiger.c
```
✅ 증분빌드   
파일이 많아지면 Shell Script로 빌드하면 좋다. 근데 문법도 복잡한 make는 왜 사용하는가? 이유는 바로 증분빌드때문이다. 증분빌드란 make의 증분 빌드는 변경된 파일만 다시 컴파일하여 전체 빌드 시간을 줄이는 기능이다.   

## 원격접속
1. 윈도우에 MobaXterm 설치   
2. 우분투에 ip 확인할 수 있는 프로그램 다운 ($sudo apt install openssh-server –y)   
3. $sudo apt install net-tools -y   
4. $ipconfig 로 우분투 ip 확인   
   
윈도우 cmd에서 ifpconfig로 확인하면 192.168.203.194 이다.   
<img src="https://github.com/user-attachments/assets/f727fb06-73d5-4822-a10f-1a7362452dfc" width="600" height="200">   
   
우분투 terminal에서 ipconfig로 확인하면 10.0.2.15와 127.0.0.1 이다.   
<img src="https://github.com/user-attachments/assets/5c253db7-8b45-449e-9639-b4f0dd0fc8e0" width="400" height="400">   
## NAT
✅ NAT   
Network Address Tramslation   
사설 IP(Private IP)를 공인 IP(Public IP)로 변환하는 기술이다. IPv4의 모자란 주소를 보완하고, 그 안의 사설 IP를 감추기 위해 사용한다. 
공인 IP는 회사의 라우터고, 사설 IP는 회사원들이 각자 사용하는 IP 주소이다. 다만 공용 IP는 세상에 유일하다. 하나의 공용 IP에서 
사설 IP는 겹칠 순 없다. 다만 서로 다른 공용 IP들은 같은 사설 IP를 사용할 수 있다.   

## 원격접속
Virtual Box 설정에 들어가 아래 사진과 같이 설정을 해준 뒤 윈도우에서 C:\Users\USER>ssh -p 2222 jw@127.0.0.1 로 접속할 수 있다. 이는 MobaXterm에서도 된다. 
MobaXterm에선 UI가 제공돼서 보기 편하다.   
<img src="https://github.com/user-attachments/assets/408e95da-aa51-4f51-a626-ed6cacf7ea27" width="500" height="400">   

## terminal
<img src="https://github.com/user-attachments/assets/a0f5b44f-8228-4aac-9494-426fbccbf69f" width="600" height="350">   
<img src="https://github.com/user-attachments/assets/fbd04a27-67ce-4998-953b-d455ba2cb1ab" width="600" height="350">   
<img src="https://github.com/user-attachments/assets/d315cf48-3d42-4d46-8495-5801e44d81af" width="600" height="350">   

## root
✅ Host   
리눅스가 설치된 컴퓨터를 Host라고 한다. 여러 사용자가 하나의 컴퓨터를 사용할 수 있다.   
<img src="https://github.com/user-attachments/assets/fcfcefe5-30a4-4c2f-bf48-0c3bdae0ada2" width="600" height="400">   
   
✅ user 설정   
<img src="https://github.com/user-attachments/assets/40d45db0-209e-44d8-b1b4-cec80bfec49b" width="600" height="400">   
```c
$sudo adduser kfc  // user 추가 가능
$su kfc // kfc 계정으로 접속하기
$su jw
$sudo deluser kfc // kfc 계정삭제
$sudo rm -r kfc // kfc 관련파일 삭제
```
kfc에서 sudo apt install sl -y 명령어를 하면 안 된다. kfc는 adduser로 만든 사용자이기에 root 권한이 없다. 물론 줄 수는 있지만 수업시간에 다루진 않았다. 중요하지 않나보다.   
   
✅ 그룹
<img src="https://github.com/user-attachments/assets/d27545f0-04c0-427a-b99b-88e5ff9315ea" width="600" height="400">   
서버에서는 그룹이 중요하나 임베디드에선 그렇게 중요하진 않다. 따라서 패스!   
```c
$groups [계정명] // 특정 user가 소속된 그룹 확인하기
// jw는 처음 생성한 사용자여서 더 많은 그룹에 속해 있다.
```   

## 리눅스 파일
✅ 리눅스는 모든 것을 파일로 관리한다.   
<img src="https://github.com/user-attachments/assets/d096b670-8d0b-402c-80b6-63f4359a5d5b" width="600" height="400">   
<img src="https://github.com/user-attachments/assets/db066f90-3d88-40a3-a05a-c4e7d4ec42bb" width="600" height="400">   
파일의 종류 다음으로 나오는 9개의 글자는 권한을 의미한다.   
onwer 권한: 3글자, owner가 속한 group: 3글자, other 권한: 3글자 순이다.   

✅ 파일 권한 바꾸기 실습   
```c
 $mkdir work
 $cd ./work
 $touch abc
 $ls –al
```   
아래의 코드로 권한을 바꿀 수 있다.   
```c
$sudo chmod u=rwx abc
```
```c
// user / group / other / all 에 권한을 부여할 수 있다.
$sudo chmod u=rx abc
$sudo chmod g=wx abc
$sudo chmod o=r abc
$sudo chmod a=x abc

// 부여할 때 기존에 있던 권한에 +,-하거나 = 지정할 수 있다.
$sudo chmod u+r abc
$sudo chmod g+r abc
$sudo chmod o+r abc
$sudo chmod a+w abc

// 보통은 이진수로 표현한다
r---w---x = 421
rwx rwx rwx = 777
rw- rw- r-- = 664
```

# Build System
## gcc build process
✅ 임베디드 리눅스에서 자주 사용되는 Build System 종류
make (우리 수업에서는 make만 다뤘다.)   
cmake   
   
✅ 빌드란?   
소스코드( .c, .cpp )에서 실행 가능한 Software( .elf, .exe )로 변환하는 과정 (Process) 또는 결과물   
   
✅ gcc 기준 빌드 과정은 크게 둘로 나뉜다.   
Compile & Assemble: 하나의 소스코드 파일이 0과 1로 구성된 Object 파일이 만들어짐   
Linking: 만들어진 Object 파일들 + Library 들을 모아 하나로 합침   
   
✅ gcc 예재   
gcc 명령어를 이용하면 여러개의 파일을 같이 Build할 수 있다.   
1. work 파일에 main.c cat.h cat.c dog.h dog.c 이렇게 5개 파일을 만든다.
2. .c 파일을 각각 Compile & Assemble 하자.
```c
$gcc –c ./main.c
$gcc –c ./dog.c
$gcc -c ./cat.c
```   
3. .o 파일이 생성된다. ($ls -al 로 확인)   
4. 만들어진 Object 파일들과 라이브러리 함수들을 하나로 합친다.
```c
$gcc ./main.o ./dog.o ./cat.o -o ./go
```   
5. $.go를 하면 한번에 실행된다.   
   
근데 gcc는 똑똑해서 굳이 나눠서 작업하지 않아도 동작한다.   
<img src="https://github.com/user-attachments/assets/e0f368bb-64be-4dde-a706-260c362c5e8c" width="600" height="400">   

## 빌드 자동화 스크립트

✅ bash shell script 를 이용한 build 방식   
자동화 프로그램 개발에 특화된 Script 언어가 있다. ( .sh ) bash Script로 build 스크립트를 작성하고 실행해봤다.   
```c  
$vi ./run.sh

<파일내용>
#!/bin/sh
gcc -c ./*.c -I .
gcc -o ./zoo ./*.o

<./run.sh 실행>
$source ./run.sh     하면 zoo라고 만들어진다.
$./zoo               라고 하면 된다.
```   
   
.o 파일들을 지우는 것도 가능하다.   
```c
$vi ./clean.sh

<파일내용>
#!/bin/sh

rm -r *.o
rm -r ./zoo
```   
<img src="https://github.com/user-attachments/assets/2dffe5f2-a176-4dca-a94a-3e66a5ef9709" width="600" height="600">   

## build system 체험
✅ make   
make: 프로그램   
Makefile: 뭘 make 할지 기술한 파일   
make프로그램이 Makefile을 읽어옴.   
```c
$sudo apt install make -y
```   
<img src="https://github.com/user-attachments/assets/720d0174-77ca-4a3f-9206-9df3136610ef" width="600" height="600">   
Makefile 내용을 기술할 때는 꼭 탭으로!!!!!!!   


# System Call
# Thread
# 라즈베리파이
# Character Device Driver 개발
