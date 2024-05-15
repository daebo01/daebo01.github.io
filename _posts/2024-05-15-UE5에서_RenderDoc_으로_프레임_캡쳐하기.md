---
title: UE5에서 RenderDoc 으로 프레임 캡쳐하기
date: 2024-05-15 22:43:00 +0900
categories:
- Unreal Engine 5
tags:
- UE5
- RenderDoc
---

## 설치

[RenderDoc 공식 사이트](https://renderdoc.org/)

### renderdoc 플러그인 활성화 및 설정

![plugin window](/assets/attachments/2024-05-15-UE5에서_RenderDoc_으로_프레임_캡쳐하기/img1.png)

에디터에서 프레임 캡쳐를 할게 아니라면 플러그인을 활성화 하지 않아도 무방하다

![editor capture without plugin](/assets/attachments/2024-05-15-UE5에서_RenderDoc_으로_프레임_캡쳐하기/img2.png)

만약 플러그인 비활성화 상태에서 에디터에 프레임 캡쳐를 한다면 뷰포트만 캡쳐되는게 아니라 윈도우 전체가 캡쳐된다

(이는 플러그인 활성화시 프로젝트 세팅에서 조정가능)

## 윈도우 빌드 및 에디터에서 RenderDoc 부착

shipping 빌드에서는 자동 부착 및 렌더링 이벤트 마커가 동작하지 않는다

될 수 있으면 development build에서 하는걸 권장

### 플러그인이 활성화되어 있으면

![windows build renderdoc attach](/assets/attachments/2024-05-15-UE5에서_RenderDoc_으로_프레임_캡쳐하기/img3.png)

메뉴에서 File -> Attach to Running Instance 를 클릭 한 후, 

실행 중인 빌드를 선택 한다

혹은 콘솔을 열어 renderdoc.captureframe을 입력하면 자동으로 renderdoc 실행 후 프레임 캡쳐가 진행된다

### 플러그인이 비활성화되어 있으면 (or shipping 빌드)

![windows build renderdoc attach without renderdoc plugin](/assets/attachments/2024-05-15-UE5에서_RenderDoc_으로_프레임_캡쳐하기/img4.png)

Launch Application 탭에서 빌드를 선택하고 Launch를 클릭

만일 Launch Application 탭이 없을 경우 메뉴 Window -> Launch Application 을 클릭

### 프레임 캡쳐

![renderdoc frame capture](/assets/attachments/2024-05-15-UE5에서_RenderDoc_으로_프레임_캡쳐하기/img10.png)

Capture Frame(s) Immediately 버튼 또는 하단의 버튼을 클릭하여 프레임을 캡쳐하자

## 안드로이드 빌드에서 RenderDoc 부착

안드로이드 기기에서 usb 디버깅 모드를 켜준 후, pc와 연결한다 (네트워크로 연결해도 될듯? 테스트는 안해봄)

### 부착

![android build renderdoc attach](/assets/attachments/2024-05-15-UE5에서_RenderDoc_으로_프레임_캡쳐하기/img5.png)

renderdoc 상태바 왼쪽에서 replay context 선택기를 누르면 연결된 안드로이드 기기가 보인다

클릭하면 자동으로 renderdoc remote server가 안드로이드 기기에 설치 및 실행된다

기기에 초록색 바탕의 액티비티가 자동으로 뜬다 (종료하면 안됨)

![renderdoc android remote server apk](/assets/attachments/2024-05-15-UE5에서_RenderDoc_으로_프레임_캡쳐하기/img6.png)

만일 모종의 이유로 renderdoc remote server가 설치되지 않는다면, renderdoc 설치 경로/plugins/android/org.renderdoc.renderdoccmd.arm64.apk 를 수동으로 설치후 다시 시도 해보자

![application start](/assets/attachments/2024-05-15-UE5에서_RenderDoc_으로_프레임_캡쳐하기/img7.png)

Launch Application 우측의 버튼을 통해 빌드를 선택하고 Launch를 클릭

### 프레임 캡쳐

Capture Frame(s) Immediately 또는 그 하단에 다른 옵션의 버튼을 클릭해서 캡쳐하자

![android build screenshot](/assets/attachments/2024-05-15-UE5에서_RenderDoc_으로_프레임_캡쳐하기/img9.jpg)

만일 캡쳐가 되지 않는 경우 Cycle Active Window를 클릭하여 활성 윈도우를 변경하고 다시 캡쳐 해보자

게임 화면 좌상단에 프레임 카운터가 보여아 한다
