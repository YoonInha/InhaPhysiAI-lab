# InhaPhysiAI-lab 프로젝트 개요

**목표:**  
* 피지컬 AI 시스템을 기반으로, 센서-모터 제어부터 고도 인공지능 판단까지 수행하는 자율 로봇을 개발한다.
* 모든 프로덕트는 공통적으로 Physical AI를 추구하므로 다음과 같은 계층 아키텍처를 기반으로 함

## 계층 아키텍처
- **말초신경 (MCU):** 센서 입력 및 서보/스텝 모터 직접 제어
- **중추신경 (Raspberry Pi):** 실시간 제어, 자체 CV 연산 수행, 비상시 독립 동작
- **대뇌 (PC/AI):** 고성능 AI 모델로 판단, 경로 계획, 목표 추론 등 수행

![](images/01/2025-02-01-22-56-07.png)
## 마일 스톤
	1. RnD
	2. 제품1: 주행형 로봇 (`SA-01`)
	2. 제품2: 이미지 인식 로봇 팔 (`NA-01`)
	3. 제품2: 4족 보행 로봇 개 (`QD-01`)
	4. 제품3: 간소화 휴머노이드 (`HM-01`)




## 단계별 마일스톤

* R&D 및 기술 검증
	* MicroControler (AVR)
	* Linux (Raspberry pi)
	* PC (Windows, Linux, Mac)


* [[R&D]]
	* MicroControler (AVR)
	* Linux (Raspberry pi)
	* PC (Windows, Linux, Mac)
* [[SlothAMR]]
	* Architecture
* [[NeuroArm]]
	* Architecture
	* 


## Project Repositories

### SlothAMR
* 개요 : ROS2의 turtlebot에 해당하는 기본적인 주행 로봇
* github URL : https://github.com/YoonInha/SlothAMR


### NeuroArm
* 개요 : Yolo를 이용한 이미지 인식과 pytorch를 이용한 학습모델로 자율동작을 하는 로봇 팔 프로젝트
	* NeuroArm-OPS : 제어 및 그래픽 UI, 학습데이터 수집, 디버깅하는 종합 앱 프로젝트
	* NeuroArm-Core : yolo 이미지 처리 및 명령하는 AI 모델 제어부 (PC)
	* NeuroArm-HW : 이미지를 모델로 전송하고 명령을 firmware로 전달하는 인터페이스
* github URL
	* NeuroArm-OPS : https://github.com/YoonInha/NeuroArm-OPS
	* NeuroArm-Core : https://github.com/YoonInha/NeuroArm-Core
	* NeuroArm-HW : https://github.com/YoonInha/NeuroArm-HW

