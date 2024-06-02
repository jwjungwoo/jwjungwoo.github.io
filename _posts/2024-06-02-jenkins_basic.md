---
layout: single
title:  "jenkins basic"
categories: cs
tag: jenkins
author_profile: false #true면 글 안에서 내 프로필 보여줌
sidebar:
    nav: "counts"
#search: false
---

# 목차

1) CI/CD 란?   
1-1) CI/CD   
1-2) Jenkins   

2) Jenkins 설치   

3) simple-java-maven-app 을 Github 에서 가져와서 Jenkins Maven project로 실행   

# CI/CD 란?

## CI/CD
Continuous Integration (코드를 통합)   
Continuous Delivery (서비스를 배달)   
   
Continuous Deployment   
![25](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/e04383f0-2c07-420b-aecc-fabed0bb8ab5)   
![26](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/13949f57-095f-4be5-94ac-6de16fac3702)   

## Jenkins
Java Runtime Environment에서 동작   
다양한 플러그인들을 활용하여 각종 자동화 작업을 처리할 수 있음   
일련의 자동화 작업의 순서들의 집합인 pipeline을 통해 CI/CD 파이프라인을 구축함   
Plugin : Credentials Plugin , Git Plugin, Pipeline, Docker plugin and Docker Pipeline (핵심 기능인 파이프라인 마저도 플러그인)   
Credentials Plugin: Jenkins는 그냥 단지 서버일 뿐이기 때문에 배포에 필요한 각종 리소스 
(가령 클라우드 리소스 혹은 베어메탈에 ssh 접근 등)에 접근하기 위해서는 여러가지 중요 정보들을 저장하고 있어야한다. (접근권한을 저장)   
![27](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/a2cd9fa6-04f3-4e2e-9060-4bf830b99535)   

# Jenkins 설치
ec2 ubuntu 기계에서 작업함.   
https://www.jenkins.io/doc/book/installing/linux/#debianubuntu 참고   

합법적인 모듈들을 다운받을 수 있게 키를 받아오는 명령어   
$ sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key   
   
리퍼지토리가 어딨는지 apt에게 알려주어 제대로 동작하게 하기위함   
$ echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null   
   
젠킨스 설치 전 업데이트   
$ sudo apt-get update   
   
젠킨스 설치   
$ sudo apt-get install jenkins     # 설치속도는 네트워크 bandwidth에 따라 다름   
![01](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/771ceb38-5ce0-4309-bb65-5cc5f46d9705)   
젠킨스 자체는 자바로 이루어져있기에 자바도 다운받아야함. 
모든 자바 버전에 지원하는 것이 아니라 그때그때 다른데 현재의 젠킨스는 java17버전만을 지원함.   
   
$ sudo apt update   
   
$ sudo apt install fontconfig openjdk-17-jre   

다 다운받고 clear   
$ java -version과 $  jenkins --version 으로 제대로 설치 됐는지 확인   

제3자의 서비스를 설치한 다음에는 그러한 데몬프로세스가 돌고있는지 (status)를 체크하기위해 
$ sudo systemctl status jenkins 를 침. 그런데 failed to start라는 문구가 써있다.   
![02](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/54bfd6eb-9deb-48cd-99ab-7a7ab2c1ba0a)   
   
따라서 $ sudo systemctl start jenkins를 치고 다시 status를 확인하면 정상적으로 실행됨을 볼 수 있다.   
![03](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/793ebb10-ed57-42ff-979b-03174b7c7acb)   
젠킨스를 jetty web server에 연동되어 jenkins 서비스 제공. jenkins는 우리가 만든 ubuntu기계에서 동작하는 것이 아니라 실제에서는 웹을 통해 ubuntu기계에 제공해줌.   
![04](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/2ee34452-35f8-47c0-89fa-f41cb8ef50af)   
인스턴드의 퍼블릭 IPv4주소:8080을 브라우저 창에 치면 이런 화면이 뜸. (그전에 보안설정에 가서 사용자지정 TCP,UDP의 접근 포트번호를 0-65535, IPv4는 0.0.0.0로 저장해놔야 뜸)   
   
