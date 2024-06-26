# CPU 성능 향상 기법

## 빠른 CPU를 위한 설계 기법

### 클럭

- 컴퓨터 부품들은 ‘클럭 신호’에 맞춰 움직인다.
- CPU는 ‘명령어 사이클’이라는 정해진 흐름에 맞춰 명령어들을 실행한다.
- 따라서 클럭 신호가 빠르면 CPU를 비롯한 컴퓨터 부품들은 그만큼 빠른 박자에 맞춰 명령어 사이클을 빠르게 반복하고, 다른 부품들도 그에 맞춰 더 빠르게 작동한다
- 클럭 속도는 CPU 속도 단위로 간주된다.
- 클럭 속도는 헤르츠(Hz) 단위로 측정한다. 이는 1초에 클럭이 몇 번 반복되는지를 나타낸다.
- 클럭 속도를 무작정 높이면 발열 문제가 심각해진다. 클럭 속도 만으로 CPU의 성능을 올리는 것에는 한계가 있다.

### 코어와 멀티코어

- 코어는 명령어를 실행하는 부품을 말한다.
- 코어는 CPU내에서 여러개 있을 수 있는데 코어가 여러개 있는 CPU를 멀티코어 CPU 또는 멀티코어 프로세서라고 한다.
- 하지만 코어 수에 비례해서 CPU의 연산속도가 증가하지는 않는다.
- 코어마다 처리할 연산이 적절히 분배되느냐에 따라 연산 속도가 크게 달라진다.

### 스레드와 멀티스레드

- 스레드의 사전적 의미는 ‘실행 흐름의 단위’이다.
- 스레드에는 CPU에서 사용되는 하드웨어적 스레드가 있고, 프로그램에서 사용되는 소프트웨어적 스레드가 있다.
    - 하드웨어적 스레드: ‘하나의 코어가 동시에 처리하는 명령어 단위’이다. 하나의 코어로 여러 명령어를 동시에 처리하는 CPU를 멀티스레드 프로세서 또는 멀티스레드 CPU라고 한다.
    - 소프트웨어적 스레드: ‘하나의 프로그램에서 독립적으로 실행되는 단위’이다. 하나의 프로그램은 실행되는 과정에서 한 부분만 실행될 수도 있지만, 프로그램의 여러 부분이 동시에 실행될 수도 있다.
    - 멀티스레드 프로세서: 하나의 코어로 여러 명령어를 동시에 처리하는 기술인 하드웨어적 스레드이다. 설계는 복잡하지만 핵심 부품은 레지스터이다. 하나의 명령어를 처리하기 위해 필요한 레지스터를 여러개 가지면 된다.

## 05 -2 명령어 병렬 처리 기법

- 명령어를 동시에 처리하여 CPU를 쉬지 않고 작동시키는 기법이다.

### 명령어 파이프라인

- 명령어는 처리되는 과정에서 각 단계가 겹치지 않는다면 동시에 각 단계를 실행할 수 있는데 마치 공장 생산 라인처럼 명령어들을 명령어 파이프라인에 넣고 동시에 처리하는 기법을 명령어 파이프라이닝 이라고 한다.
- 파이프 라이닝이 특정 상황에서는 성능 향상에 실패하는 경우도 있는데 이러한 상황을 파이프라인 위험이라고 부른다. 파이프라인 위험은 크게 세 가지가 있다.
    - 데이터 위험: 명령어 간 데이터 의존성에 의해 발생. 데이터 의존적인 두 명령어를 무작정 동시에 실행하려고 하면 파이프라인이 제대로 작동하지 않는 것
    - 제어 위험: 프로그램 실행 흐름이 바뀌어 명령어가 실행되면서 프로그램 카운터 값에 갑작스러운 변화가 생겨 명령어 파이프라인에 미리 가지고 와서 처리중이었던 명령어들이 아무 쓸모 없어지는 것. 이를 해결하기 위해 프로그램이 어디로 분기할 지 미리 예측하고 주소를 인출하는 분기 예측이라는 기술을 사용한다.
    - 구조적 위험: 명령어들을 겹쳐 실행하는 과정에서 서로 다른 명령어가 동시에 ALU, 레지스터 등과 같은 CPU 부품을 사용하려고 할 때 발생. 자원 위험 이라고도 한다.

