---
layout: single
title:  "졸업 프로젝트(개요)"
categories: project
tag: 
author_profile: false #true면 글 안에서 내 프로필 보여줌
sidebar:
    nav: "counts"
#search: false
---

# AI 용어

## 데이터 형식

bounding box : 좌상단부터 시계 방향으로 객체를 박스침   
segmentation : 객체를 픽셀로 표현함. 선으로 그어놨다고 보면됨. bounding box보다 더 정확함   
keypoint : 객체 내의 중요한 지점(특징점)을 표현함. segmentation의 축소 버전이라 보면 됨   

# 데이터 증강

## 사용 이유

ocr 을 사용하여 번호판을 읽어오는 모델을 개발하는데 있어 필요한 클래스는 자동차, 오토바이, 번호판인데 이에 맞는 라벨링이 되어 있는 데이터가 부족하여 그것을 해결하기 위해 사용하기로 정하였다.   

훈련 데이터 세트에 대해 과적합 (overfitting)   
적은 훈련 세트에 대해 학습을 시키면 훈련 세트에 대해 과하게 학습되어   
테스트 세트에서의 정확도가 떨어질 수 있다.   
모의고사에 너무 익숙해져서 실제 수능에서 성적이 낮게 나오는 경우와 비슷하다.   
   
불균형 데이터 (imbalanced data)   
특정 클래스에 대한 훈련 데이터가 적을 때 발생하는 문제이다.   
예를들어 사과 데이터가 99개, 오렌지 데이터가 1개이면   
모든 데이터를 사과로 분류하는 바보같은 모델이더라도   
훈련 세트에서의 정확도가 99%가 되는 문제가 발생할 수 있다.   
   
위의 문제들을 해결할 수 있는 방법이 바로 데이터 증강(Data Augmentation)이다.   

## 데이터 증강 종류

1) 이미지 데이터 증강   
   
이미지에 약간의 변형을 주는 방법으로 데이터를 증강시킬 수 있다.   
   
- 좌우 뒤집기   
- 채도 변경   
- 밝기 변경   
- 시계방향 회전하기   
- 이미지 자르기   
- 노이즈 삽입   
   
![examples](https://github.com/user-attachments/assets/d2b73199-eb9f-48ed-af50-0bf123656a4a)   

2) 텍스트 증강   
   
데이터에 약간의 변형을 주는 이미지 증강과 달리 텍스트는 한 단어만 바꿔도 문장의 의미가 달라질 수 있으므로 데이터 증강이 어렵다고 알려져있다. 그럼에도 아래의 방법으로 데이터를 증강시킬 수 있다.   
   
- text를 다른 언어로 번역한 후 다시 원래 언어로 번역   
- 특정 단어를 유의어로 교체   
- 임의의 단어를 삽입 or 삭제   
- 문장내 두 단어의 위치를 임의로 바꾸기   
   
마지막 두 방법의 경우 의외로 의외로 원본 문장의 라벨을 대체로 잘 따른다는 결과가 나왔다.   
   
3) 시계열 데이터 증강   
   
3-1) 시계열 데이터란?   
   
시계열 데이터 증강을 이해하기 전에 시계열 데이터에 대해 이해할 필요가 있다.   
시계열 데이터란, 일정 시간 간격으로 배치된 데이터들을 말한다.   
ex. 일별 주가, 분당 심박동 수   
시계열의 영어 표현인 time series를 생각하면 이해하기 편하다.   
시계열 데이터를 통해 ‘시계열 분류’와 ‘이상치 탐지’라는 작업를 수행할 수 있다.   
   
시계열 데이터는 시간 영역과 주파수 영역으로 나누어 분석할 수 있다.   

시간 영역 (time domain)   
일정 기간 동안의 데이터를 분석하는 것 ( = 변수를 시간에 대해 측정)   
ex. 시간 영역에서의 전기 신호 분석 : 오실로스코프   
   
주파수 영역 (frequency domain)   
주파수에 대한 수학적 함수 또는 신호를 주파수와 관련하여 분석하는 것   
푸리에 변환 (시간 영역 → 주파수 영역)   
   
