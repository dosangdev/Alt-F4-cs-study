# CPU의 작동원리

## ALU와 제어장치

### APU

- ALU: 연산한 결괏값과 플래그를 내보낸다.
  - ALU는 레지스터를 통해 피연사자를 받고, 제어장치로부터 수행할 연산을 알려주는 제어신호를 받는다. 받아들인 2가지로 산술연산, 논리연산 등 다양한 연산을 수행한다. 연산을 수행한 결괏값은 일시적으로 레지스터에 저장된다.(메모리에 접근하는 속도보다 레지스터에 접근하는 속도가 더 빠르기 때문)
  - 플래그: 연산 결과에 대한 추가적인 상태정보.
    - 플래그 레지스터에 저장됨
      |플래그 종류|의미|사용 예시|
      |----|----|----|
      |부호 플래그|연산한 결과의 부호를 나타낸다.|1일 경우 계산 결과는 음수, 0일 경우 계산 결과는 양수를 의미|
      |제로 플래그|연산 결과가 0인지 여부를 나타낸다.|1일 경우 연산결과는 0, 0일 경우 0이 아님을 의미|
      |캐리 플래그|연산 결과 올림수나 빌림수가 발생했는지를 나타낸다.|1일 경우 올림수나 빌림수가 발생했음을 의미, 0일 경우 발생하지 않았음을 의미|
      |오버플로우 플래그|오버플로우가 발생했는지를 나타낸다|1일 경우 오버플로우가 발생했음을 의미, 0일 경우 발생하지 않았음을 의미|
      |인터럽트 플래그|인터럽트가 가능한지를 나타낸다.|1일 경우 인터럽트가 가능함을 의미, 0일 경우 불가능함을 의미|
      |슈퍼바이저 플래그|커널 모드로 실행 중인지, 사용자 모드로 실행중인지를 나타낸다.|1일 경우 커널모드로 실행중임을 의미, 0일 경우 사용자모드로 실행중임을 의미|

### 제어장치

- 제어장치가 받아들이는 정보

  - 첫째, 제어장치는 클럭 신호를 받아들인다.
    - 클럭(clock): 컴퓨터의 모든 부품을 일시불란하게 움직일 수 있게 하는 시간 단위
  - 둘째, 제어장치는 '해석해야 할 명령어'를 받아들인다.
  - 셋째, 제어장치는 플래그 레지스터 속 플래그 값을 받아들인다.
  - 넷째, 제어장치는 시스템 버스, 그중에서 제어버스로 전달된 제어 신호를 받아들인다.

- 제어장치가 내보내는 정보
  - CPU 외부에 전달하는 제어신호: 제어버스로 제어신호를 내보낸다.
    - 메모리에 전달하는 제어신호: 메모리에 저장된 값을 읽거나 메모리에 새로운 값을 쓰고 싶을 때
    - 입출력장치(보조기억장치 포함)에 전달하는 제어신호: 입출력장치의 값을 읽거나 새로운 값을 쓰고 싶을 때
  - CPU 내부에 전달하는 제어신호
    - ALU에 전달하는 제어신호: 수행할 연산을 지시하기 위해
    - 레지스터에 전달하는 제어신호: 레지스터 간 데이터를 이동시키거나 저장된 명령어를 해석하기 위해

## 레지스터

    프로그램 속 명령어와 데이터는 실행 전후로 반드시 레지스터에 저장된다. 따라서 레지스터만 잘봐도 프로그램의 실행 흐름을 파악할 수 있다.

- 상용화된 CPU 속 레지스터들은 CPU마다 이름, 크기, 종류가 매우 다양하다.

### 반드시 알아야 할 레지스터

- 프로그램 카운터: 메모리에서 읽어드릴 명령어의 주소를 저장한다. 명령어 포인터라고 부르는 CPU도 있다.

- 명령어 레지스터: 방금 메모리에서 읽어드린 명령어를 저장하는 레지스터. 제어장치는 명령어 레지스터 속 명령어를 받아들이고 이를 해석한 뒤 제어 신호를 내보낸다.

- 메모리 주소 레지스터: 메모리의 주소를 저장하는 레지스터. CPU가 읽어들이고자 하는 주소값을 주소버스로 보낼 때 메모리 주소 레지스터를 거치게 된다.

- 메모리 버퍼 레지스터(=메모리 데이터 레지스터): 메모리와 주고받을 값(데이터와 명령어)을 저장하는 레지스터. 메모리에 쓰고 싶은 값이나 메모리로부터 전달받은 값은 메모리 버퍼 레지스터를 거친다. CPU가 주소 버스로 내보낼 값이 메모리 주소 레지스터를 거친다면, 데이터 버스로 주고 받을 값은 메모리 버퍼 레지스터를 거친다.

- 범용 레지스터: 데이터와 주소를 모두 저장할 수 있고, 이름 그대로 다양하고 일반적인 상황에서 자유롭게 사용할 수 있는 레지스터. 일반적으로 CPU안에는 여러 개의 범용 레지스터들이 있고, 현대 대다수 CPU는 모두 범용 레지스터를 가지고 있다.

- 플래그 레지스터: ALU 연산 결과에 따른 플래그를 저장하는 레지스터.

- 스택 포인터

- 베이스 레지스터

### 특정 레지스터를 이용한 주소 지정방식(1): 스택 주소 지정 방식

