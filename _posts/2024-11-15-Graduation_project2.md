---
layout: single
title:  "졸업 프로젝트(OCR)"
categories: ai
tag: 
author_profile: false #true면 글 안에서 내 프로필 보여줌
sidebar:
    nav: "counts"
#search: false
---

# 사전 준비

## 블로그

참고블로그1: <https://soyoung-new-challenge.tistory.com/m/category/DeepLearning/OCR_> 를   
따라하기 위해 ai-hub에서 관광표지판 데이터셋 중 Quality라는 단어가 붙은 데이터들만 다운받았었다.   
   
참고블로그2: <https://velog.io/@shasha/%ED%95%9C%EA%B8%80-OCR-CRNN-Model-%ED%95%99%EC%8A%B5>   
이분 블로그도 위의 블로그를 참고한 블로근데 두 블로그에서 많은 도움을 얻었다. 정말 감사했다.   

## 깃허브

참고깃주소: <https://github.com/mvoelk/ssd_detectors/blob/master/ssd_data.py>   
윗 블로그를 작성하신 분으로 추정되는 분의 깃허브를 발견했다.   
여기서 ssd_data.py를 저장하였다.   

## VGG Image Annotator

참조블로그: <https://gjghks.tistory.com/65>   
   
1. 다운로드 링크   
  - <http://www.robots.ox.ac.uk/~vgg/software/via/>   
  - 하단에 Downloads 링크가 있다.   
  - 1.0.6 버전을 받아주자. via-1.0.6.zip   
   
2. 압축을 풀고 via.html 실행 (google chrome 활용)   
   
3.  Image -> Load or Add Images 선택   
<img width="1280" alt="vgg0" src="https://github.com/user-attachments/assets/c1526cf4-a76f-44d5-8c07-402fa8d55b62">
   
4. annotation과 class 설정   
<img width="1280" alt="vgg2" src="https://github.com/user-attachments/assets/8ee69248-c1e7-4df8-b46c-da4ec3608f19">
Region Shape에서 Polygon Region Shape 선택 후 annotation하고   
Region Attributes를 누르고 class를 설정한다.   
   
5. 불러온 모든 이미지에 대한 작업을 마치고 저장 Annotation -> Save as Json   
<img width="1280" alt="vgg3" src="https://github.com/user-attachments/assets/f76de7b1-6bd4-4c10-86ff-fe328afc4322">
필자는 저장된 json파일 이름을 vgg_example1이라는 이름으로 바꿨다.   

## GTUtility 생성

GTUtility를 사용해 데이터셋의 바운딩 박스(어노테이션)와 텍스트 라벨을 CRNN 모델에 맞게 정규화하고 정리하는 과정이다. 이를 통해 CRNN 모델 학습에 적합한 입력 형식을 준비한다.   
```python
import numpy as np
import json
import os

from ssd_data import BaseGTUtility

class GTUtility(BaseGTUtility): # GTUtility 클래스는 BaseGTUtility를 상속하며, 한국어 텍스트 데이터셋의 정답 데이터(라벨)와 관련 이미지를 관리한다
    def __init__(self, data_path, quality='high', validation=False, polygon=True, only_with_label=True): # polygon과 only_with_label은 바운딩 박스 형식과 라벨링이 있는 데이터만 불러올지 여부를 제어
        test = False

        self.data_path = data_path
        gt_path = data_path # 정답 라벨 데이터가 포함된 JSON 파일의 경로
        image_path = data_path # 이미지 파일이 저장된 경로  
        #image_path = os.path.join(data_path, 'train')
        self.gt_path = gt_path
        self.image_path = image_path
        self.classes = ['Background', 'Text']

        self.image_names = []
        self.data = []
        self.text = []

        if quality == 'high':
            with open(os.path.join(gt_path, 'K-SignNet_DS_Sign_Annotation_HighQuality.json'),encoding='UTF8') as f:
                gt_data = json.load(f) # 가져온 제이슨 파일은 딕셔너리 타입이다. # gt_data.keys()의 값은 이미지 파일의 이름에 뒤에 특정 숫자가 붙는다.
        elif quality =='mid':
            with open(os.path.join(gt_path, 'K-SignNet_DS_Sign_Annotation_MidQuality.json'),encoding='UTF8') as f:
                gt_data = json.load(f)
        elif quality == 'low':
            with open(os.path.join(gt_path, 'K-SignNet_DS_Sign_Annotation_LowQuality.json'),encoding='UTF8') as f:
                gt_data = json.load(f)
        else:
            print('quality 미입력')

        for img in gt_data.keys(): # 각 이미지에 대해,
            image_name = gt_data[img]['filename'] # 이미지 파일 이름을 추출하고  ex: "IMG_20181022_173137_2560_1440.jpg"
            #print(image_name)

            boxes = [] # 바운딩 박스 좌표(boxes)와
            text = [] # 텍스트(text)를 저장할 리스트를 초기화한다.
            for ann in range(len(gt_data[img]['regions'])): # 각 객체의 gt_type에 따라 박스 타입을 구분한다.
                gt_type = gt_data[img]['regions'][ann]['shape_attributes']['name'] # box값은 rect 타입과 polygon 타입으로 구성.

                if gt_type == "rect": 
                    x = gt_data[img]['regions'][ann]['shape_attributes']['x']
                    y = gt_data[img]['regions'][ann]['shape_attributes']['y']
                    w = gt_data[img]['regions'][ann]['shape_attributes']['width']
                    h = gt_data[img]['regions'][ann]['shape_attributes']['height']

                    #box = np.array([x,y, x+w, y+h], dtype=np.float32)
                    #box = np.array([x,y,x,y+h,x+w,y+h,x+w,y], dtype=np.float32)
                    box = np.array([x,y,x+w,y,x+w,y+h,x,y+h], dtype=np.float32) # 왼쪽 상단부터 시계 방향으로. (현재 COCO-Text의 boxes의 값과 동일하게 맞춰주기 위해서)

                elif gt_type == "polygon": # for 4points
                    xlist = gt_data[img]['regions'][ann]['shape_attributes']['all_points_x']
                    ylist = gt_data[img]['regions'][ann]['shape_attributes']['all_points_y']

                    if len(xlist)!= 4:
                        continue

                    xy= []
                    xy = [xlist[0]]+ [ylist[0]]+ \
                         [xlist[3]]+ [ylist[3]]+ \
                         [xlist[2]]+ [ylist[2]]+ \
                         [xlist[1]]+ [ylist[1]]
                    #여기서 박스의 모양:
                    #왼쪽 상단부터 시계 방향으로.

                    '''for x, y in zip(xlist, ylist):
                        xy = xy + [x] + [y]'''

                    box = np.array(xy, dtype=np.float32)
                else:
                    continue

                if 'annotation_kr' in gt_data[img]['regions'][ann]['region_attributes'].keys():
                    txt = gt_data[img]['regions'][ann]['region_attributes']['annotation_kr']
                else:
                    if only_with_label:
                        continue
                    else:
                        txt = ''

                boxes.append(box)
                text.append(txt) # label text 값이 들어간다 # 모델 학습 시 텍스트 검출 또는 OCR 작업을 위한 정답 데이터로 사용

            if len(boxes) == 0:
                #print("No Bounding Box !")
                continue

            boxes = np.asarray(boxes)
            #print(boxes.shape)
            # boxes 값을 정규화
            # boxes 값의 x값들은 이미지의 width로 나눠준다 (0과 1 사이로 맞춰줌)
            # boxes 값의 y값들은 이미지의 height로 나눠준다 (0과 1사이로 맞춰줌)
            boxes[:,0::2] /= 2560 # 0::2는 모든 행에서 짝수 인덱스(0번째, 2번째, 4번째 등)에 있는 값을 의미함. 이는 바운딩 박스 좌표 배열에서 x 좌표들에 해당함. 모든 x 좌표 값을 이미지의 너비인 2560으로 나눠줌
            boxes[:,1::2] /= 1440

            boxes = np.concatenate([boxes, np.ones([boxes.shape[0],1])], axis=1)

            self.image_names.append(image_name)
            self.data.append(boxes)
            self.text.append(text) 

        self.init()

if __name__ == '__main__':

    gt_util = GTUtility('data/Directly_Captured_Signs_Annotation_HighQuality_new/HighQuality', polygon=True, only_with_label=True)

    import pickle

    file_name = 'gt_util_ksigntext.pkl'
    print('save to %s...' % file_name)
    pickle.dump(gt_util, open(file_name,'wb')) # 'wb': write binary
    print('done')

    print(gt_util.data)
```
<img width="497" alt="gtu머시기 py실행결과" src="https://github.com/user-attachments/assets/7b371abc-e3af-4e03-9190-864c982693a1">   

