---
layout: post
title: 운영체제 Three Easy Pieces Ch.5
---
### 제 5 장 프로세스 API

##### 서론

- UNIX 시스템의 프로세스 생성에 관해 논의할 것

  > 프로세스를 생성하고 제어하기 위해 운영체제가 어떤 인터페이스를 제공해야 하는가?
  >
  > 유용성, 편리성, 성능을 위해 어떻게 인터페이스를 설계해야 하는가?

##### 5.1 fork() 시스템 콜

- **Process identifier (PID)**: 프로세스 식별자

- **fork()**를 호출한 프로세스의 복사본을 생성하고, Child Process에게는 0을, Parent Process에게는 Child Process의 PID를 Return하게 됨

- Parent와 Child의 실행 순서는 CPU Scheduler에 의해 결정됨

  > Nondeterminism(비결정성)으로 인해 MultiThread Program 실행에 다양한 문제가 발생한다

##### 5.2 wait() 시스템 콜

- Parent Process가 **wait()**을 호출하여 Child Process의 종료를 기다리게 할 수 있음

##### 5.3 exec() 시스템 콜

- 다른 프로그램을 실행해야 할 때 사용
- 새로운 실행 파일이 들어오면 현재 실행 파일의 코드와 정적 데이터 부분을 덮어 쓴 뒤, Heap과 Stack이 다시 초기화됨

##### 5.4 이러한 API가 생긴 이유

- UNIX Shell을 구현하기 위해 **fork()**와 **exec()**를 분리해야 함

  > Shell은 단순한 사용자 프로그램이고, 사용자에게 명령어를 입력 받으면 fork()를 호출하여 새로운 Child Process를 만든 뒤, exec()를 호출하여 프로그램을 실행시킨 뒤 wait()을 호출하여 Child Process가 끝나기를 기다린다

- Redirection을 하는 경우 Standard Output을 닫고 목표로 하는 파일을 열어 줌

- UNIX **pipe()** System Call은 한 프로세스의 출력과 다른 프로세스의 입력을 연결하기 위한 파이프를 생성

##### 5.5 프로세스 제어와 사용자

- **kill()** 시스템 콜은 프로세스에게 멈추거나, 끝내는 Signal을 보내는 데 사용됨

  > SIGINT, SIGSTP 등이 있다

- 사용자가 다른 사용자의 프로세스에게 signal을 보내는 것을 방지하기 위해 user라는 개념을 도입

  > 모든 권한을 가진 super user, 혹은 root 사용자가 존재한다

##### 5.6 유용한 도구들

- man: 매뉴얼 페이지
- top: 시스템에 존재하는 프로세스와 해당 프로세스가 사용 중인 자원 표시
- kill, killall: 프로세스에게 Signal 보낼 때 사용
- MenuMeter 등: 시스템 부하 정도 측정, CPU 측정기

##### 5.7 요약

- Spawn() 같은 간단한 프로세스 생성 API를 사용해야 한다는 주장도 있음