jenkins라는 유저가 만들어놓은 secrets이라는 디렉토리 밑에 패스워드정보를 넣음. 따라서 
$ sudo cat /var/lib/jenkins/secrets/initialAdminPassword로 비밀번호를 확인한다.   
![05](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/e751a577-c63c-4016-b694-452cba3158d1)   
“우리가 만약 소프트웨어를 개발하다 중요한 도구를 하나 만들었다 하면 제일 먼저 해야하는 것은jenkins 오픈커뮤니티에 연락하던지 Cloudbit에 연락을 해서 
내가 만든 소프트웨어도구가 사람들이jenkins 에서 내껄 제공할 수 있는지(plugin으로 다운될 수 있는지) 확인해야함.”   

회원가입절차 따르고 디폴드값은 냅두고 회원가입 완료를 하면 된다.   
![06](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/259b1dd7-ff89-41e1-b9ba-bba8aec369c8)   

현재는 node에 jenkins controller하나만 있다.   
![07](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/a7a659c6-2675-44da-8ed5-8071919d32b9)   
이스트 기계 하나로 여러 프로젝트를 실행할 수 있으나 프로젝트 마다 별도의 이스트 기계를 사용하는 것이 깔끔하다.   

널리 쓰이는 프레임워크(dependency)가 굉장히 많은데 그것을 관리해주는 것이 빌드도구이고 빌드도구 중 하나가 maven임. 
어떤 라이브러리 등 필요한 것을 maven의 repository에 가서 필요한 도구를 가져오면됨. maven이 필요한 것들을 
자동으로 compile타임에서 붙여줌.빌드도구엔 java에선 intellij, 코딩 시 VSCode등이 있다.   
   
jenkin는 모든 언어의 프로젝트를 컨드롤해주는건아니고 자바 프로젝트를 컨트롤해줌. 근데 지금은 툴 추가하면 된다곤함.   

Maven: java를 사용한 다양한 프레임워크에서 필요한 dependency들을 자동적으로 찾아서 제공해줌.   
Set up an agent(아래 그림): 또다른 이스트기계던, mac이던, window 기계건 지금 현재 jenkins control이 돌고있는 이스트기계말고 다른 것들을 설치할 수 있음.   
![08](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/efcb229c-8306-48d3-a308-5eb2b4e6c280)   
![09](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/d7045aca-60d1-4be2-919a-4e3fd3621f93)   
New item을 누르고 이름은 마음대로 정하고 FreeStyle project를 누른다.   
설명엔 본인이 쓰고 싶은 말을 쓰면 됨. 필자는 demo 라 적음.   
![10](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/0559a5f5-670e-4e1d-a304-236ea9f8c18b)   

build Steps엔 Execute shell을 누른다.   
Command엔 필자의 경우   
   
pwd   
echo "헬로, 학교!"   
   
라고 쳤다.   
   
저장하고 ‘지금 빌드’를 누르면 초록색 글자 #1이 뜬다.   
![11](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/c3e528ee-a6c7-4526-8fd0-36a4b9146fca)   
job이 일어나는 디렉토리는 workspace 옆에 적혀있다. 모든 정보가 저장되고 jenkins job이 벌어짐.   
아래에선 maven명령을 사용해서 자바프로그램을 빌드할 것임.   
![12](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/de19b652-7513-4167-9f1c-df6da2f087a0)   

# simple-java-maven-app 을 Github 에서 가져와서 Jenkins Maven project로 실행   
https://github.com/jenkins-docs/simple-java-maven-app 참고   
   
jenkins에서 java maven실험해보라고 제공해줌.   
![13](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/08d1bd56-3f86-4ae3-ae02-5ebc069be48c)   
maven은 pom.xml을 보고 필요한 dependency들을 자바프로그램이 빌드될 때 가져다줌.   
프로젝트를 구성하고있는 여러 object들을 xml format으로 적어둠.   
   
Maven package라는걸 maven goal이라 부름.   
   
$ sudo apt-get install git -y   
$ sudo apt-get update   
   
만약 노란색으로 systemctl daemon reload라는 오류가 뜨면 $ sudo systemctl daemon reload라고 치면 됨.   
maven설치를 확인하고자한다면 $ mvn -v 를 치면됨.   
당연히 설치 안 돼있을거고 sudo apt install maven을 그대로 치면 우리가 원하는 마지노선 버전 (pom.xml에 적혀있음)에 따른 maven을 설치 못 함.   
   