## practice

아까 저장한 두 사진의 형식을 보기위해 연습파일을 만들었다.   
   
코드1   
```python
import json

with open('vgg_example1.json',encoding='UTF8') as f:
    ksign = json.load(f)

print(ksign.keys())
```   
실행결과1   
<img width="1241" alt="practice1" src="https://github.com/user-attachments/assets/97c1ab73-ddad-461e-b4e6-4130b9504ae7">   
   
코드2   
```python
import json

with open('vgg_example1.json',encoding='UTF8') as f:
    ksign = json.load(f)

print(ksign['lp001.jpg7274'])
```   
실행결과2   
<img width="1240" alt="practice2" src="https://github.com/user-attachments/assets/ba5789d6-ec81-4374-bfbd-aa882fd47c60">   
보기 쉽게 풀어쓰면   
```java
{
  'fileref': '',
  'size': 7274,
  'filename': 'lp001.jpg',
  'base64_img_data': '',
  'file_attributes': {},
  'regions': {
    '0': {
      'shape_attributes': {
        'name': 'rect',
        'x': 41,
        'y': 95,
        'width': 78,
        'height': 28
      },
      'region_attributes': {
        'class': 'text'
      }
    },
    '1': {
      'shape_attributes': {
        'name': 'rect',
        'x': 127,
        'y': 95,
        'width': 72,
        'height': 27
      },
      'region_attributes': {
        'class': 'text'
      }
    }
  }
}
```
이다.   

## 누,하

이번엔 '누'와 '하'라는 글씨까지 포함된 파일을 annotation안 한 상태로 전체 데이터셋으로 저장해봤다.   
![KakaoTalk_20241114_170318987](https://github.com/user-attachments/assets/808b7d97-5bb9-4193-be9f-8b0f6cf6c85d)   
![KakaoTalk_20241114_170318987_01](https://github.com/user-attachments/assets/e04401e4-3af7-441f-bbb1-d54d6291b457)   

이미지증강기법을 이용할 때 rotation을 사용하고 싶으나 rotation을 하면 annotation의 정확도가 떨어질 것으로 예상돼 '123가 4567' 같은 번호판에서 
'1', '2', '3', '가', '4', '5', '6', '7'로 떼어내어 분리하고 증강시키고, annotation은 사진 전체가 되게하려했다. 이때 사진 크기가 서로 다르면 
width,height 값이 들쭉날쭉해서 통제하기 힘든 상황이 생길 수도 있을 것 같아 우선 사진의 크기를 일정 크기로 고정시키는 작업이 필요하다 생각했다.   
