# **Deadlock #1**



## Deadlock Concept 

- Motivation 

  Multiprogramming 을 위해서 Process 개념이 도입되었다. 이 Process를 Memory에 동시에 올려 사용한다. 여러개의 Process간에 Data를 공유해야 했지만 불일치의 문제가 발생할 수 있기 때문에 Synchronize 해야 한다. 여기에 Critical section을 만들어서 semaphore, Mutex 등을 사용해서 접근에 제한을 주었다. 

  - Deadlock 

    서로의 Semaphore를 얻어야 하지만 동시에 과정을 수행 할 수 없어서 다음 과정으로 갈 수 없는 상태이다. 

    'release then I release'

- Bridge Crossing Example 

  One-lane Bridge 에 앞뒤로 자동차가 마주한 상황이 Deadlock이다. 

- Deadlock Problems Example

  1. Semaphore를 서로가 가지고 있어서 통과하지 못하는 상황. 
  2. Disk drive 와 Network card에서 서로의 장치를 반납하려 하지만 이미 각 장치가 점유하고 있는 상황도 Deadlock 이다. 

- Dining - Philosophers Problem 

  1. N명의 배고픈 철학자가 원탁 테이블에 둘러 앉는다.

  2. 모든 철학자가 왼쪽 포크를 왼손으로 집는다. 

  3. 모든 철학자가 오른쪽 포크를 오른손으로 집는다. 

     -> Deadlock 이 발생한다. 

## System Model  

- Deadlock: System Model 

  System Model이란 Process와 Resource의 관계를 Graph로 나타낸 것이다.  

  - Physical resource types 

    I/O devices, memory spaces, CPU cycles

  - Logical resource types 

    Semaphores, mutex locks, files

  - Instances

    Resource속에 여러개의 Instance를 가질 수 있다. 예를들어 Printer라는 Resource에는 여러개의 Instance가 있을 수 있다. 하지만 반드시 Resource type을 하나만 정의해야 하는 것은 아니다. 이는 사용자가 어떻게 표기하냐에 따라 다르게 설정할 수 있다. 
    
  - Resource type 

    Process는 Instance를 요청하지 않고 Resource type을 요청한다. 

  - Request, use, release

    System call을 호출해서 자원을 Request 하고 use 하고 release 한다. 

    

- Deadlock state 

  Process가 Resource를 Request 했지만 사용이 가능하지 않아서 Process가 Wait 상태로 간다. 이때 Wait 상태의 Process들이 절대 바뀔 수 없는 상황이다. 

  - 1번 정의 

    Process가 Wait state인데 절대 깨어날 수 없는 상황이다.

  - 2번 정의
  
    System내부의 Process들이 Deadlock 상태이다. 

## Deadlock Characterization 

4가지 필요 조건이 있다. 이 4가지 조건에 모두 만족해야 Deadlock 이 발생한다. 

- 4 necessary conditions 

  1. Mutual exclusion

     누군가 자원을 사용하고 있으면 누구든 사용할 수 없다. 

  2. Hold and wait 

     Semaphore 처럼 누군가 Semaphore를 가지고 있다면 다른 이는 기다려야한다. 

  3. No preemption

     Critical section에 누군가 들어가 있으면 강제로 뺏을 수 없다. 

  4. Circular wait 

     Wait 상태를 모두가 원형으로 기다리는 것이다. 

- Resource-Allocation Graph 

  - Graph model

    Nodes & Edges 로 나타낼 수 있다. 

    - Request edge 

      Process --> Resource type

    - Assignment edge 

      Instance --> Process

- Graph with A Cycle But No Deadlock 

  Cycle 이 있지만 Deadlock이 발생하지 않을 수 있다.

## Methods for Handling Deadlocks

- Deadlock Prevention 

  예방하는 방법은 절대 Deadlock 이 발생하지 않도록 하는 것이다. 이때 4가지 Conditions에 1가지라도 걸리지 않도록 한다. 

  1. No Mutual Exclusion 

     실현가능성이 없다. 이미 OS는 Mutual Exclusion을 기반으로 동작하고 있기 때문이다. 

  2. No Hold and Wait 

     Deadlock 은 Process가 한 자원을 사용하고 있으면 다른 Process가 사용할 수 없도록 한다. 

     - Total allocation 

       Process가 Resource를 필요한다면 모든 Resource가 동시에 Available 할 수 있을 때 Resource를 할당 받는다. 

       - 문제점 

         1. Low resource utilization이다. Resource가 빌 때 까지 기다려야 하므로 할당 받게될 다른 Resource도 같이 기다린다. 

         2. Starvation 

            자주 사용되는 Resource를 할당받기 위해 Process가 할당받는게 길어진다. 

  3. Allow Preemption 

     자원을 뺏는것을 허용한다. 하지만 Printer 같은 Resource는 동작중에 뺏을 경우 문제가 생긴다. 

  4. No Circular Wait 

     Resource마다 번호를 준다. Request를 오름차순으로만 할 수 있게한다.

- Summary of prevention schemes 

  지금까지 만들어온 OS를 퇴행하는 방법이다. 

  -> Deadlock avoidance 한다. 

