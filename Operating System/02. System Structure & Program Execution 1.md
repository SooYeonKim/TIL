# System Structure & Program Execution 1

> "KOCW - 반효경 교수님의 운영체제"를 듣고 정리한 내용입니다.
>
> http://www.kocw.net/home/search/kemView.do?kemId=1046323

## 컴퓨터 시스템 구조
- CPU
- Memory
    - CPU의 작업공간
    - CPU는 매 클럭 사이클마다 메모리에서 기계어를 하나씩 읽어서 실행을 함
- I/O device
    - Input device: 키보드, 마우스
    - Output device: 모니터, 프린터
- Disk
    - 하드 디스크는 데이터를 메모리로 읽어 들이기도 하고, 처리 결과를 디스크의 파일 시스템에 저장을 하기도 함 (I/O device)
- registers
    - 메모리보다 더 빠르면서 정보를 저장할 수 있는 작은 공간
- mode bit
    - 사용자 프로그램의 잘못된 수행으로 다른 프로그램 및 운영체제에 피해가 가지 않도록 하기 위한 보호 장치 필요
    - 지금 CPU에서 실행되는 것이 운영체제인지 사용자 프로그램인지를 구분
    - 0일 때는 CPU에서 아무거나 실행이 되고, CPU를 운영체제가 사용자 프로그램한테 넘겨줄 때 mode bit을 1로 바꾸기 때문에 한정된 instruction만 실행할 수 있음
    - 인터런트가 들어오게 되면 CPU 제어권이 운영체제에게 넘어가면서 mode bit은 자동으로 0으로 바뀌게 됨
- Interrupt line
- timer
    - 특정 프로그램이 CPU를 독점하는 것을 막기 위한 것
    - 정해진 시간이 흐른 뒤 운영체제에게 제어권이 넘어가도록 인터럽트를 발생시킴
- Device Controller
    - I/O device controller
        - 해당 I/O 장치유형을 관리하는 일종의 작은 CPU
        - 제어 정보를 위해 control register, status register를 가짐
        - local buffer를 가짐(일종의 data register)
    - I/O는 실제 device와 local buffer 사이에서 일어남
    - Device controller는 I/O가 끝났을 경우 interrupt로 CPU에 그 사실을 알림

## 입출력(I/O)의 수행
- 모든 입출력 명령은 특권 명령
- 사용자 프로그램은 어떻게 I/O를 하는가?
    - 시스템콜(system call)
        - 사용자 프로그램은 운영체제에게 I/O 요청
    - trap을 사용하여 인터럽트 벡터의 특정 위치로 이동
    - 제어권이 인터럽트 벡터가 가리키는 인터럽트 서비스 루틴으로 이동
    - 올바른 I/O 요청인지 확인 후 I/O 수행
    - I/O 완료 시 제어권을 시스템콜 다음 명령으로 옮김

## 인터럽트(Interrupt)
- 인터럽트
    - 인터럽트 당한 시점의 레지스터와 program counter를 save 한 후 CPU의 제어를 인터럽트 처리 루틴에 넘긴다.
- Interrupt (넓은 의미)
    - Interrupt (하드웨어 인터럽트): 하드웨어가 발생시킨 인터럽트
    - Trap (소프트웨어 인터럽트)
        - Exception: 프로그램이 오류를 범한 경우
        - System call: 프로그램이 커널 함수를 호출하는 경우
- 인터럽트 관련 용어
    - 인터럽트 벡터
        - 해당 인터럽트의 처리 루틴 주소를 가지고 있음
    - 인터럽트 처리 루틴
        - 해당 인터럽트를 처리하는 커널 함수

## 시스템콜(System Call)
- 시스템콜
    - 사용자 프로그램이 운영체제의 서비스를 받기 위해 커널 함수를 호출 하는 것