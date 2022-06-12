---
layout: single
title: "System Structure & Program Execution"
date: "2022-06-06"
last_modified_at: "2022-06-12"
---
<br>
> CPU의 헤르츠 정보는 초당 얼마나 데이터를 처리하는지 나타냄 (GHz는 약 초당 32억번의 사이클)
<div></div>
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

mode bit은 CPU에서 실행하고 있는것이 운영체제인지 사용자 프로그램인지 구분하는 것.<br>
    - 사용자 프로그램을 실행중일 때 mode bit이 1로 설정된다.
    - 운영체제가 실행중일 때는 mode bit이 0으로 설정된다.
    - modebit이 1일때 보안상의 이유로 할 수 있는 일이 제한된다.
<br>

### Interrupt Line

interrupt line: interrupt가 없다면 프로그램 계속 실행. 있으면 운영체제에게 CPU를 반납한다. (타이머로 설정한 시간으로 interrupt line 생성한다. 혹은 작업이 끝났다면 device controller가 interrupt line을 생성한다.)  

### Timer

특정 프로그램이 CPU를 독점하는 것을 막고 부작용을 방지하기 위한 것. 예를들어 한 프로그램이 무한루프에 빠진다면 무한로프를 도는 프로그램이 CPU를 장악한채로 다른 작업을 할 수 없게되는 것이다. <br>
운영체제가 CPU를 사용자 프로그램에게 할당해줄때 실제로는 아주 짧은 시간(ms 단위)로 타이머를 설정해서 준다. 이러한 타이머는 정해진 시간이 흐른 뒤 운영체제에게 제어권이 넘어가도록 인터럽트 시킨다.

### CPU가 너무 많이 Interrupt되는 경우에 어떻게 될까? (DMA(Direct Memory Access) Controller) 

- 원래는 메모리에 접근할 수 있는 것은 CPU밖에 없다. <br>
- 버퍼에 특정 용량이 쌓였다면 그 작업이 끝났다고 CPU에게 알려준다. <br> 
- 이렇게하면 CPU가 인터럽트 당하는 빈도가 줄어든다. <br>
- 빠른 입출력 장치를 메모리에 가까운 속도로 처리하기 위해 사용
- CPU의 중재 없이 device controller가 device의 buffer storage의 내용을 메모리에 block 단위로 직접 전송
- 바이트 단위가 아니라 block 단위로 인터럽트를 발생시킴
- 같은 리소스를 동시에 다루는 경우를 막기 위해 momory controller라는 장치를 또 둔다.

### Device Controller & Device Driver

Device Driver: OS 코드 중 각 장치별 처리루틴 -> Device Controller와 다르게 소프트웨어다.
<br>

### 인터럽트

I/O Device가 인터럽트를 통해 CPU에게 정보를 전달

인터럽트 당한 시점의 레지스터와 program counter를 save한 후 CPU의 제어를 인터럽트 처리 루틴에 넘긴다.<br>
    - 하드웨어 인터럽트: 하드웨어가 발생시킨 인터럽트<br>
    - Trap: (소프트웨어 인터럽트)
        - **Exception**: 프로그램이 오류를 범한 경우
        - **System call**: 프로그램이 커널 함수를 호출한 경우

### 인터럽트 관련 용어

- 인터럽트 벡터: 해당 인터럽트의 처리 루틴 주소를 가지고 있음
- 인터럽트 처리 루틴(= Interrupt Sever Rutine): 해당 인터럽트를 처리하는 커널 함수
<br>

### 인터럽트의 종류

- device controller interrupt
- timer interrupt
<br>

### 레지스터

메모리보다 빠른 저장 장치

### Program Counter (CPU안의 레지스터)

CPU가 다음에 읽어들어야 할 메모리 주소를 가리킴. CPU는 이런 주소를 읽는 일을 하는 것.<br>
하지만 그전에 인터럽트가 있는지 확인한다. 인터럽트가 있다면 운영체제에게 CPU가 넘어가고 해당 일을 CPU가 처리한다.

### 시스템 콜

시스템 호출 또는 시스템 콜(system call), 혹은 간단히 시스콜은 운영 체제의 커널이 젝오하는 서비스에 대해, 응용 프로그램의 요청에 따라 커널에 접근하기 위한 인터페이스다. 보통 C나 C++과 같은 고급언어로 작성된 프로그램들은 직접 시스템 호출을 사용할 수 없기 때문에 고급 API를 통해 시스템 호출에 접근하게 하는 방법이다.

