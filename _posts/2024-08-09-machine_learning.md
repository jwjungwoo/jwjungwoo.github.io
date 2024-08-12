---
layout: single
title:  "machine learning"
categories: ai
tag: 
author_profile: false #true면 글 안에서 내 프로필 보여줌
sidebar:
    nav: "counts"
#search: false
---

# 1. colab

colab이란 구글에서 제공하는 클라우드 서비스이다. 클라우드에 있는 컴퓨팅 인스턴스라 부르는 자원에 연결한다.   
브라우저(코딩) >> 구글 클라우드(코드 실행) >> 브라우저 (결과 화면) 느낌으로 이해하면 된다.   
저장은 구글 드라이브에 된다.   
우측상단에 런타임 >> 런타임 유형변경 >> 머신러닝 = none를 우선 선택하면 되며, 나중에 tensorflow를 배울 땐 gpu로 바꾸면 된다. 
tpu는 구글에서 만든 gpu인데.. 그냥 gpu 쓰자.   
![KakaoTalk_20240812_163139420](https://github.com/user-attachments/assets/b9f2e3e1-bef8-470d-8534-78186f215c9a)   
sklearn과 tensorflow는 딥러닝에 사용되는 프레임워크이며 이 글에선 sklearn(싸이킷런)과 tensorflow를 공부한 내용을 기록할 것이다. 

# 2. 마켓과 머신러닝

## 용어

도미와 빙어를 자동으로 구분해주는 모델을 만드려한다.   
![KakaoTalk_20240812_163139420_01](https://github.com/user-attachments/assets/9b6a8f27-8765-4f5f-af97-6409cc91f91d)   
sample: 도미 한마리   
특성: 길이, 무게   

![KakaoTalk_20240812_163139420_02](https://github.com/user-attachments/assets/51c8d124-6b43-4085-8b06-f2d2e6034943)   

## 산점도 (scatter plot)

```python
import matplotlib.pyplot as plt # 맷플로립. 그래프 그리는 라이브러리

# 산점도 그리기
bream_length = [25.4, 26.3, 26.5, 29.0, 29.0, 29.7, 29.7, 30.0, 30.0, 30.7,
                31.0, 31.0, 31.5, 32.0, 32.7, 32.8, 33.0, 33.5, 33.5, 33.5,
                33.5, 34.0, 34.0, 34.6, 34.9, 35.0, 35.0, 35.0, 35.1, 35.2, 
                35.5, 35.9, 36.0, 36.4, 37.0]
bream_weight = [242.0, 290.0, 300.0, 320.0, 340.0, 363.0, 430.0, 450.0, 500.0, 390.0,
                450.0, 500.0, 475.0, 500.0, 500.0, 600.0, 600.0, 700.0, 700.0, 610.0,
                650.0, 575.0, 685.0, 620.0, 680.0, 700.0, 725.0, 720.0, 714.0, 850.0,
                1000.0, 920.0, 955.0, 975.0, 950.0]
smelt_length = [9.8, 10.5, 10.6, 11.0, 11.2, 11.3, 11.8, 11.8, 12.0, 12.2,
                12.4, 13.0, 14.3, 15.0]

smelt_weight = [6.7, 7.5, 7.0, 9.7, 9.8, 8.7, 10.0, 9.9, 9.8, 12.2,
                12.2, 19.7, 19.9, 20.0]
plt.scatter(bream_length, bream_weight)
plt.scatter(smelt_length, smelt_weight)
plt.xlabel('length')
plt.ylabel('weight')
plt.show()
```
   
결과   
<img width="447" alt="그래프결과" src="https://github.com/user-attachments/assets/f253c3f7-e654-4bfb-a620-685fe39003a9">   


## 도미와 빙어 합치기

사이킷런이 기대하는 데이터셋으로 준비   
![KakaoTalk_20240812_163139420_05](https://github.com/user-attachments/assets/946b7322-8546-48dc-a925-c5505e4fde0e)   
   
리스트 내포   
![KakaoTalk_20240812_163139420_06](https://github.com/user-attachments/assets/e44efc97-2e91-43da-bb62-7278f849f7da)   
   
정답 준비   
![KakaoTalk_20240812_163139420_08](https://github.com/user-attachments/assets/414613b9-d904-4dc8-a565-37c28801aa6d)   
   
```python
length = bream_length+smelt_length
weight = bream_weight + smelt_weight
fish_data = [[l,w] for l,w in zip[length, weight]] # 리스트 내포

fish_target = [1]*35 + [0]*14 # 지도학습 # 이진분류에선 보통 찾고싶은걸 1, 아닌걸 0으로 함
```

## k-최근접 이웃

![KakaoTalk_20240812_163139420_09](https://github.com/user-attachments/assets/15ad7fd3-653b-4ddd-aa72-ffe903c31e31)   
k는 판단할 때 볼 주면 sample의 갯수이며 이는 "패턴을 학습한다."라는 표현이 어울린다.   
kn을 보통 머신러닝 모델이라 부름.   
fit 메소드: 두 데이터를 가지고 머신러닝 모델을 훈련함.   
score 메소드: 테스트.   

# 3. 


 







