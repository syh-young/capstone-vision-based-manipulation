# Vision-Based Robotic Manipulation System  
**Capstone Design Project | ROS 2 · MoveIt 2 · Vision**

<p align="center">
  <img src="images/robot_overview.jpg" width="600"/>
</p>

---

## 1. Project Overview

본 프로젝트는 **비전 기반 객체 인식과 로봇 매니퓰레이터 제어를 통합한 자율 조작 시스템**을 구현하는 것을 목표로 한다.  
기존의 티칭(Teaching) 기반 로봇 제어 방식이 가지는 구조적 한계를 해결하기 위해,  
**환경 인식 기반(Vision-to-Action) 로봇 제어 파이프라인**을 실제 로봇 하드웨어 상에서 설계·구현하였다.

본 시스템은 단순한 동작 반복이 아닌,  
**환경을 인식하고 그 결과를 기반으로 행동을 생성하는 로봇 제어 구조**를 지향한다.

---

## 2. Background and Problem Definition

### 2.1 Limitations of Teaching-Based Manipulation

- 로봇의 기준 좌표(base 또는 pelvis)는 실행 시마다 미세한 오차 발생
- 동일한 위치를 티칭하더라도:
  - 관절 각도 불일치
  - 정밀 작업 실패
- 이동 로봇 또는 휴머노이드 환경에서는 티칭 재현성이 매우 낮음

<p align="center">
  <img src="images/teaching_problem.jpg" width="500"/>
</p>

### 2.2 Proposed Approach

- 비전 기반 환경 인식
- 객체 중심의 상대 좌표 기반 제어
- 로봇 초기 위치 변화에 강건한 제어 구조 설계

---

## 3. System Architecture

<p align="center">
  <img src="docs/system_diagram.png" width="700"/>
</p>

Camera Input  
→ Object Detection (YOLOv5)  
→ Depth-based 3D Position Estimation  
→ Coordinate Transformation (Camera → Robot Base)  
→ Motion Planning (MoveIt 2)  
→ Autonomous Manipulator Control  

---

## 4. Hardware Configuration

<p align="center">
  <img src="images/hardware_full.jpg" width="300"/>
  <img src="images/hardware_arm.jpg" width="300"/>
  <img src="images/hardware_gripper.jpg" width="300"/>
</p>

- **Manipulator**: BCN3D MOVEo (custom-modified)
- **Actuators**
  - MG995 Servo Motors
  - Stepper Motors
- **End-Effector**
  - Custom-designed and 3D-printed gripper
- **Vision Sensor**
  - Arducam ToF Camera (RGB + Depth)
- **Fabrication**
  - 3D printing and MDF machining
  - Custom wiring and mechanical assembly

---

## 5. Software Stack

| Category | Technology |
|------|------|
| Robot Middleware | ROS 2 Humble |
| Motion Planning | MoveIt 2 |
| Robot Modeling | URDF (custom-written) |
| Vision | YOLOv5 |
| Programming Languages | Python / C++ |
| Operating System | Ubuntu 22.04 |

---

## 6. Core Functionalities

### 6.1 Vision-Based Object Detection

<p align="center">
  <img src="images/yolo_detection.png" width="500"/>
</p>

- Real-time object detection using YOLOv5
- Fusion of bounding box and depth information
- Object position estimation in camera coordinate frame

---

### 6.2 Coordinate Transformation

<p align="center">
  <img src="images/tf_frames.png" width="500"/>
</p>

- Transformation from camera frame to robot base frame
- ROS TF-based coordinate management
- Robust target pose computation under robot pose variations

---

### 6.3 Autonomous Manipulation

<p align="center">
  <img src="images/moveit_planning.png" width="500"/>
</p>

- MoveIt 2-based:
  - Inverse kinematics
  - Motion planning
  - Position and orientation tolerance tuning
- Automatic arm motion execution based on perceived object position

---

## 7. 프로젝트 구조

- **moveo_description/**
  - 로봇 URDF 모델 및 링크/조인트 정의 파일

- **moveo_moveit/**
  - MoveIt 기반 모션 플래닝 설정
  - 역기구학 및 경로 계획 파라미터

- **vision_node/**
  - YOLO 기반 객체 인식 노드
  - Depth 정보를 이용한 객체 위치 추정

- **coordinate_tf/**
  - 카메라 좌표계에서 로봇 기준 좌표계로의 변환
  - ROS TF 프레임 관리

- **control_node/**
  - 로봇 매니퓰레이터 제어 로직
  - 인식 결과 기반 자율 동작 실행

- **images/**
  - 로봇 하드웨어 사진
  - 실험 결과 및 시연 이미지

- **README.md**
  - 프로젝트 전체 설명 및 사용 방법



---

## 8. Experimental Results

<p align="center">
  <img src="images/result_pick.gif" width="500"/>
</p>

- Stable object 접근 및 조작 성공
- 로봇 기준 위치 변화에 대한 강건한 동작 수행
- 티칭 의존도 감소
- 실제 산업 및 연구용 로봇과 유사한 **Perception-to-Action 파이프라인 구현**

---

## 9. Key Insights

- Teaching은 기억 기반 제어이며, 비전 기반 제어는 이해 기반 제어이다.
- 로봇의 자율성은 다음 요소의 결합을 통해 확보된다:
  - Environment Perception
  - Coordinate Interpretation
  - Action Generation
- 본 프로젝트는 **Physical AI 및 학습 기반 로봇 제어로 확장 가능한 구조적 기반**을 제공한다.

---

## 10. Future Work

- Demonstration data 기반 Behavioral Cloning
- Diffusion Policy 기반 행동 생성
- ArUco Marker를 이용한 정밀 보정
- Mobile robot와의 통합
- Learning-based manipulation 확장

---

## 11. Author

- **Younghoon Son**
- Department of Robotics Engineering  
  Hanyang University ERICA
- Research Interests:
  - Robot Software
  - ROS 2
  - Physical AI
  - Vision-based Manipulation

---

## 12. References

- Demo Video: *(To be added)*
- Final Report: *(To be added)*