### 슈퍼스칼라

- CPU 내부에 여러 개의 파이프라인을 포함한 구조.
- 매 주기마다 동시에 명령어를 인출할 수도, 실행할 수도 있다.
- 이론적으로는 파이프라인 개수에 비례하여 프로그램 처리 속도가 빨라지지만, 실제로는 반드시 비례하여 빨라지지는 않는다.
- 파이프라인이 여러 개이다 보니 데이터 위험, 제어 위험, 자원 위험에 더 많이 노출되기 때문이다.

### 비순차적 명령어 처리

- 명령어들을 순차적으로 실행하지 않는 명령어 처리 기법
- 순차적으로 명령어를 실행하면 데이터 의존성이 있는 명령어 때문에 데이터 의존성이 없는 명령어도 기다려야 해서 명령어 파이프라인이 멈추게 되는 상황이 발생하는데 이런 경우에서 데이터 의존성이 없는 데이터를 먼저 실행하여 명령어 파이프라인이 멈추는 것을 방지하는 기법이다.

## CISC와 RISC

### 명령어 집합

- CPU가 이해하고 실행하는 명령어 들은 다 똑같이 생기지 않았다.
- 명령어의 세세한 생김새, 명령어로 할 수 있는 연산, 주소 지정 방식 등은 CPU마다 조금씩 다른데 CPU가 이해할 수 있는 명령어들의 모음을 명령어 집합 또는 명령어 집합 구조(ISA)라고 한다.
- ISA가 다른 CPU는 서로의 명령어를 이해할 수 없기 때문에 실행할 수 있는 파일도 다르다.
- ISA가 다르면 제어장치가 명령어를 해석하는 방식, 사용되는 레지스터의 종류와 개수, 메모리 관리 방법 등 많은 것이 달라지고 이는 곧 CPU 하드웨어 설계에 영향을 준다.

### CISC

- 복잡한 명령어 집합을 활용하는 컴퓨터를 의미한다.
- 다양하고 강력한 기능의 명령어 집합을 활용하기 때문에 명령어의 형태와 크기가 다양한 가변 길이 명령어를 활용한다.
- 상대적으로 적은 수의 명령어로도 프로그램을 실행할 수 있어 메모리 공간을 절약할 수 있다는 장점이 있다.
- 하지만, 활용하는 명령어가 워낙 복잡하고 다양한 기능을 제공하다 보니 명령어의 크기와 실행되기까지의 시간이 일정하지 않다는 단점이 있다.
- 그 말은 명령어 하나를 실행하는 데에 여러 클럭 주기를 필요로 한다는 것이고 이러한 규격화 되지 않은 명령어들은 파이프라이닝을 어렵게 한다.
- 또 복잡한 명령어는 사용 빈도가 낮고 사용되는 명령어만 주로 사용된다.

### RISC

- CISC에 비해 명령어의 종류가 적고 짧고 규격화된, 되도록 1클럭 내외로 실행되는 명령어를 지향한다.
- 그래서 명령어 파이프라이닝에 최적화 되어 있다.
- 메모리에 직접 접근하는 명령어를 두개로 제한할 만큼 메모리 접근을 단순화하고 최소화를 추구하여 CISC에 비해 주소 접근 방식의 종류가 적다.
- 메모리 접근을 최소화 하는 대신 레지스터를 적극 활용하여 레지스터를 이용하는 연산이 많고, 일반적으로 범용 레지스터의 개수도 많다.
- 다만 사용 가능한 명령어의 개수가 적다보니 CISC보다 많은 명령으로 프로그램을 작동시킨다.
