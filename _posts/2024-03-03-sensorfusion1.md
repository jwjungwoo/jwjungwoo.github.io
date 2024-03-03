---
layout: single
title:  "2024년의 다짐"
categories: autonomous vehicle
tag: sensor fusion
author_profile: false #true면 글 안에서 내 프로필 보여줌
sidebar:
    nav: "counts"
#search: false
---

# 2D object detection

## classification & localization

![스크린샷 2024-03-02 191851](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/d79806fd-1824-4a44-bb53-034762fb28cd)   
* classification: single object에 대해서 object의 클래스를 분류하는 문제   
* classification + localization: single object에 대해서 object의 위치를 bounding box로 찾고 (localization) + 
클래스를 분류하는 문제이다. (classification)   

## 2D bounding box

![스크린샷 2024-03-02 192109](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/4ab249b9-1e90-46c1-b751-e44f557e3961)   
* 4 DoF: 물체의 2차원 중점 좌표, 가로 세로   

# 3D object detection

## 3D

![스크린샷 2024-03-02 192129](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/5d66db2f-dc09-45a8-b1c9-28a562f30141)   
* RGB image 즉, 2D 정보를 입력받아도 3D 위치로 출력해야한다.   
* radar 정보는 높이축이 보통 포함되지 않는다.   
* 2D에서 classification에 초점을 맞췄다면 3D에선 3차원에서 안전하게 주행하기 위해 물체의 중심위치를 검출하는 것이 중요해진다.   
* 2D: image 좌표계 vs 3D: sensor 좌표계   

## 3D bounding box

![스크린샷 2024-03-02 194740](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/9ead2785-2a30-4356-b5d6-d5f5fd39db0c)   
* 3차원 물체는 물체가 가질 수 있는 방향이 더 다양해질 수 있다. x, y, z 차원으로 물체가 갖는 방향 정보를 role, pitch, yaw로 표현할 수 있는데 
자율주행 차량환경에선 주로 차량의 헤딩 앵글 값이 두드러지게 변화하고, 차가 앞뒤로 얼마나 기울어져 있는지나 오른쪽 바퀴만 들리는 것 같은 
role같은 정보는 그러한 모션이 상대적으로 덜 발생하기 때문에 주로 yaw를 검출하는데 초점을 맞춘다. 상황에 따라 role, pitch를 추가하여 9 DoF로 
할 수 있다.   

## other 3D object detection

![스크린샷 2024-03-03 192112](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/7245b291-fc29-467a-81b5-1ed5e244ab7d)   
* 위와 같이 bounding box 외에도 다른 방식으로 3D Object를 표현할 수 있다.   
* 최근에 테슬라에서 발표한 방식이다. 차량이 주행할 때 차량 주변에 있는 물체 혹은 공간 정보를 그 물체가 
점유하고 있는지 아닌지 즉, occupancy로 나타내는 방식이다.   
* 위 예시는 대형 트럭의 왼쪽 앞뒤로 조그맣게 다리가 튀어나온 예신데 bounding box로 한다고 할 때 이 다리들을 bounding box에 포함하냐 
포함하지 않냐에 따라 bounding box의 크기가 달라진다. 포함할 땐 빈공간을 활용할 수 없고, 포함하지 않을 땐 다리를 검출하지 못해 사고가 발생할 
수 있다. 이와 다르게 occupancy의 장점은 정형화되지 않은 모양을 갖는 물체를 검출할 때 실제로 물체가 가지고있는 공간만을 
효과적으로 나타낼 수 있어 free space를 효과적으로 검출할 수 있다.   

# sensor fusion의 장점

* better performance: 각각의 센서로만 식별할 수 있는 정보를 합치기에 퍼포먼스가 좋다.   
* better robustness: 한 센서가 고장나도 다른 센서로 식별할 수 있기에 강인함이 좋다.   
