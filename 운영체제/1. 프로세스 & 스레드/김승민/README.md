# Process & Thread

**프로세스 & 스레드**


> 
이번 챕터에서는 프로세스(Prcoess)와 스레드(Thread)에 대해서 알아보도록 하겠습니다.                   
추가적으로 이와 관련된 면접 질문들과 우리가 사용하는 노드에서의 프로세스와 스레드는 어떻게 동작하는지에 대해서도 조사해보았습니다.

## 🤵🏻‍♂️ 프로세스 ( Process )

## 📌 프로세스란?

> 프로그램이 메모리에 적재되어 CPU를 할당받아 실행되는 것입니다.
> 

⇒ 프로세스 관련 질문이 나올 경우, 메모리와 CPU관점으로 설명하는 것이 유리하다.

- 우리가 짠 code(program)은 HDD에 있다.
- 그걸 실행하려면 CPU에서 실행해야 하는데, CPU는 HDD에 있는 프로그램은 읽을 수 없다.
- 따라서, 실행하려는 프로그램을 RAM 메모리에 적재한다.
- RAM 메모리에 적재되면 CPU는 이 프로세스를 한줄한줄 읽어나가며 연산한다!

**⇒ 프로그램이 RAM 메모리에 적재되며 CPU 할당도 받으면, 이게 프로세스가 되는 것.**


프로세스 내에서도 각 용도에 따라, 다음과 같이 메모리 영역이 구분된다.

