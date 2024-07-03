---
layout: single
title:  "LiDAR-camera calibration(이론편)"
categories: sensorfusion
tag: 
author_profile: false #true면 글 안에서 내 프로필 보여줌
sidebar:
    nav: "counts"
#search: false
---

# sensor fusion

총 세단계로 이루어진다.
1. intrinsic calibration (카메라 자체의 왜곡 보정)   
2. extrinsic calibration (라이다와 카메라 관계 행렬 보정)   
3. 비전 인식이 잘 되면 early fusion 중 camera만 객체를 판단하며 camera에 LiDAR정보를 입히고, 잘 안 되면 late fusion을 통해 camera와 LiDAR 둘다 객체를 판단   
   
우리팀은 early fusion을 목표로 하고 있으며 extrinsic calibration은 사용하지 않고 LiDAR 포인트들을 직접 pixel 값에 매핑할 것이다.   
따라서   
1. intrinsic calibration   
2. mapping   
3. early fusion   
순으로 할 것이다.   
   
국토부 대회에 라바콘으로 이루어진 주차구역에 주차하는 미션이 있는데 이건 LiDAR로 봐야할 것 같아서 late fusion을 써야할수도 있다.   

이 글에선 맨 처음 말한 기본 세단계에 대해 설명하려한다.   

# calibration? fusion?

calibration(보정)은 서로 다른 여러 개의 좌표계를 맞추는 과정이고 fusion(융합)은 calibration을 통해서 나온 결과를 이용하는 것을 말한다.   
   
calibration   
<img width="958" alt="001" src="https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/173435a7-1beb-4f39-ad5b-ff427ca96845">   
   
fusion   
<img width="960" alt="002" src="https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/2f0eefff-39f8-4136-a4cd-9e22b2d120d7">   

위와 같이 calibration을 검색하면 checkerboard로 좌표계를 맞추는 과정이 나오고 fusion을 검색하면 융합된 결과 화면이 나오게 된다.   

# calibration

## camera calibration

<img width="571" alt="003" src="https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/d3ae1331-65d6-4cd1-9b7c-e44563045015">   
camera calibration은 카메라의 내부 파라미터(intrinsic parameter)를 구하는 과정이다.   
camera calibration 작업은 checkerboard를 통해서 이루어진다. open source와 관련된 작업과 내용들이 많아 쉽게 구할 수 있다.   

## LiDAR-camera Calibration

<img width="584" alt="004" src="https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/572c1756-daa3-45cc-9e14-d7719ba22a02">   

LiDAR-Camera calibration은 서로 다른 두 좌표계를 일치시키는 작업을 말한다. 정확히 얘기하면 카메라와 LiDAR 좌표계간의 외부 파라미터(extrinsic parameter)를 구하는 것이다.   
   
<img width="555" alt="005" src="https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/b67abc1a-4f42-4672-b590-59fb982f73c9">   
LiDAR-Camera calibration은 크게 두 가지 방식으로 나눠진다.   
checkerboard과 같이 target을 중심으로 사용하는 target-based method, target없이 풍경과 같은 지형지물을 이용해서 하는 targetless method가 있다.   

찾다보니 LiDAR-Camera Calibration에 대해서 정리한 github가 있었다. Target-based와 Targetless에 대해 구분하였으며, 
논문과 코드를 잘 정리해 두어서 좋은 것 같다. 관심있으신 분들은 한 번 봐도 좋을 것 같다! (https://github.com/Deephome/Awesome-LiDAR-Camera-Calibration)   

### target-based method

<img width="256" alt="006" src="https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/e09173d4-03a6-4f20-8e12-dcb33fe9373e">   
<img width="296" alt="007" src="https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/32ed05e9-c8c5-4e6b-9cb7-a5229684aa17">   
명확한 목표인 target을 이용해서 LiDAR와 camera를 calibration한 방식이다. target으로는 checkerboard, cardboard로 사용한다.   
   
<img width="586" alt="008" src="https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/b2e07da3-5633-44fe-9ad7-e02ba3afe55a">   
OpenCV에서 제공하는 ArUco Marker를 이용해서도 가능하다. Aruco Marker는 컴퓨터 비전에서 사용되는 미리 정의된 2D 바코드 패턴이다. 
카메라를 통해 쉽게 감지되고, 각각 고유한 ID를 가지고 있어 식별이 가능하다.   

### targetless method

targetless 방식은 target없이 주변 지형지물을 이용해 feature를 추출하는 방법이다.   
motion based 방식, scene based 방식, deep learning 방식 등이 있다는데 사용하진 않을 듯 하다.   

# fusion

calibration 작업을 마쳤다면 객체 판단을 누가 할지도 정해야한다.   

## early fusion

인지처리를 (카메라 혹은 라이다) 한 번만 하기 때문에 연산량이 적다는 장점이 있다.   
그러나 Raw 데이터의 차원이 다르면 융합하기 힘들다는 단점이 있다. (ex: 3D LiDAR와 2D Camera)   \

<img width="581" alt="012" src="https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/221a8fbc-e8ca-442e-b293-7ca618e286b7">   
예시 사진과 같이 LiDAR를 사진에 먼저 투영만 시킬뿐 detection 작업은 따로 하지 않는다. 
camera를 통한 object detection을 하고 이후에 bounding box안에 LiDAR를 그냥 투영시킨다.   

## late fusion

late fusion은 각각의 센서에 대해서 detection을 수행하고, 그 결과를 바탕으로 fusion하는 방식이다.   
오류가 발생한 센서가 어떤 건지에 대해 파악할 수 있고, 결과에 대한 신뢰도 파악이 가능하다.   
하지만 모든 센서들이 인지처리를 해야해서 계산량이 많아진다는 단점이 있다.   

<img width="587" alt="013" src="https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/ced463f9-1bbd-4bb1-9e2a-ee0b143640d0">   

## intermediate fusion

early fusion과 late fusion의 장점들을 합친 방식으로, 각각 센서에 대해서 detection을 하고 fusion 이후에도 detection을 한다. 
둘의 장점을 모두 가지고 있어 좋은 성능을 내지만 계산량이 너무 많다는 단점이 있다.   