### 동기식 입출력과 비동기식 입출력

- 동기식 입출력(synchronous I/O)
    - I/O 요청 후 입출력 작업이 완료된 후에야 제어가 사용자 프로그램에 넘어감
    
    - 구현 방법 1
        - I/O가 끝날 때까지 CPU를 낭비시킴
        - 매시점 하나의 I/O만 일어날 수 있음
    - 구현 방법 2
        - I/O가 완료될 때까지 해당 프로그램에게서 CPU를 빼앗음
        - I/O 처리를 기다리는 중에 그 프로그램을 줄 세움
        - 다른 프로그램에게 CPU를 줌
        
- 비동기식 입출력 (asynchronous I/O)
    - I/O가 시작된 후 입출력 작업이 끝나기를 기다리지 않고 제어가 사용자 프로그램에 즉시 넘어감 
<br>
두 경우 모두 인터럽트를 통해서 I/O가 작업이 끝났다고 알려준다. 

### 서로 다른 입출력 명령어

- I/O를 수행하는 special instruction에 의해
- Memory Mapped I/O에 의해
<br>
메모리 주소가 있듯이 접근할 수 있는 Device에도 접근할 수 있는 주소가 있다.

### 저장장치 계층 구조

Registers - Cache Memory - Main Memory(D-RAM):<br>
저용량, 빠름, 휘발성, 단위 면적당 가격이 비쌈, Primary(Executable: CPU가 접근하려면 byte단위로 저장소가 구성되어야 하는데 이러한 저장장치들은 바이트 단위로 접근 가능)<br>
<br>
Magnetic Disk - Optical Disk - Magnetic Tape:<br>
고용량, 느림, 비휘발성, 단위 면적당 가격이 쌈, Secondary(Executable) 
<br>
> Volatility(휘발성) - 전원이 나가면 데이터가 사라짐
<div></div>
> Caching: copyint information into fater storage system
<br>
빠르게 접근하기 위해 접근이 빠른 저장소에 저장해 놓는 것. 저용량이기 때문에 새로운 것을 읽어들이면 기존의 것을 쫓아내야함.

### 프로그램의 실행 (메모리 load)

File system -> Virtual memory -> Physical memory(커널 항상 존재)<br>

실제 프로그램의 모든 데이터를 피지컬 메모리에 올려두지 않는데 그 이유는 피지컬 메모리 낭비가 되기 때문. 그래서 필요한 함수 등만 메모리에 올려 놓는다. 나머지 필요하지 않은 부분은 디스크, Swap area에 내려놓게 된다. Virtual memory는 가상의 주소공간으로 실제로 존재하는 주소가 아니다. Virtual Memory는 각 프로그램이 독자적으로 가지고 있다.<br>

> Virtual Memory는 전원이 나가면 사라진다. 반대로 File system에 있는 것은 보관된다.
<div></div>
> 메모리 주소변환(Address translation) 가상 메모리를 실제 메모리로 올릴때 주소를 정하는 것.

### 커널 주소 공간의 내용

커널 코드(OS) in memory
- 시스템콜, 인터럽트 처리 코드
- 자원 관리를 위한 코드
- 편리한 서비스 제공을 위한 코드

### 사용자 프로그램이 사용하는 함수

- 사실 어떠한 프로그램이라도 함수로 짜여져 있다.
- 함수(function)
    - 사용자 정의 함수
    자신의 프로그램에서 정의한 함수 <br>
    - 라이브러리 함수
    자신의 프로그램에서 정의하지 않고 갖다 쓴 함수<br>
    
    각 프로세스의 메모리 Address space에 존재한다<br>
    - 커널 함수
    커널의 존재해서 메모리 점프를 해야 하는데......<br>
    
    사실 엄연히 다른 메모리 공간이라 점프하지 못한다(사용자 프로그램이 운영체제(커널)를 직접 건드리지 못한다).<br>
    따라서 커널 함수를 사용하기 위해선 interrupt를 사용, 시스템 콜을 이용해 불러야 한다.
<br>
 
<p align="center">
<img src="/assets/images/2022-06-06-images/2.png" width="90%" height="90%" title="from: 반효경 [운영체제] 3. System Structure & Program Execution 2" alt="ERROR:cannot load image"/>
</p>

# Reference
- [KOCW 운영체제 강의: Introduction to Operating System](http://www.kocw.or.kr/home/enrolment/enrolmentView.do?cid=3646706b4347ef09&lid=5a0205590631eece)