![](https://velog.velcdn.com/images/turfguy/post/6c541244-87f2-44d4-bdff-f00bcc3757e5/image.png)
![](https://velog.velcdn.com/images/turfguy/post/a74435c9-0d70-4ca2-a5e9-2fecf8e40f4a/image.png)


- **stack:** 지역변수 / 매개변수
- **heap**: 런타임 중 할당된 동적메모리 ( Malloc/ free )
- **data:** 전역변수
- **code:** 실제로 우리가 작성한 코드 (컴파일 한 이후, 기계어로 번역된 코드로)

**⇒ 프로세스는 결국 실행중인 프로그램을 뜻한다. 실행 파일 형태로 HDD에 저장되어 있던  프로그램이,**

**메모리에 적재된 이후 CPU를 할당받아서 한줄 한줄 읽혀나가도록 만든 것이 프로세스!**


> 💡 자주 언급된 **메모리 적재/ CPU할당** 이 부분에 대해서 조금 더 알아보도록 하겠습니다.


### 💾 Memory에 적재

- 프로세스의 개념에 대해 설명할 때, 우리가 작성한 프로그램은 HDD에 저장된다고 언급했다.
    
    그래서 우리는 이 프로그램을 메모리에 적재시킨다. 왜?
    
    **메모리는 CPU가 직접 접근할 수 있는 컴퓨터 내부의 기억장치이므로**
    

### 💾 CPU 연산

- 프로그램을 토대로 CPU가 실제로 연산을 해야만, 프로그램이 실행되는 것
    
    어떤 코드를 실행할지는 CPU 내부의 PC register(Program Counter) 에 저장되어있음.
    
    메모리에 적재된 프로세스 코드 영역의 명령어 중, 읽어야 하는 줄을 pc register가 가르키게 된다.
    
    이를 순차적으로 CPU가 가져와서 명령어를 실행, 연산 하면 ⇒ Process
    

## 📌 멀티 프로세스(Multi Process)

> 멀티 프로세스란, 2개 이상의 프로세스가 동시에 실행되는 것입니다.
’동시에’ 는 **동시성(concurrency)** 과 **병렬성(parallelism)** 을 생각해야 합니다.
> 
- **동시성 :**
    - CPU core가 1개일 때, 여러 프로세스를 짧은 시간동안 번갈아가며 연산하는 시분할 시스템으로 동작
- **병렬성 :**
    - CPU core가 여러 개일 때, 각각의 코어가 프로세스를 실행함으로서, 동시에 실행되는 것을 의미

⇒ 우리가 쓰고있는 PC들의 CPU은 현재 대부분 코어가 여러개입니다. 그래서 병렬성의 측면은 너무 쉽게 설명할 수 있습니다. 코어가 각자 자기가 맡은 연산을 수행하면 동시에 작동하는 것이기 때문입니다.

하지만, 동시성에 대해 생각해봐야합니다. 한 코어가 짧은 시간 동안 여러 작업을 번갈아가며 계속 수행하는 것! CPU는 매 순간 하나의 프로세스만 연산할수 있습니다. CPU의 연산속도가 매우 빠르기 때문에 동시에 처리 하는 것처럼 보이는 것 뿐.. 

이때, CPU의 작업시간을 여러 프로세스가 조금씩 나누어 쓰는 것을 **시분할 시스템**이라고 부른다!

### 💾 Context

시분할 시스템에서 한 process는 매우 짧은 시간동안 CPU를 점유, 일정 부분의 명령을 수행한다.

그리고 수행한 뒤 다른 프로세스에게 넘기고 본인 차례가 오면 다시 명령 수행..

따라서, 이전에 어디까지 작업을 수행했고 pc레지스터에는 어떤 값이 저장되어 있는지에 대한 정보 등

프로세스가 현재 어떤 상태로 수행되고 있었는지에 대한 총체적인 정보 ⇒ **Context**

context는 pcb (Process Control Block)에 저장됩니다.

### 💾 Context Swithcing

한 프로세스에서 다른 프로세스로 CPU 제어권을 넘겨주는 것.

이전의 프로세스 상태를 PCB에 저장해서 보관하고 새로운 프로세스의 PCB를 읽어 보관된 상태를 파악하여

복구한다.

## 📌 멀티 스레드(Multi Thread)

> **Thread**란? 프로세스 내에서 실행되는 동작,기능(function)의 단위이다.
각 스레드는 속해있는 프로세스의 스택 메모리를 제외한 나머지 메모리를 공유한다.
**Multi Thread**란? 하나의 프로세스가 동시에 여러 일을 할수있도록 해주는것
**⇒ 한 프로세스 내의 여러개의 thread가 있고, 각 스레드들은 스택 메모리를 제외한 나머지 영역 공유.**
> 

## 📌 싱글 스레드(Single Thread)

> 싱글 스레드는 말 그대로 스레드가 하나 뿐이라는 것을 의미한다. 스레드를 이해하기 위해 필요한 개념은 프로세스와 스레드의 차이이다.
> 
- **프로세스**

운영체제에서 할당하는 작업의 단위이다. 노드나 웹 브라우저와 같은 프로그램은 개별적인 프로세스이다.

프로세스 간에는 메모리등의 자원을 공유하지 않는다.

- **스레드**

프로세스 내에서 실행되는 흐름의 단위이다. 프로세스는 스레드를 여러 개 생성, 여러 작업을 동시에 처리할 수 있다. 스레드들은 부모 프로세스의 자원을 공유한다. 같은 주소의 메모리에 접근 가능하므로 데이터를 공유할 수 있다. **스레드**를 작업을 처리하는 '일손'으로 표현하기도 한다.

➡️***쉽게 정리해보면, 한 os내 여러개의 프로세스를 두고 작업을 할당한다. 이 프로세스들은 서로 자원을 공요하지 않는다. 그리고 각 프로세스는 N개의 스레드를 가지고 있다. 같은 프로세스에 속해있는 이 스레드들은 부모격 프로세스의 자원을 서로 공유한다. 이는  같은 주소의 메모리에 접근이 가능하기 때문이다.***

노드의 스레드는 어떨까? 다들, 노드가 싱글 스레드 기반이라는 말을 많이 들어봤을 것이다.

나 역시 이유는 몰라도 노드가 싱글 스레드라는 사실만은 알고있었다. ***하지만 엄밀히 말하면 노드는 싱글 스레도 작동하지는 않는다고 한다.*** 노드를 실행하면 먼저 프로세스가 하나 생성되고, 그 프로세스에서는 여러개의 스레드들을 생성한다. (멀티 스레드인 셈)

**하지만! 그 중에서 우리가 직접 제어할 수 있는 스레드는 하나 뿐이기에, 노드를 '싱글 스레드'라고 부르는 것이다.**

➡️ ***노드의 프로세스는 멀티 스레드이지만, 직접 다룰 수 있는 스레드는 하나이기 때문에 "싱글 스레드"이다.***

언뜻 보면, 여러 개의 작업을 동시에 처리할 수 있으므로 멀티 스레드가 싱글 스레드보다 유리해보인다.

하지만 꼭 그렇지만은 않다. 우리는 각 스레드 방식에 어떤 블로킹 방식을 적용할지를 고려해보아야 한다.

## 📌  노드의 스레드 동작방식

🔴 ***싱글스레드 + 블로킹***

음식점에 손님이 여러명, 한 명의 점원이 첫번째 손님의 주문을 받고 서빙까지 마친 뒤 다음 손님의 주문을 받는 방식. ***매우 비효율적이다.***

🟢 ***싱글스레드 + 논 블로킹***

음식점에 손님이 여러 명, 한 명의 점원이 한 손님의 주문을 받은 뒤 주방에 주문 내역을 넘긴다. 서빙할 때 까지 기다리지 않고, 다음 손님의 주문을 받는다. 주방에서 요리가 완료되면 완료된 순서대로 손님에게 서빙한다. 논 블로킹의 특성상, 주문이 들어온 순서와 서빙하는 순서는 일치하지 않을 수 있다.

***이 방식이 노드가 채택하고 있는 방식이다.***

🔴 ***멀티스레드 + 블로킹***

멀티스레드를 이 예시에 적용하면, 손님이 한 명 올 때마다 점원도 한 명씩 붙어 주문을 받고 서빙하는 것과 같다. 싱글스레드보다 효율적이라고 생각할 수 있으나, 장단점이 명확하다. 일단 손님 한 명당 점원이 한 명 붙기 때문에 서빙에 문제될게 없다. 또한 직원 한명이 사라져도 대체할 수 있다.

하지만, 손님 수가 늘어난다면 점원 수도 그에 맞춰 1:1로 늘려야 한다. 뿐만 아니라, 손님 수가 줄어들었을 때 나머지 점원들은 일을 하지않고 놀고 있어야한다. 점원을 새로 고용/해고하는데는 비용이 발생하기 때문에, 이 방식은 효율적이라고 하기 힘들다.

🔴 ***멀티스레드 + 논 블로킹***

이론 상, 멀티 스레드(여러명의 점원)이 논 블로킹 방식으로 주문을 받으면 더 좋지 않을까? 라는 의문이 들 수 있다. 실제로 그렇다. 다만, 멀티 스레드 방식으로 개발을 하는 것은 상당히 어려우므로 멀티 프로세싱 방식을 택한다.

I/O 요청에는 멀티 프로세싱이 실제로 더 효율적이기도 하다.