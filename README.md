# 🤖 Vision 기반 로봇 매니퓰레이션 시스템  
**Capstone Design Project | ROS 2 · MoveIt 2 · Vision**

<p align="center">
  <img src="images/robot_overview.jpg" width="600"/>
</p>

---

## 📌 프로젝트 개요

본 프로젝트는 **비전 기반 객체 인식과 로봇 매니퓰레이터 제어를 통합한 자율 조작 시스템**을 구현하는 것을 목표로 한다.  
기존의 **티칭(Teaching) 기반 로봇 제어 방식이 가지는 구조적 한계**를 해결하기 위해,  
**환경 인식 기반(Vision-to-Action) 로봇 제어 파이프라인**을 실제 로봇 하드웨어 위에서 설계·구현하였다.

> 단순히 동작을 외우는 로봇이 아닌,  
> **환경을 인식하고 스스로 행동을 생성하는 로봇 시스템**을 지향한다.

---

## 🎯 프로젝트 배경 및 문제 정의

### ❌ 기존 티칭 기반 로봇의 한계
- 로봇의 기준 좌표(base/pelvis)는 매 실행마다 미세하게 달라짐
- 동일한 위치를 티칭해도:
  - 관절 각도 오차 발생
  - 정밀 작업 실패
- 이동 로봇·휴머노이드 환경에서는 **티칭 재현성이 매우 낮음**

<p align="center">
  <img src="images/teaching_problem.jpg" width="500"/>
</p>

### ✅ 해결 접근
- **비전 기반 환경 인식**
- 객체 중심의 **상대 좌표 기반 제어**
- 로봇 초기 위치 변화에 강건한 제어 구조 설계

---

## 🛠 시스템 전체 구조

<p align="center">
  <img src="docs/system_diagram.png" width="700"/>
</p>

