# **Context Switching**

멀티 프로세스 환경에서 CPU가 어떤 하나의 프로세스를 실행하고 있는 상태에서 인터럽트 요청에 의해 다음 우선 순위의 프로세스가 실행되어야 할 때 기존의 프로세스의 상태 또는 레지스터 값(Context)을 저장하고 CPU가 다음 프로세스를 수행하도록 새로운 프로세스의 상태 또는 레지스터 값(Context)을 교체하는 작업을 `Context Switching`이라 한다.

# **Intterupt(인터럽트)**

인터럽트는 CPU가 프로그램을 실행하고 있을 때, 실행 중인 프로그램 외부에서 예외 상황이 발생하여 처리가 필요한 경우 CPU에게 알려 예외 상황을 처리할 수 있도록 하는 것이다.

- **`Context Switching` 을 위한 인터럽트 종류**
    - `I/O request` : 입출력 요청
    - `time slice expired` : CPU 사용시간이 만료
    - `fork a child` : 자식 프로세스 생성
    - `wait for an interrupt` : 인터럽트 처리 대기

![https://user-images.githubusercontent.com/49153756/95016480-389f0380-068e-11eb-93c0-0da3b80b5f57.png](https://user-images.githubusercontent.com/49153756/95016480-389f0380-068e-11eb-93c0-0da3b80b5f57.png)

위 이미지는 공룡책에서 나오는 `Context Switching` 에 대한 이미지이다. `P0` 가 실행 중인데,`intterupt` 가 발생했다. 그럼 현재 실행 중이었던 `P0` 의 정보들(`PC(Program Counter)`, `SP(Stack Pointer)` 등의 레지스스터 정보)을 해당 프로세스의 `PCB` (여기서는 `PCB0`)에 저장한다. 그리고 `P1` 을 실행시키기 위해 `PCB1` 에 저장되어 있던 `P1` 의 정보를 가져와 프로세스를 실행시킨다.

이 때, `Context Switching` 이 진행되는 동안 CPU는 아무 일도 하지 못하기 때문에 `Context Switching` 이 잦아지면 `overhead`가 많이 발생하게 되어 효율이 떨어질 수 있다.

[ `Context Switching` 에서 `overhead` : `Context Switching` 에 걸린 시간과 메모리 ]

**그렇다면 이렇게 `overhead` 가 발생하게 되는데도 `Context Switching` 을 하는 이유는 뭘까?**→ 프로세스 수행 중 `I/O 이벤트` 가 발생해 `waiting` 상태로 전환시키는 경우, CPU가 아무런 일을 하지 않아 발생하는 낭비보다 `overhead` 가 발생하더라도 `Context Switching` 을 통해 다른 프로세스를 실행시키는 것이 더 효율적이기 때문이다.

# **PCB(프로세스 제어 블록)**

- 운영체제가 프로세스를 제어하기 위해 정보를 저장하는 자료구조이다.
- 프로세스 상태 관리와 `Context Switching`을 위해 필요하다.
- 모든 프로세스는 고유의 PCB를 가지며, 프로세스 생성 시 PCB가 생성되고 주기억장치에 유지되다가 프로세스가 완료되면 PCB는 제거된다.

### **PCB에서 유지되는 정보**

- PID : 운영체제가 각 프로세스를 식별하기 위해 부여된 프로세스 식별번호
- 프로세스 상태 : 준비, 대기, 실행 등의 상태 정보
- PC(프로그램 카운터) : CPU가 다음에 실행할 명령어를 가리키는 값
- Priority (스케줄링 우선순위) : 운영체제가 여러 개의 프로세스가 CPU에서 실행되는 순서를 결정하는 것을 스케줄링이라 하는데, 이 스케줄링에서의 우선순위 정보
- CPU 레지스터 및 일반 레지스터
- 메모리 관리 정보 : 해당 프로세스의 주소 공간 등의 정보
- 입출력 상태 정보 : 프로세스에 할당된 입출력 장치 목록, 열린 파일 목록 등
- 계정 정보 : CPU 사용 시간, 실제 사용된 시간 등
- 포인터
    - 부모 프로세스와 자식 프로세스에 대한 포인터
    - 프로세스가 위치한 메모리 주소에 대한 포인터
    - 할당된 자원에 대한 포인터
