---
layout: single
title:  "최종 플젝"
categories: autoever
tag: 
author_profile: false #true면 글 안에서 내 프로필 보여줌
sidebar:
    nav: "counts"
#search: false
---

# vector 현직자분들

## 자동차 관련 회사들

HMC, 쌍용, HL만도, 클래무브, 모비스(인포, ADAS, 왠만한거 다함), 케피코(제어기 만듦), 위아, 트랜시스, SL(대구_경산), LG(인포), LS 엠트론(트랙터 만듦)

## 지식

✅ vECU(가상 ECU)   
시간과 비용을 줄이려고 사용   
   
✅ MBD(Model-Based Design)   
우리가 저번에 사용한 matlab 이 대표적인 예시   
   
✅ vehicle OS   
하드웨어는 변경 되더라도 상위 SW는 사용될 수 있게함   
   
✅ FUSA(퓨사)   
Functional Safety. “시스템이 고장 나더라도 사람이나 환경에 위험을 주지 않도록 설계하는 방법론 및 표준” 이 안에 ASIL, HARA, Watchdog, 정적테스트 등이 있음   
   
✅ Zonal Architecture   
이것 덕분에 전선량이 66퍼센트 줄었다.   
   
✅ HPC   
High Performence Computer_ 그렇게 해야 SW Defined할 수 있다. 몰빵해서 해야 제어기 숫자도 줄고 업데이트도 좋다. 또한 데이터를 몰빵해서 해야 제어기 숫자도 줄고 업데이트도 좋다.   

## 프로젝트의 목표

✅ 통신   
ECU 간에 필요한 정볼르 주고 받으며 제어 및 안전한 주행, 효율적인 동작을 달성함   
통신시스템에서 사용하는 메시지와 Communication Matrix는 DB에 정의되어 있음   
   
✅ 목표   
차량 환경을 시뮬레이션으로 구성하고, 차량 시스템 개발 프로세스를 적용하여 차량 기능을 구현해 보는 것이 본 프로젝트의 목적   

# OTA

## ota 서버 개발 순서

```c
1. wsl --install 로 우분투 설치. + 컴퓨터 재부팅
2. ubuntu 앱 실행
3. 파이썬 설치
$ sudo apt get update
$ sudo apt install python3-pip

4. 가상환경 설치
$ sudo apt install python3.12-venv

5. 가상환경 접근 후 flask 설치
$ python3 -m venv venv
$ source venv/bin/activate
$ pip install flask

6. 잘 설치됐는지 확인
$ which python
# → /home/jw/venv/bin/python 이어야 정상
$ which pip
# → /home/jw/venv/bin/pip 이어야 정상

7. ota_server.py 만들기
$ nano ota_server.py

8. ota_server.py 코드

from flask import Flask, send_file, jsonify
import os

app = Flask(__name__)
LATEST_VERSION = "v1.2.0"
SW_FILE = "latest_sw.bin"

@app.route('/check_update')
def check_update():
    return jsonify({"version": LATEST_VERSION})

@app.route('/download')
def download_file():
    return send_file(SW_FILE, as_attachment=True)

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)

그리고 ctrl+o, ctrl+x 인데 잘 안되네.. 중간에 엔터 눌렀던거같음
또한 latest_sw.bin 파일을 만들어두면됨
```

## ota 서버 개발 결과

![ota](https://github.com/user-attachments/assets/4e8e7806-379f-4c04-a20e-0e33f0b1bdd8)   
이렇게 되면 현재 두 가지 주소로 접속 가능하다.   
```c
1. 로컬 테스트용
http://127.0.0.1:5000
→ 같은 컴퓨터 안에서만 접근 가능

2. 외부 장비(HPC등) 접속용
http://172.19.206.171:5000
→ HPC에서 이 주소로 접속하면 됨
```

## ota hpc 개발 과정

우분투 터미널을 하나 더 만든다.   
```c
1. ota_hpc dir 설정
$ mkdir ~/ota_hpc
$ cd ~/ota_hpc
$ pip install requests

2. 파티션 만들기
$ mkdir -p ~/ota_hpc/a_partition
$ mkdir -p ~/ota_hpc/b_partition
$ echo "v1.0.0" > ~/ota_hpc/b_partition/version.txt

3. 파일 저장
$ nano ota_client.py

import requests
import os

# 서버 IP 주소 (같은 WSL이면 localhost 또는 127.0.0.1)
SERVER_IP = "127.0.0.1"
PORT = 5000

# 저장 위치: a 파티션 역할
DOWNLOAD_PATH = "/home/jw/ota_hpc/a_partition/latest_sw.bin"
CURRENT_VERSION_FILE = "/home/jw/ota_hpc/b_partition/version.txt"

def get_current_version():
    if not os.path.exists(CURRENT_VERSION_FILE):
        return "v0.0.0"
    with open(CURRENT_VERSION_FILE, "r") as f:
        return f.read().strip()

def check_for_update():
    try:
        resp = requests.get(f"http://{SERVER_IP}:{PORT}/check_update")
        return resp.json()["version"]
    except Exception as e:
        print("업데이트 확인 실패:", e)
        return None

def download_update():
    try:
        resp = requests.get(f"http://{SERVER_IP}:{PORT}/download")
        os.makedirs(os.path.dirname(DOWNLOAD_PATH), exist_ok=True)
        with open(DOWNLOAD_PATH, "wb") as f:
            f.write(resp.content)
        print("📦 업데이트 파일 다운로드 완료:", DOWNLOAD_PATH)
    except Exception as e:
        print("❌ 다운로드 실패:", e)

if __name__ == "__main__":
    current = get_current_version()
    latest = check_for_update()

    print(f"🔎 현재 버전: {current}")
    print(f"🌐 서버 최신 버전: {latest}")

    if latest and current != latest:
        print("⬇️ 업데이트 필요. 다운로드 시작...")
        download_update()
    else:
        print("✅ 최신 상태입니다. 업데이트 불필요.")

4. partition a로 다운
서버에서 코드 실행된 상태에서 hpc 실행하면 a partition 에 코드 저장됨.

5. 
```

# Flash_Programming 예제 코드

## writeProgramFlash

1. 실행 중인 PFLASH에 코드를 덮어쓰지 않도록 방지   
AURIX TC275는 플래시 메모리(PFLASH)에 저장된 코드에서 실행된다. 그런데 플래시 쓰기/지우기 작업 중에는 플래시를 동시에 실행할 수 없다. 
만약 플래시에 있는 함수를 이용해 PFLASH를 지우거나 쓰면, 그 순간 함수 코드가 날아가면서 Trap 이 발생한다. 그래서 플래시에 있는 함수들을 RAM(PSPR)으로 복사해서 거기서 실행하는 방식이다.   
