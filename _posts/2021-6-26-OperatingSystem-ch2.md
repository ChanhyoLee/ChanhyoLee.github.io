---
layout: post
title: 운영체제 Three Easy Pieces Ch.2
---

### 제 2 장 운영체제 개요

##### 서론

- 프로그램의 일: **fetch, decode, execute** (Vonn Neumann의 컴퓨팅 모델의 기초)
- physical resource를 이용하여 **virtual 형태의 자원**을 생성 –> 운영체제를 Virtual Machine라고도 부름
- OS는 **system call, standard library**를 제공하며, CPU, 메모리, 및 디스크라는 시스템의 자원을 공정하게 관리하는 자원 관리자로 불리기도 함

##### 2.1 CPU 가상화

- 한 개, 혹은 소규모의 CPU를 무한 개의 CPU가 존재하는 것처럼 변환하여 동시에 많은 수의 프로그램을 실행시키는 것
- 운영체제의 **Policy(정책)** vs. 자원 관리자로서의 운영체제의 역할 **Mechanism(매커니즘)**

##### 2.2 메모리 가상화

- 물리 메모리에 read, write(or update)하기 위해서는 address가 명시되어야 함
- 각 프로세스는 자신만의 가상 주소 공간 (Virtual Address Space)를 가짐, 운영체제가 이 주소를 실제 물리 메모리로 **Mapping함**, 하나의 프로그램이 수행하는 메모리 연산은 다른 프로그램의 주소 공간에 영향을 주지 않음

##### 2.3 병행성

- CPU는 여러 명령어들을 **Atomically(한 번에, 원자적으로)** 실행하지 못하기 때문에 예상하지 않은 일이 발생할 수 있음

##### 2.4 영속성

- DRAM 같은 장치는 Volatile(휘발성) 특성이 있기 때문에 영속적으로 저장할 수 있는 장치가 필요
- 입력/출력(Input/Output) 장치 형태로 제공, 최근에는 Solid-State Drives(SSDs)가 많이 사용되지만, 일반적으로 Hard Drive가 사용됨
- 디스크를 관리하는 운영체제 소프트웨어를 File System이라고 부르며, 사용자가 생성한 File을 안전하고 효율적인 방식으로 저장할 책임이 있음
- 운영체제는 System Call이라는 표준화된 방법으로 장치들을 접근할 수 있게 하기 때문에 Standard Library처럼 보이기도 함
- 대부분의 파일 시스템들은 응용 프로그램들의 쓰기 요청을 모아두었다가 한 번에 처리하는데, 저장되기 이전에 문제가 생겼을 때 이를 대비하여 Journaling이나 Copy-On-Write 기법들을 사용하며, 다양한 자료 구조를 사용함

##### 2.5 설계 목표

- 운영체제는 물리 자원을 가상화 함
- 가장 기본적인 목표는 시스템을 편리하고 사용하기 쉽게 만드는 데 필요한 **개념**을 정의하는 것 **(Abstraction, 추상화)**
- 가장 중요한 목표는 **성능(Performance)** - Overhead를 최소화 하는 것 
- 응용 프로그램 간의 **Protection**, 운영체제와 응용 프로그램 간의 **Protection**, **Protection**은 **Isolation**의 핵심
- 높은 수준의 **신뢰성(Reliability), 에너지 효율성(Energy-Efficiency), 보안(Protection), 이동성(Mobility)**

##### 2.6 배경 소개

- 초창기 운영체제는 단순 **라이브러리**에 불과했으며, 컴퓨터 관리자가 직접 처리하는 **Batch Computing(일괄 처리)** 방식이었음
- 이후로 **System Call**이라는 아이디어가 발명되었으며, 운영체제와 일반 라이브러리가 **분리**되기 시작하였음
- System Call과  Procedure Invocation의 차이는 제어권을 운영체제에게 넘길  때 **System Call**의 경우 **hardware privilege level**(하드웨어 특권 수준)으로 상향 조정함, 이외의 프로그램들은 **user mode**에 한해서 실행됨
- 사용자 응용 프로그램들이 수행할 수 없는 작업을 수행하면 **Trap**이라는 특수한 하드웨어 명령어를 호출하여 System Call을 수행함
- System Call이 시작되면 미리 지정된 **Trap Handler** 함수에게 제어권을 넘기고, **Kernel Mode**로 격상 시킨 뒤 특수 작업을 끝나고 **return-from-trap** 명령어를 사용하여 제어권을 다시 사용자에게 넘김
- 미니 컴퓨터가 등장하면서 운영체제가 고도화 되었으며, 컴퓨터 자원을 효율적으로 활용하기 위해 Multi Programming 기법이 발전하게 되었음, Memory Protection, Concurrency 등의 주제가 각광받음
- Personal Computer(PC)가 등장한 지 얼마 되지 않았을 때에는 Mini Computer에서 얻었던 운영체제 기술들을 제대로 활용하지 못하였지만, 시간이 지나면서 제대로 활용하기 시작하였고 현대에 이르러 많이 발전하였음

##### 2.7 요약

- 운영체제에는 Networking 코드가 있음, Graphic 장치가 중요함, Protection도 상세히 다룰 수 있는 부분이 있음
- CPU, memory의 가상화와 기본 원리, 병행성 및 장치와 파일 시스템을 통한 영속성에 대해 다룰 것