시간 도메인에서의 연속적 신호를 여러 sin 곡선들의 합으로 나타내어   
주파수 도메인으로 변환할 수 있음   
cf. 역 푸리에 변환 : 주파수 영역 → 시간 영역   

3-2) 시계열 데이터 증강의 어려움   
   
시계열 데이터 증강에는 다음과 같은 어려움이 있다.   
   
첫번째, 현재의 데이터 증강 기법은 시간에 종속적(temporal dependency)이라는 시계열의 본질적 특성을 활용하지 못한다.   
두번째, 증강 기법이 데이터를 왜곡하거나 새로운 패턴을 만들어내면 오히려 이상치를 더 찾기 어렵게 만들 수 있다.   
   
따라서 단순히 기존의 데이터 증강을 적용하는 것은 효과적이지 않으므로   
시계열 데이터를 위한 증강 방법을 고안할 필요가 있다.   

3-3) 시계열 데이터 증강의 분류   
   
![series_data](https://github.com/user-attachments/assets/4ba6c322-e2e4-4eb6-b867-e264f70712df)   
논문 [Time Series Data Augmentation For Deep Learning : A Survey] 에서는 시계열 데이터 증강 기법을 위와 같이 분류하고 있다. 본 글에서는 이중 가장 활용도가 높은 Time Domain에서의 시계열 데이터 증강 기법을 정리하고자 한다.   

3-4) Time Domain에서의 증강 기법   

Window cropping or slicing   
원본 데이터에서 랜덤하게 연속적인 slice를 추출하는 방법이다.   
CV에서 이미지를 랜덤하게 잘라 데이터를 증강하는 것과 유사하다.   
   
Window warping   
랜덤한 구간을 선정하여 압축 or 확장하는 방법이다. (DTW와 유사함)   
원본 시계열의 전체 길이를 바꿀 수 있다.   
   
Flipping   
원본 데이터의 부호를 바꿔 새로운 시퀀스를 만드는 방법이다.   
부호를 바꿔도 라벨은 동일하다. (시계열 분류, 이상치 탐지 둘 다 해당됨)   
   
Noise injection   
라벨을 바꾸지 않은 채 원본 데이터에 노이즈를 삽입하는 방법이다.   
가우시안 노이즈(정규분포를 갖는 잡음) 또는   
spike, step-like trend, and slope-like trend 등의 노이즈를 삽입할 수 있다.   
Label expansion 이상치 감지를 위한 증강 기법이다.시계열 데이터에서 이상치는 연속적으로 발생하기 때문에 시작점과 끝점이 모호하다. 따라서 시간 거리와 값 거리가 이상치와 가까운 데이터는 이상치일 가능성이 높다. 이러한 데이터를 이상치로 분류하여 라벨을 확장할 수 있다.   

## 사용할 증강기법

이미지 증강 중 tensorflow에서 제공하는 기법 중 tf.image메서드를 사용할 것이다.   

