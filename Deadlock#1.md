# **Deadlock #1**

Deadlock이 무엇인지. 특성. 예방방법과 피하는 방법을 알아보자. 

## Deadlock Concept 

- Motivation 

  Multiprogramming 을 위해서 Process 개념이 도입되었다. 여러개의 Process가 Data를 공휴하는데 불일치의 문제가 발생할 수 있다. 이는 Synchronize 기법으로 해결 한다. 여기에 Critical section을 만들어서 semaphore, Mutex 등을 사용해서 접근에 제한을 주었다. 

  - Deadlock 

    서로의 Semaphore를 얻어야 하지만 동시에 과정을 수행 할 수 없어서 다음 과정으로 갈 수 없는 상태이다. 

    'release then I release'

- Bridge Crossing Example 

  One-lane Bridge 에 앞뒤로 자동차가 마주한 상황이 Deadlock이다. 마주선 자동차는 서로가 지나갈 수 없다. 

- Deadlock Problems Example

  1. Semaphore를 서로가 가지고 있어서 통과하지 못하는 상황. 
  2. Process 1 이 Disk drive 를 사용하고 있고 Process 2 는 Network card 를 사용하고 있다. Process 1 이 해당 장치를 반납하려 할 때 Network card 를 필요로 하고 Process 2 가 장치를 반납하려 할 때 Disk drive를 필요로 한다. 이 상황도 Deadlock 상황이다. 

- Dining - Philosophers Problem 

  1. N명의 배고픈 철학자가 원탁 테이블에 둘러 앉는다.

  2. 모든 철학자가 왼쪽 포크를 왼손으로 집는다. 

  3. 모든 철학자가 오른쪽 포크를 오른손으로 집는다. 

     -> Deadlock 이 발생한다. 
  
- How can we solve this ? 

  Deadlock Prevention 과 Deadlock Avoidance 로 해결한다. 

## System Model  

- Deadlock: System Model 

  System Model이란 Process와 Resource의 관계를 Graph로 나타낸 것이다. 이것으로 Deadlock 이 발생한 System 내부의 문제를 살펴 볼 수 있다.  먼저 Resource 는 무엇이냐. 

  - Physical resource types 

    I/O devices, memory spaces, CPU cycles, print device ,, etc

  - Logical resource types 

    Semaphores, mutex locks, files,, etc. Semaphore 가 Critical section 에서 문을 막고 여는데 사용되었기 때문에 이것도 Resource로 볼 수 있다. 

  - Instances

    Resource속에 여러개의 Instance를 가질 수 있다. 예를들어 Printer라는 Resource에 여러대의  Printer Instance가 있을 수 있다. 하지만 반드시 Resource type을 하나만 정의해야 하는 것은 아니다. 이는 사용자가 어떻게 표기하냐에 따라 다르게 설정할 수 있다. 
    
  - Process requests an Instance of a Resource type 

    Process는 Instance를 요청하지 않고 Resource type을 요청한다. 

  - Request, use, release

    System call을 호출해서 자원을 Request 하고 Use 하고 Release 한다. 

    

- Deadlock state 

  Process가 Resource를 Request 했지만 사용이 가능하지 않다면 Process가 Wait 상태다. 이때 Deadlock 이 발생한다는 것은 Wait 상태의 Process들이 절대 바뀔 수 없는 상황이다. 

  - 1번 정의 

    Wait state의 Process가 절대 깨어날 수 없는 상황이다.

  - 2번 정의
  
    System내부의 Process들이 Deadlock 상태이다. 

## Deadlock Characterization 

Deadlock 이 발생하기 위한 4가지 필요 조건이 있다. 이 4가지 조건에 모두 만족해야 Deadlock 이 발생한다. 하지만 반드시 4 가지 조건을 만족한다고 해서 Deadlock 이 발생하는 것은 아니다. 

- 4 Necessary Conditions 

  1. Mutual exclusion

     누군가 자원을 사용하고 있으면 누구든 사용할 수 없다. 

  2. Hold and Wait 

     Process가 Semaphore를 가지고 있는 상태에서 다른 Process는 Semaphore를 기다리고 있는 것이다. 

  3. No preemption

     Critical section에 들어가 있으면 누군가 강제로 뺏을 수 없다. 

  4. **Circular wait** 

     Wait 상태의 Process가 원형으로 기다리고 있으면 Deadlock 에 빠질 가능성이 커진다. 

- Resource-Allocation Graph 

  System model을 통해서 그려보자. 

  - Graph model

    Nodes & Edges 로 나타낼 수 있다. Node를 Process와 Resource type, Edge는 Vertex 사이의 관계를 나타낸다. 

    - Request edge 

      Process --> Resource type

    - Assignment edge 

      Process --> Instance 

- Graph with A Cycle But No Deadlock 

  Cycle 이 있지만 Deadlock이 발생하지 않을 수 있다. 그리고 여러개의 Instance가 Resource type에 존재한다면 Deadlock 이 발생할 수도 발생하지 않을 수 도 있다.  
  
  확실하게 Deadlock 을 만들어주지 않는 방법은 Cycle 을 만들지 않는 것이다. 

## Methods for Handling Deadlocks

- Deadlock Prevention 

  Deadlock 을 예방하는 방법은 절대 Deadlock 이 발생하지 않도록 하는 것이다. 여기선 4가지 Conditions 하나라도 걸리지 않도록 한다. 

  1. No Mutual Exclusion 

     실현가능성이 없다. 이미 OS는 Mutual Exclusion을 기반으로 동작하고 있기 때문이다. 

  2. No Hold and Wait 

     Deadlock 은 Process가 한 자원을 사용하고 있으면 다른 Process가 사용할 수 없도록 한다. 이걸 못하게 하자는 것이다. 

     - Total allocation 

       Process가 Resource를 필요한다면 모든 Resource가 동시에 Available 할 수 있을 때 Resource를 할당 받는다. 

       - Example 

         어떤 Process가 DVD drive, Print drive, Disk 를 필요로 한다고 하면 3가지 Device 가 다 Available 할 때 Process 해당 장치를 할당받는 것이다. 

       - 문제점 

         1. Low resource utilization 이다. Resource 가 중간에 쉬는 시간이 발생한다. 
         2. 어떤 Process가 많은 Resource를 할당 받아야 한다면 그 Process는 Resource를 할당받기 어렵다. 

     - No hold and Wait 

       Process 가 Resource type 을 할당받았다면 Request 를 하지 못하게 하는 것이다. 

       - 문제점 

         1. Low resource utilization이다.  

         2. Starvation 

            자주 사용되는 Resource를 할당받기 위해 Process가 할당 받는 시간이 길어진다. 

  3. Allow Preemption 

     자원을 뺏는것을 허용한다. 하지만 Printer 같은 Resource는 동작중에 뺐을 경우 문제가 생긴다. 

     - 문제점
       1. Printer 같은 경우 중간에 Preemptive 가 되면 문제가 생긴다. 
       2. Synchronization 문제가 발생한다. 

  4. No Circular Wait 

     Resource마다 번호를 준다. Request를 오름차순으로만 할 수 있게한다.

- Summary of prevention schemes 

  지금까지 만들어온 OS를 퇴행하는 방법이다. 

  

