---
layout: single
title:  "introduction to SLAM"
categories: slam
tag: 
author_profile: false #true면 글 안에서 내 프로필 보여줌
sidebar:
    nav: "counts"
#search: false
---

# 슬램이란?

SLAM (Simultaneous Localization And Mapping)   
로보틱스의 세 가지 요소인 인지-판단-제어 중 슬램은 인지에 해당함.   

<img width="157" alt="001" src="https://github.com/user-attachments/assets/10a4ac20-1722-49f2-92e6-4553c4a6d8d6">   

# 슬램에 사용되는 하드웨어

\<내부\>   
wheel encoder: 이동량을 측정하는 센서   
<img width="190" alt="002" src="https://github.com/user-attachments/assets/fd10748b-6634-4cc2-8995-5cd128a88e62">   
사진에서 보여지는 것처럼 LED가 있고 바퀴가 돌면서 센서가 LED를 감지한다.   
   
이동량 = 바퀴의 회전량(RPM)*바퀴의 둘레   
단점: Drift, 바퀴 헛돌기에 취약 (누적됨)   
   
그러나 wheel 사용 안 하고 LiDAR로만 모션을 추정할 수 있음.   
<img width="247" alt="005" src="https://github.com/user-attachments/assets/13b25e01-ecf4-4f0d-83cd-21a0ac210726">   
   
IMU: 차량의 속도, 가속도, 회전 각속도 등을 측정   
<img width="182" alt="003" src="https://github.com/user-attachments/assets/615c29f9-bb4b-4b5a-965b-96ac93531c86">   
단점: 오차 누적   
   
따라서 slam에선 카메라나 라이다로 오차 줄이는 방법을 이용함.   

\<외부\>
GNSS   
<img width="255" alt="004" src="https://github.com/user-attachments/assets/fe53ed50-0a3c-4a76-a417-7db7cc66e9a5">   
나라마다 시스템이 다르다. GPS(USA), GLONASS(Russia), ...   

# SLAM의 종류

## VSLAM

visual slam (오류 나면 리셋하고 다시해야함.. 단, visual-inertial/-wheel odometry 제외)   
장점: 센서 성능 조정 쉬움, 빠른 센서 속도, 딥러닝 적용 쉬움   
단점: 갑작스런 빛 변화   
   
VSLAM의 종류   
<img width="533" alt="006" src="https://github.com/user-attachments/assets/ac9b2891-426f-444c-9329-c454e4ecd243">   
   
monocular VSLAM: 단점: 3차원 공간을 실제 비율을 추정할 수 없음--> 이 문제를 풀기위해 IMU, Whell encoder 등 사용   
monocular VSLAM 중 DSO, LSD-SLAM, ORB-SLAM중 사용할거면 DSO를 사용해야함.   
   
stereo/multi camera VSLAM: 3차원 공간 복원 및 depth 추정 가능   
단점: 정교한 설정 및 캘리브레이션 과정이 필요. 정확한 캘리브레이션은 어려움. GPU 성능 좋아야함.   
   
RGB-D VSLAM: 외부사용불가   
   
Visual-inertial 또는 Visual-wheel odometry   
VSLAM 중 위치/지도 연산을 수행할 때, motion 정보로써 IMU 또는 wheel의 값을 함께 사용하는 방법. 생각보다 정확함.   
단점: 어려움.   

## LiDAR SLAM

센서 속도가 느림(10-20Hz)   
   
2D, 3D, LiDAR-inertial/-wheel SLAM이 있는데 LiDAR-inertial/-wheel SLAM (LIO-SLAM)은 
motion정보도 가져옴으로써 de-skew 작업이 쉬워짐. (=차가 이동하면서 물체 정보를 가져올 때 움직이면서 라이다가 측정하기에 실제 길이보다 길어보일 수 있는걸 motion정보를 가져옴으로써 수정할 수 있음)   
단점: 캘리브레이션이 어려움.   

# SLAM의 사용처

Driving,   
-highway driving, urban driving, urban parking(비어있는 공간을 찾아 주차해야함)   
Parking,   
HD-Map   
<img width="407" alt="007" src="https://github.com/user-attachments/assets/090d4409-36d3-4f6c-a6d9-492ef887c2de">   
슬램으로 나라 단위로도도 맵을 딸 수 있다는건 놀라웠다. 다만 유지보수에 매우 많은 비용이 들 것 같았다. 쓰는 진영과 안 쓰는 진영으로 나뉘는데 
테슬라의 경우엔 HD-Map을 사용하지 않는다는 것도 신기했다. 어떤 기술들을 이용하는 걸까?   