- 스택 주소 지정 방식: 스택과 스택포인터를 이용한 주소지정 방식
  - 스택: 한쪽 끝이 막혀있는 통과같은 저장 공간이다. 그래서 가장 최근에 저장한 값부터 꺼낼 수 있다.
    - 메모라 안에 스택 영역이 있다. 이 영역은 다른 주소공간과는 다르게 스택처럼 사용하기로 암묵적으로 약속된 영역이다.
  - 스택 포인터: 스택의 꼭대기를 가리키는 레지스터. 즉, 스택에 마지막으로 저장한 값의 위치를 저장하는 레지스터

### 특정 레지스터를 이용한 주소 지정방식(2): 변위 주소 지정 방식

- 변위 주소 지정 방식: 오퍼랜드 필드의 값(변위)과 특정 레지스터의 값을 더하여 유효 주소를 얻어내는 주소 지정 방식

  - 변위주소 지정방식을 사용하는 명령어는 연산코드필드, 어떤 레지스터의 값과 더할지를 나타내는 레지스터 필드, 그리고 주소를 담고있는 오퍼랜드 필드가 있다.
  - 이때 오퍼랜드 필드의 주소와 어떤 레지스터를 더하는지에 따라 **상대주소 지정방식**, **베이스 레지스터 주소 지정방식** 등으로 나뉜다.

  - 상대주소 지정방식: 오퍼랜드와 프로그램 카운터의 값을 더하여 유효 주소를 얻는 방식
    - 프로그램밍 언어의 if문과 유사하게 모든 코드를 실행하는 것이 아닌, 분기하여 특정 주소의 코드를 실행할 때 사용된다.
  - 베이스 레지스터 주소 지정방식: 오퍼랜드와 베이스 레지스터의 값을 더하여 유효주소를 얻는 방식
    - 베이스 레지스터 속 기준 주소로부터 얼마나 떨어져 있는 주소에 접근할 것인지를 연산하여 유효주소를 얻어내는 방식

## 명령어 사이클과 인터럽트

### 명령어 사이클

- CPU가 하나의 명령어를 처리하는 과정에는 어떤 정해진 흐름이 있고, CPU는 그 흐름을 반복하며 명령어들을 처리해 나간다. 이렇게 하나의 명령어를 처리하는 정형화된 흐름을 **명령어 사이클**이라 한다.
- 처리 과정

  - 1.인출 사이클 - 명령어를 메모리에서 CPU로 가져오는 단계
  - 2.실행 사이클 - CPU로 가져온 명령어를 실행하는 단계(제어장치가 명령어 레지스터에 담긴 값을 해석하고, 제어신호를 발생시키는 단계)

  - 간접 주소 지정 방식처럼 명령어를 실행하기 위해서 메모리 접근을 한 번 더 해야 하는 경우는 **간접 사이클**을 추가한다

### 인터럽트

- CPU의 작업을 방해하는 신호
- 종류
  - 동기 인터럽트(=예외): CPU에 의해 발생하는 인터럽트. 가령 CPU가 실행하는 프로그래밍상의 오류와 같은 예외적인 상황에 마주쳤을 때 발생하는 인터럽트가 동기 인터럽트이다.
  - 비동기 인터럽트(=하드웨어 인터럽트): 주로 입출력장치에 의해 발생하는 인터럽트.
    - 처리 순서
      - 1.입출력장치는 CPU에 인터럽트 요청신호를 보낸다.
      - 2.CPU는 실행 사이클이 끝나고 명령어를 인출하기전 항상 인터럽트 여부를 확인한다.
      - 3.CPU는 인터럽트 요청을 확인하고 인터럽트 플래그를 통해 현재 인터럽트를 받아들일 수 있는지 여부를 확인한다.
      - 4.인터럽트를 받아들일 수 있다면 CPU는 지금까지의 작업을 백업한다.
      - 5.CPU는 인터럽트 백터를 참조하여 인터럽트 서비스 루틴을 실행한다.
      - 6.인터럽트 서비스 루틴 실행이 끝나면 4에서 백업해둔 작업을 복구하여 실행을 재개한다.

#### 용어 노트

- 인터럽트 요청 신호: "지금 끼어 들어도 되나요?"하고 물어보는 신호
- 인터럽트 플래그: 하드웨어 인터럽트를 받아들일지, 무시할지 결정하는 플래그. "불가능" 상태로 설정돼있는 플래그를 받더라도 정전이나 하드웨어 고장으로 인한 인터럽트는 막을 수 없다.
- 인터럽트 서비스 루틴(ISR: Interrupt Service Routine): 인터럽트를 처리하기 위한 프로그램. 인터럽트 핸들러라고도 한다. 입출력장치마다 다른 인터럽트 서비스 루틴을 가지고있다.
- 인터럽트 벡터: 각기 다른 인터럽트 서비스 루틴을 식별하기 위한 정보.

### 예외의 종류

- 폴트: 예외를 처리한 직후 예외가 발생한 명령어부터 실행을 재개하는 예외
- 트랩: 예외를 처리한 직후 예외가 발생한 명령어의 다음 명령어부터 실행을 재개하는 예외.
- 중단: CPU가 실행 중인 프로그램을 강제로 중단시킬 수 밖에 없는 심각한 오류를 발견했을 때 발생하는 예외.
- 소프트웨어 인터럽트: 시스템 호출이 발생했을 때 나타난다.
