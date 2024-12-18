---
layout: single
title:  "LiDAR-camera calibration(실전편)"
categories: sensorFusion
tag: 
author_profile: false #true면 글 안에서 내 프로필 보여줌
sidebar:
    nav: "counts"
#search: false
---

# 사진 찍기

show_image.py   

```python
import cv2

def main():
    cap = cv2.VideoCapture(1) # 카메라 장치를 열어 캡쳐 객체를 생성한다.

    cap.set(cv2.CAP_PROP_FRAME_WIDTH, 1280)
    cap.set(cv2.CAP_PROP_FRAME_HEIGHT, 720)

    save_idx = 1 # 저장할 이미지의 인덱스를 초기화함.

    while True:
        ret, frame = cap.read() # 웹캠에서 frame 변수에 이미지를 저장함

        cv2.imshow('Camera', frame) # 이미지 뜸

        key = cv2.waitKey(1) & 0xFF # 키보드 입력을 대기함
        if key == ord('s'):
            filename = 'images/image_{}.png'.format(save_idx) # 파일 이름을 지정함
            save_idx += 1
            cv2.imwrite(filename, frame)
            print('saved!')
        elif key == ord('q'):
            break

    cap.release()
    cv2.destroyAllWindows()

if __name__ == "__main__":
    main()
```

# intrinsic calibration

get_intrinsic_parameter.py   

```python
import numpy as np
import cv2
import glob

wc = 10 # 가로 코너 수
hc = 6 # 세로 코너 수

# 종료 기준
criteria = (cv2.TERM_CRITERIA_EPS + cv2.TERM_CRITERIA_MAX_ITER, 30, 0.001)

# 체커보드의 객체 포인트 준비
objp = np.zeros((wc * hc, 3), np.float32)
objp[:, :2] = np.mgrid[0:wc, 0:hc].T.reshape(-1, 2)

# 실제 세계와 이미지 평면에서 객체 포인트와 이미지 포인트를 저장할 배열
objpoints = []  # 실제 3D 점
imgpoints = []  # 이미지 평면의 2D 점

# 켈리브레이션에 사용할 체커보드 이미지 파일 읽음.
images = glob.glob('images/*.png')
for fname in images:
    img = cv2.imread(fname)
    gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

    # 체커보드 코너 찾음.
    ret, corners = cv2.findChessboardCorners(gray, (wc, hc), None)

    # 코너를 찾았으면 객체 포인트와 이미지 포인트 저장함.
    if ret == True:
        objpoints.append(objp)
        corners2 = cv2.cornerSubPix(gray, corners, (11, 11), (-1, -1), criteria)
        imgpoints.append(corners2)

        # 코너 그려서 보여줌.
        img = cv2.drawChessboardCorners(img, (wc, hc), corners2, ret)
        cv2.imshow('img', img)
        cv2.waitKey(500)

cv2.destroyAllWindows()

ret, mtx, dist, rvecs, tvecs = cv2.calibrateCamera(objpoints, imgpoints, gray.shape[::-1], None, None)

print('내부 파라미터 행렬:', mtx)
print('왜곡 계수 :', dist) # 요즘은 카메라가 잘 나와서 0으로 하면됨.
```

# C:픽셀, L:포인트 가져오기

```python
# LiDAR 포인트는 rviz 상에 Point 불러오는 기능이 있는데 그것을 이용하면 된다. pixel값은 별도로 코드를 작성하는 방식으로 가져와야한다.
```

# extrinsic calibration

get_extrinsic_parameter.py

```python
import numpy as np
import cv2

# 2D-image coordination
points_2D = np.array([ # 픽셀 좌표
    (0, 580),
    (433, 489),
    (915, 565),
    (596, 482),
    (111, 513),
    (1097, 557)




], dtype="double")

# 픽셀값에 대응되는 라이다 포인트 좌표 (x,y,z)
points_3D = np.array([
    (0.9607036709785461, 0.7617925405502319, 0.06174028664827347),
    (1.160376787185669, 0.34440624713897705, 0.14931541681289673),
    (1.1497472524642944, -0.19925804436206818, 0.06323496252298355),
    (1.1689457893371582, 0.15919405221939087, 0.14431604743003845),
    (1.1064852476119995, 0.7151114344596863, 0.11941461265087128),
    (0.6875247955322266, -0.22531066834926605, 0.061700232326984406)




], dtype="double")

# Camera Intrinsic Parameter
cameraMatrix = np.array([[978.01906392, 0, 617.53193012],
                [0, 976.10133873, 356.24646461],
                [0, 0, 1]])
'''
[[978.01906392   0.         617.53193012]
 [  0.         976.10133873 356.24646461]
 [  0.           0.           1.        ]]
'''

# 카메라 matrix의 왜곡 계수. 0으로 해도 무방.
dist_coeffs = np.array([0, 0, 0, 0, 0])

# using solvePnP to calculate R, t
retval, rvec, tvec = cv2.solvePnP(points_3D, points_2D, cameraMatrix,
                                  dist_coeffs, rvec=None, tvec=None, useExtrinsicGuess=None, flags=None)

rvec, _ = cv2.Rodrigues(rvec)
tvec = tvec

print('rvec :', rvec)
print('tvec :', tvec)
```

# 결과

show_image.py

```python
import cv2
import numpy as np

rvec = np.array([[-0.6280287, 0.74892565, 0.21140086], # R행렬 for extrinsic
                 [0.12376181, 0.36432388, -0.92301199],
                 [-0.76828573, -0.55351466, -0.32149425]])

tvec = [[ 0.64914071], # T행렬 for extrinsic
 [-0.11004174],
 [-0.24576798]]

cameraMatrix = np.array([[978.01906392, 0, 617.53193012], # intrinsic 결과
                [0, 976.10133873, 356.24646461],
                [0, 0, 1]])

img_points, jacobian = cv2.projectPoints(
    objPoints, rvec, tvec, cameraMatrix,
    np.array([0, 0, 0, 0], dtype=float))

cap = cv2.VideoCapture(1)

cap.set(cv2.CAP_PROP_FRAME_WIDTH, 1280)
cap.set(cv2.CAP_PROP_FRAME_HEIGHT, 720)
while True:
    ret, frame = cap.read()

    for i in range(len(img_points)):
        # Express Lidar points to image using circle
        cv2.circle(frame, (int(img_points[i][0][0]), int(
        img_points[i][0][1])), 3, (0, 0, 255), 1)
```