소프트웨어를 리눅스에 설치하는 방법은 크게 관례상 user/local 혹은 /opt에 장착함. ubuntu linux에 필수적으로 필요한거면 user/local에 아니면 /opt 디렉토리에 저장함.   
   
$ cd /opt   
![14](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/5a172b8e-5c2d-42d7-914d-60d7f84e5d58)   
링크 주소 복사 후   
$ sudo wget https://dlcdn.apache.org/maven/maven-3/3.9.7/binaries/apache-maven-3.9.7-bin.tar.gz   
$ sudo  tar -xvf apache-maven-3.9.7-bin.tar.gz   
$ sudo ln -s /opt/apache-maven-3.9.7 /opt/maven   
$ sudo vi /etc/profile.d/maven.sh   
export M2_HOME=/opt/maven   
export MAVEN_HOME=/opt/maven   
export PATH=${M2_HOME}/bin:${PATH}   
$ sudo chmod +x /etc/profile.d/maven.sh   
$ source /etc/profile.d/maven.sh   
$ mvn -v   
   
$ cd ~                    ##우분투 디렉토리로 돌아감   
$ mkdir maven-test && cd maven-test   
:~/maven-test$ git clone https://github.com/jenkins-docs/simple-java-maven-app   
$ ls -al   
![15](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/2ad8c329-b7a9-48e1-8b03-63ea3d9ee025)   
![16](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/cfe9b2e0-54a2-432a-a714-055883f43fab)   
$ cd simple-java-maven-app   
$ ls -al           ## 깃허브에 있는거 잘 다운됨   
$ mvn compile          ## 컴파일에 필요한 dependency modules 가져옴   
$ mvn test                 ## test에 필요한 것 가져오고 +  /src에 있는 test파일 2개를 실행해봄.   
![17](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/2097164f-7ad5-494e-bd34-3644a0d42d04)   
$ mvn package          ##이 파일을 최종적으로 실행파일로 만듦   
   
위아래 사진을 비교하면 기존에 없었던 target이라는 디렉토리가 생김. 실행파일을 target이란 디렉토리를 만들어 저장함.   
![18](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/3aa67167-354a-4777-915f-eef03111478b)   
![19](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/72b2de6e-77c2-411e-bb38-24dd8a10d818)   
$ ls target   
$ java -jar target/my-app-1.0-SNAPSHOT.jar   
$ cd ..   
$ cd ..   
~$ cd /opt   
/opt$ ls -al   
   
apach-maven-3.9.7에 maven이 장착돼있음   
   
maven에선 xml파일로 object가 만들어지는데 gradle은 복잡함. jenkins는 maven도구를 아주 밀접하게 사용해서 jenkins를 가지고 아주 다양한 실행을 할 수 있으며 java프로그램에 특화돼있다.   
   
jenkins시스템에서 소위 jenkins controller가 돌고있는 우분투기계에 maven시스템이 장착돼있는 디렉토리를 maven홈으로 우리가 
등록해주면 추후에 jenkins가 maven프로젝트를 실행할 때 자동화해줌.   
   
/opt$ cd apache-maven-3.9.7   
$ pwd   
주소 복사해서 아래 사진처럼 붙여넣음   
![20](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/44d1994a-696f-4368-bd69-3211f38d9977)   
그럼 jenkins에 maven이란 도구를 등록함   
   
Enter new item 누름   
![21](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/8646db3a-8737-4eac-a360-b09f3194cfb4)   
![22](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/057990c6-ba4c-4358-9c68-d193f02edd6c)   
이번예시에선 credential에 아무것도 안했지만 깃이 이번 예시와 같은 public이 아니라 private이면 추가적으로 접근권한을 추가해야한다.   
   
저장하고 지금빌드 클릭   
![23](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/e55cfd65-0d0b-40bc-ab6d-f3a2cc154a84)   
맨마지막까지 스크롤해서 내림   
Building jar에서 /var/lib/jenkins/workspace/simple-java-maven-app/target/ 까지만 복사   
   
$ cd /var/lib/jenkins/workspace/simple-java-maven-app/target/   
$ ls -al   
java -jar my-app-1.0-SNAPSHOT.jar   
![24](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/76f7bcad-e426-42d3-a9e0-9ce3a9b04e02)   
Hello world!가 출력된 것을 볼 수 있다.
