# Vision-Based Robotic Manipulation System  
**Capstone Design Project | ROS 2 · MoveIt 2 · Vision**

<p align="center">
  <img src="images/robot_overview.jpg" width="600"/>
</p>

---

## 1. 프로젝트 개요

본 프로젝트는 **비전 기반 객체 인식과 로봇 매니퓰레이터 제어를 통합한 자율 조작 시스템**을 구현하는 것을 목표로 한다.  
기존의 **티칭(Teaching) 중심 로봇 제어 방식이 가지는 구조적 한계**를 극복하기 위해,  
**환경 인식 결과를 직접 로봇 행동으로 연결하는 Vision-to-Action 제어 파이프라인**을 실제 로봇 하드웨어 상에서 설계·구현하였다.

본 시스템은 단순히 사전에 입력된 동작을 반복 수행하는 방식이 아니라,  
**환경을 인식하고 그 결과를 기반으로 동작을 생성하는 자율 로봇 제어 구조**를 지향한다.

---

## 2. 배경 및 문제 정의

### 2.1 Teaching 기반 매니퓰레이션의 한계

기존의 티칭 방식은 다음과 같은 구조적 문제를 가진다.

- 로봇 기준 좌표(Base 또는 Pelvis)는 실행 시마다 미세한 위치 오차 발생
- 동일한 위치를 티칭하더라도:
  - 관절 각도 불일치
  - 정밀 작업 실패 가능성 증가
- 이동 로봇 또는 휴머노이드 환경에서는  
  **티칭 동작의 재현성이 크게 저하**

<p align="center">
  <img src="images/teaching_problem.jpg" width="500"/>
</p>

---

### 2.2 제안 접근 방식

본 프로젝트에서는 위 문제를 해결하기 위해 다음과 같은 접근을 제안한다.

- 비전 기반 환경 인식
- 객체 중심의 **상대 좌표 기반 제어**
- 로봇 초기 위치 변화에 강건한 제어 구조 설계

---

## 3. 시스템 아키텍처

<p align="center">
  <img src="docs/system_diagram.png" width="700"/>
</p>

**System Flow**

Camera Input  
→ Object Detection (YOLOv5)  
→ Depth 기반 3D 위치 추정  
→ 좌표 변환 (Camera → Robot Base)  
→ Motion Planning (MoveIt 2)  
→ 자율 매니퓰레이터 제어  

---

## 4. 하드웨어 구성

<p align="center">
  <img src="images/hardware_full.jpg" width="300"/>
  <img src="images/hardware_arm.jpg" width="300"/>
  <img src="images/hardware_gripper.jpg" width="300"/>
</p>

- **Manipulator**: BCN3D MOVEo (커스텀 개조)
- **Actuators**
  - MG995 서보 모터
  - 스테퍼 모터
- **End-Effector**
  - 자체 설계 및 3D 프린팅 그리퍼
- **Vision Sensor**
  - Arducam ToF Camera (RGB + Depth)
- **제작 및 조립**
  - 3D 프린팅 및 MDF 가공
  - 커스텀 배선 및 기구 조립

---

## 5. 소프트웨어 스택

| 구분 | 기술 |
|------|------|
| Robot Middleware | ROS 2 Humble |
| Motion Planning | MoveIt 2 |
| Robot Modeling | URDF (직접 작성) |
| Vision | YOLOv5 |
| Programming Language | Python / C++ |
| Operating System | Ubuntu 22.04 |

---

## 6. 핵심 기능

### 6.1 비전 기반 객체 인식

<p align="center">
  <img src="images/yolo_detection.png" width="500"/>
</p>

- YOLOv5 기반 실시간 객체 검출
- Bounding box와 Depth 정보 융합
- 카메라 좌표계 기준 객체 위치 추정

---

### 6.2 좌표 변환 (Coordinate Transformation)

<p align="center">
  <img src="images/tf_frames.png" width="500"/>
</p>

- 카메라 좌표계 → 로봇 기준 좌표계 변환
- ROS TF 기반 좌표 프레임 관리
- 로봇 자세 변화에도 안정적인 목표 Pose 계산

---

### 6.3 자율 매니퓰레이션

<p align="center">
  <img src="images/moveit_planning.png" width="500"/>
</p>

- MoveIt 2 기반:
  - 역기구학(Inverse Kinematics)
  - 경로 계획(Motion Planning)
  - 위치 및 자세 오차 허용 범위 튜닝
- 인식된 객체 위치를 기반으로 한 자동 팔 동작 실행

---

## 7. 프로젝트 구조

- **moveo_description/**
  - 로봇 URDF 모델
  - 링크 및 조인트 정의

- **moveo_moveit/**
  - MoveIt 설정 패키지
  - 역기구학 및 경로 계획 파라미터

- **vision_node/**
  - YOLO 기반 객체 인식 노드
  - Depth 정보 기반 위치 추정

- **coordinate_tf/**
  - 좌표계 변환 로직
  - ROS TF 프레임 관리

- **control_node/**
  - 매니퓰레이터 제어 로직
  - 인식 결과 기반 자율 동작 수행

- **images/**
  - 하드웨어 사진
  - 실험 결과 및 시연 이미지

- **README.md**
  - 프로젝트 개요 및 사용 방법

---

## 8. 실험 결과

<p align="center">
  <img src="images/result_pick.gif" width="500"/>
</p>

- 객체 접근 및 조작 안정적으로 성공
- 로봇 기준 위치 변화에도 강건한 동작 수행
- 티칭 의존도 대폭 감소
- **Perception-to-Action 파이프라인 구현**

---

## 9. 핵심 인사이트

- Teaching 기반 제어는 **기억 중심 제어**
- 비전 기반 제어는 **이해 중심 제어**
- 로봇 자율성은 다음 요소의 결합을 통해 확보된다:
  - Environment Perception
  - Coordinate Interpretation
  - Action Generation

본 프로젝트는 향후 **Physical AI 및 학습 기반 로봇 제어로 확장 가능한 구조적 기반**을 제공한다.

---

## 10. 향후 연구 방향

- Demonstration 데이터 기반 Behavioral Cloning
- Diffusion Policy 기반 행동 생성
- ArUco Marker를 활용한 정밀 좌표 보정
- 이동 로봇(Mobile Robot)과의 통합
- Learning-based Manipulation 확장

---

## 11. 작성자

- **손영훈 (Younghoon Son)**  
- 한양대학교 ERICA 로봇공학과
- 관심 분야:
  - Robot Software
  - ROS 2
  - Physical AI
  - Vision-based Manipulation

---

## 12. 참고 자료

- Demo Video: *(추가 예정)*  
- Final Report: *(추가 예정)*  
