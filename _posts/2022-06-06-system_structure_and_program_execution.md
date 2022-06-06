
---
layout: single
title: "System Structure & Program Execution"
date: "2022-06-06"
last_modified_at: "2022-06-06"
---

> CPU의 헤르츠 정보는 초당 얼마나 데이터를 처리하는지 나타탬 (GHz는 약 초당 32억번의 사이클)

> 비트 정보는 한번에 얼마나 많은 데이터를 처리할 수 있느냐. 32비트는 2의 32승, 64비트는 2의 64승 만큼의 데이터를 처리

# 컴퓨터 시스템 구조

<p align="center">
<img src="/assets/images/2022-06-06-images/1.png" width="90%" height="90%" title="from: 반효경 [운영체제] 3. System Structure & Program Execution 1" alt="ERROR:cannot load image"/>
</p>

위와 같이 CPU와 메모리로 돼 있는 것을 보통 컴퓨터라고 부른다.<br>
각각의 I/O Device들은 그 장치들을 관리하는 CPU(device controller)가 있다. 즉 I/O의 내부 통제는 device controller가 수행한다.<br>
각각의 device들은 처리 속도가 느리다.<br>
CPU의 운명은 매 클럭마다 메모리에 있는 명령어를 하나하나 읽어서 실행하는 것이다.<br>

### Mode Bit

mode bit은 CPU에서 실행하고 있는것이 운영체제인지 아닌지 구분하는 것.<br>
    - 사용자 프로그램을 실행중일 때 mode bit이 1로 설정된다.
    -  운영체제가 실행중일 때는 mode bit이 0으로 설정된다.
    - modebit이 1일때 보안상의 이유로 할 수 있는 일이 제한된다.
<br>

### Interrupt Line

interrupt line: interrupt가 없다면 프로그램 계속 실행. 있으면 운영체제에게 CPU를 반납한다. (타이머로 설정한 시간으로 interrupt line 생성한다. 혹은 작업이 끝났다면 device controller가 interrupt line을 생성한다.)  

### Timer

특정 프로그램이  CPU를 독점하는 것을 막기 위한 것. 한 프로그램이 무한루프에 빠지면? 운영체제가 CPU를 사용자 프로그램에게 할당해줄때 실제로는 아주 짧은 시간(ms 단위)로 타이머를 설정해서 준다. 정해진 시간이 흐른 뒤 운영체제에게 제어권이 넘어가도록 인터럽트 시킨다

### CPU가 너무 많이 Interrupt되는 경우에 어떻게 될까? (DMA) 

그래서 DMA Direct Momory Access를 둔다. 그러면 CPU를 DMA에서도 접근 가능한데. 같은 리소스를 동시에 다루는 경우를 막기 위해 momory controller라는 장치를 또 둔다.

### Device Controller & Device Driver

Device Driver: OS 코드 중 각 장치별 처리루틴 -> Device Controller와 다르게 소프트웨어다.

### 인터럽트

인터럽트 당한 시점의 레지스터와 program counter를 save한 후 CPU의 제어를 인터럽트 처리 루틴에 넘긴다.<br>
    - 하드웨어 인터럽트: 하드웨어가 발생시킨 인터럽트
    - Trap: (소프트웨어 인터럽트)
        - **Exception**: 프로그램이 오류를 범한 경우
        - **System call**: 프로그램이 커널 함수를 호출한 경우

### 인터럽트 관련 용어

- 인터럽트 벡터: 해당 인터럽트의 처리 루틴 주소를 가지고 있음
- 인터럽트 처리 루틴(= Interrupt Sever Rutine): 해당 인터럽트를 처리하는 커널 함수
<br>        
                
### 인터럽트의 종류

- timer interrupt
- device controller interrupt

### 레지스터

메모리보다 빠르면서 정보저장 레지스터

# Reference
- [KOCW 운영체제 강의: Introduction to Operating System](http://www.kocw.or.kr/home/enrolment/enrolmentView.do?cid=3646706b4347ef09&lid=5a0205590631eece)