## rotation의 문제점
데이터 증강과 관련해서 좋은 블로그를 찾아서 여기서 코드를 공부했다.   
<https://developeryoung.tistory.com/6>   
rgb값을 바꾸기 전 rotation과 flip 기술을 이용하려했다. 먼저 colob에서 rotation을 이용해 image와 label 파일을 새롭게 만드는 과정에서 rotation된 라벨이 제대로 된 라벨인지 확인했는데 자동차 크기에 제대로 맞지 않았다.   
before_rotation   
![output (1)](https://github.com/user-attachments/assets/a8252dac-2716-42b2-9d07-1c71358d9d87)   
after_rotation   
![output](https://github.com/user-attachments/assets/8a767639-d015-4caf-85f1-ced18c98eb66)   

그 원인을 그림 그려 찾으려했는데 아인슈타인처럼 전두엽 주름을 깊게 만들고자 머리속에서 생각해봤는데 그 이유는 다음 그림과 같다.   
![Screenshot_20240925_170215_Flexcil](https://github.com/user-attachments/assets/50cfa395-7609-49cf-8e48-2b3e60dfe326)   
rotation된 BB는 기존의 BB를 핏하게 담지만 차량을 핏하게 담는 건 아니다. 따라서 노이즈 이슈로 rotation은 안 쓰기로 했다.


코드는 적어두겠다.   
```python
import imgaug as ia # 이미지 증강 library
import imgaug.augmenters as iaa # 다양한 증강기
import imageio.v2 as imageio # 이미지 입출력
from imgaug.augmentables.bbs import BoundingBox, BoundingBoxesOnImage
import cv2 # 이미지 처리를 위함
from google.colab.patches import cv2_imshow # google colab에서 이미지를 보여주기 위한 패치(cv2_imshow) 임포
from os import listdir # 디렉토리의 파일 리스트를 얻기 위한 함수 임포트
import os

from google.colab import drive
drive.mount('/content/drive')
a = 0
# 경로 확인
path = '/content/drive/My Drive/before_aug/car/images/train/'
files = os.listdir(path)

# print(os.path.exists(path))  # 경로가 존재하면 True, 존재하지 않으면 False 출력

ia.seed(1)
for file in files: # 해당 경로에 있는 모든 파일에 대해
  a+= 1
  image = cv2.imread(path+file) # 파일 경로를 이용해 이미지 파일을 읽어옴

  # ex) image1.jpg 라는 파일이 있으면, 그와 동일한 이름의 라벨 파일이 image1.txt 라는 이름으로 존재한다고 가정함
  labelfile = file.replace('.jpg','.txt') # 이미지 파일의 확장자를 '.txt'로 바꾸어 라벨 파일명 생성

  f = open('/content/drive/My Drive/before_aug/car/labels/train/'+labelfile) # 사진에 맞는 라벨 파일 열기
  boundingboxes = f.readlines() # 라벨 파일에서 바운딩 박스 정보를 한 줄씩 읽어 리스트로 저장

  for i in range(len(boundingboxes)): # 각 바운딩 박스에 대해 반복문 실행하여 string을 int, float로 바꿔줌
    boundingboxif = boundingboxes[i].split(" ")
    for j in range(len(boundingboxif)):
      if boundingboxif[j] == '0': # 자동차
        boundingboxif[j] = 0
      elif boundingboxif[j] == '1': # 오토바이
        boundingboxif[j] == 1
      elif boundingboxif[j] == '2': # LP
        boundingboxif[j] == 2
      else:
        x = float(boundingboxif[j])
        boundingboxif[j] = x
    boundingboxes[i] = boundingboxif
  # 바운딩박스의 [x1, y1, x2, y2]를 담는 리스트를 만듦
  boundingbox_newif = []

  for i in range(len(boundingboxes)):
    x_center = boundingboxes[i][1]
    y_center = boundingboxes[i][2]
    height = boundingboxes[i][4]*640
    width = boundingboxes[i][3]*640

    x_center_new = x_center*640
    y_center_new = y_center*640

    x1 = (x_center_new-width/2)
    y1 = (y_center_new-height/2)
    x2 = (x_center_new+width/2)
    y2 = (y_center_new+height/2)

    boundingbox_newif.append([x1,y1,x2,y2]) # 이 리스트엔 라벨값은 안 들어감

  ia_bounding_boxes = []
  if image is None:
    print("Error loading image: {file}")

  for box in boundingbox_newif:
    ia_bounding_boxes.append(ia.BoundingBox(x1 = box[0], y1 = box[1], x2 = box[2], y2 = box[3])) # BoundingBox 객체 생성 후 리스트에 추가
  # bbs는 이미지 위에 어떤 위치에 바운딩 박스가 존재하는지를 정의한 객체임
  # 이 bbs 객체는 이후 이미지에 변환(예: 회전, 스케일링 등)을 적용할 때 바운딩 박스들도 함께 변환할 수 있도록 도와줌
  bbs =ia.BoundingBoxesOnImage(ia_bounding_boxes, shape = image.shape) # 이미지의 크기에 맞는 BoundingBoxesOnImage 객체를 생성하여 바운딩박스 리스트와 이미지 모양(shape)을 연결

  seq = iaa.Sequential([iaa.Affine(rotate = 20)]) # 이미지에 20도 회전을 적용하는 Affine 변환을 정의한 시퀀스(증강 파이프라인)를 생성

  # Augment images and BBs
  image_aug, bbs_aug = seq(image = image, bounding_boxes = bbs) # image_aug: 증강된 이미지, bbs_aug: 증강된 바운딩박스
  print(bbs_aug)
  # 객체의 위치가 변하기에 label파일의 BB의 값도 변경해서 저장해야한다.
  cv2.imwrite('/content/drive/My Drive/after_rotation/car/images/train/' + "rotate__" + file, image_aug) # 회전된 이미지 저장

  # 회전된 바운딩 박스 좌표 가져오기
  after_bounding = []
  my_i = 0
  for bb_aug, original_box in zip(bbs_aug.bounding_boxes, boundingbox_newif): # bbs_aug.bounding_boxes: 이미지 증강(회전) 후 변형된 바운딩 박스들
    x1_aug, y1_aug, x2_aug, y2_aug = bb_aug.x1, bb_aug.y1, bb_aug.x2, bb_aug.y2
    # YOLO 형식으로 변환 (x_center, y_center, width, height) 이미지 크기 비율 기준으로 계산
    x_center_aug = (x1_aug + x2_aug) / 2 / 640
    y_center_aug = (y1_aug + y2_aug) / 2 / 640
    width_aug = (x2_aug - x1_aug) / 640
    height_aug = (y2_aug - y1_aug) / 640

    # 클래스 ID와 함께 추가
    after_bounding.append([int(boundingboxes[my_i][0]), x_center_aug, y_center_aug, width_aug, height_aug])
    my_i = my_i + 1

  f = open('/content/drive/My Drive/after_rotation/car/labels/train/' + "rotate__" + labelfile, "w") # 회전 정보를 담은 label파일 저장
  for i in range(len(after_bounding)):
    for j in range(5): # 각 바운딩 박스의 5개의 값 (클래스, )
      if j == 4: # 마지막 값일 때
        f.write(str(after_bounding[i][j])) # 줄바꿈 없이 마지막 값을 파일에 기록
      else:
        f.write(str(after_bounding[i][j]) + " ") # 값 뒤에 공백을 추가하고 기록
      print(str(i) + " : " + str(after_bounding[i][j]))
    f.write("\n")

  f.close()
  print(a)
```

## horizontal flip
rotation코드에서 
```python
seq = iaa.Sequential([iaa.Affine(rotate = 20)]) # 이미지에 20도 회전을 적용하는 Affine 변환을 정의한 시퀀스(증강 파이프라인)를 생성
```
부분을
```python
  seq = iaa.Sequential([iaa.Fliplr(1.0)]) # 좌우 반전을 적용하는 Affine 변환을 정의한 시퀀스(증강 파이프라인)를 생성
```
로 바꾸면 끝이다( + 파일명만 바꾸기).

# paddle OCR
설치 명령어
```java
pip install paddlepaddle
pip install paddleocr
```
   
숫자 영어 인식 코드
```python
import cv2
from paddleocr import PaddleOCR, draw_ocr
import numpy as np

# PaddleOCR 초기화
ocr = PaddleOCR(use_angle_cls=True, lang='en')  # English OCR

# 카메라 초기화
cap = cv2.VideoCapture(0)

stop_conf = 0

while True:
    # 프레임 읽기
    ret, frame = cap.read()
    if not ret:
        print("카메라에서 영상을 가져올 수 없습니다.")
        break

    try:
        # PaddleOCR로 텍스트 인식
        result = ocr.ocr(frame, cls=True)

        # 결과가 None이 아니고, 결과가 있는 경우 처리
        if result is not None and len(result[0]) > 0:
            for line in result[0]:
                # 인식된 텍스트와 위치 정보 가져오기
                box = line[0]  # 텍스트의 바운딩 박스 좌표
                text = line[1][0]  # 인식된 텍스트
                score = line[1][1]  # 인식 신뢰도
                score_vision = round(score,2)

                # 바운딩 박스 그리기
                box = np.array(box).astype(np.int32)
                cv2.polylines(frame, [box], isClosed=True, color=(0, 255, 0), thickness=2)

                # 인식된 텍스트를 바운딩 박스의 왼쪽 상단에 표시
                text_position = (box[0][0], box[0][1] - 10)  # 왼쪽 상단 좌표, 상단으로 약간 올림
                score_position = (box[0][0], box[0][1] - 30)
                cv2.putText(frame, f'{text}', text_position,
                            cv2.FONT_HERSHEY_SIMPLEX, 0.7, (255, 0, 0), 2, cv2.LINE_AA)
                cv2.putText(frame, f'score : {score_vision}', score_position, cv2.FONT_HERSHEY_TRIPLEX, 0.7, (255, 0, 0), 2, cv2.LINE_AA)

                if text == 'SAMSUNG' and score > 0.7:
                    stop_conf += 1

        if stop_conf == 3:
            break

        print(stop_conf)

    except Exception as e:
        print(f"오류 발생: {e}")

    # 화면에 프레임 보여주기
    cv2.imshow('Camera', frame)

    # 'q' 키를 누르면 종료
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

# 카메라와 윈도우 해제
cap.release()
cv2.destroyAllWindows()
```

# YOLO

## 평가표
<img width="536" alt="스크린샷 2024-09-13 053027" src="https://github.com/user-attachments/assets/06794d89-be95-4b90-93a2-5b10e58b6528">   
Model: 'n'은 Nano, 's'는 Small, 'm'은 Medium, 'l'은 Large, 'x'는 Extra-Large를 의미하며, 각 모델은 크기와 성능에서 차이가 있다.   
Size (pixels): 입력 이미지의 해상도를 나타낸다. 여기서는 모든 모델에서 640 픽셀 크기의 이미지를 사용합니다.   
mAPᵥₐₗ 50-95: "mean Average Precision"의 줄임말로, 모델의 정확도를 평가하는 지표이다. 0.5에서 0.95까지의 IoU(Intersection over Union) 임계값을 기반으로 평균 정확도를 나타낸다. 값이 클수록 모델이 더 정확하게 객체를 탐지한다.   
Speed CPU ONNX (ms): CPU에서 ONNX(개방형 신경망 교환 형식) 형식을 사용하여 모델이 추론하는 속도를 나타낸다. 밀리초(ms) 단위로 측정되며, 값이 작을수록 속도가 빠르다.   
Speed A100 TensorRT (ms): NVIDIA A100 GPU에서 TensorRT(엔비디아의 고성능 추론 라이브러리)를 사용할 때의 추론 속도를 나타낸다. 마찬가지로 밀리초(ms) 단위로 측정되며, 값이 작을수록 속도가 빠르다.   
Params (M): 모델의 파라미터 수를 백만(M) 단위로 나타낸다. 파라미터 수가 많을수록 모델이 더 복잡하고 메모리를 많이 사용한다.   
FLOPs (B): 모델이 연산할 때 요구되는 부동소수점 연산 수(Floating Point Operations)를 10억 단위로 나타낸다. 연산량이 많을수록 성능이 강력하지만 속도와 메모리 사용량이 증가할 수 있다.   
<https://docs.ultralytics.com/models/yolov8/#supported-tasks-and-modes>

## 사용할 버전

자동차가 시속 30km/h로 달린다고 가정했을 때 이는 8.3m/s이며 0.1초당 대략 1m를 이동하는 셈이다.   
즉, 불법주정차 단속구간에서 적절한 속도로 달렸을 때 0.1초당 1m를 넘게 갈 case가 많을 것으로 판단되어 yolo 모델의 speed가 빠른 모델 즉, 비교적 light한 모델을 사용해야할 것으로 판단해 우선 적당히 똑똑해보이는 YOLOv8s를 사용하기로 결정했다. 다만 데이터량에 자신이 없는 관계로 overfitting될 수 있기에 yolov5도 학습시켜볼 생각이다.   
<img width="294" alt="스크린샷 2024-09-13 054710" src="https://github.com/user-attachments/assets/d21abcae-0894-4e8e-90df-3fed7d7fc32a">   





