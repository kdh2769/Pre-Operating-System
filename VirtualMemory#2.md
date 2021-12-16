# **Virtual Memory #2** 



## Page Replacement 

- Page Replacement 

  Page fault가 발생해서 Disk 에서 Page를 가져오려 한다. 하지만 Memory에 Frame이 꽉 차있다. 이때 특정 Frame을 Page out 시키고. 필요한 Page를 Page Replacement 한다. 

-  Victim Frame 

  Page replacement를 수행할때 Page out 되는 Frame을 말한다. 

- Modify Bit 

  - If no frames are free

    Memory가 가득 찼을 때 Page replacement를 수행하면 Memory 와 DISK 사이에 Page 교환이 2번 발생한다. Memory에서 Page out 시킬때 1번,  Disk 에서 Page in 시킬때 1번 이렇게 총 2번이다. 

  - Solution 

    Modify Bit 을 만들어 0으로 초기화 시켜주고 해당 Frame에 수정 사항이 있다면 1 로 변화 시킨다. 만약 수정사항이 없다면 해당 Frame위에 Page replacement할 Page를 덮어 씌우면 된다. 이러면 Page out을 하지 않아도 되므로 1번만 Disk에 접근하면 된다.  이것으로 Page - transfer overhead 시간을 단축 시켜줄 수 있다.

- Benefits of Virtual Memory 

  Protection, Resource 활용, Transparency를 제공 한다. 

## Page Replacement Algorithms 

어떤 Frame을 Victim frame으로 선정할지 알고리즘이 존재한다. 

- Page and Frame Replacement Algorithms 

  알고리즘의 목적은 Page fault 가 적게 일어나도록 해야한다. 기본적으로 Frame의 수를 많이 확보하면 Page fault는 적게 일어날 것이다. 

- FIFO page replacement 

  먼저 들어온 Frame이 Victim frame 이 된다.

  - FIFO Illustrating Belady's Anomaly 

    FIFO를 사용하면 Frame 수가 늘어났지만 Page fault가 더 많이 발생하는 경우가 있다. 이는 Victim frame이  자주 사용하는 Frame 이라면 빈번하게 Page fault가 발생하기 때문이다. 

- Optimal Algorithm 

  앞으로 들어올 Page가 무엇인지 알고 있어서 가장 늦게 나올 Page를 Victim frame으로 선택하는 것이다. 

  - Impossible 

    하지만 어떤 Page가 늦게 나올지 알 수 없다. 따라서 Optimal Algorithm으론 알고리즘 지표의 Upper bound로 사용한다. 

- Least Recently Used (LRU) Algorithm

  가장 오랬동안 사용하지 않은 Frame을 Victim frame으로 삼는다. LRU 구현엔 Counter, Stack 두가지가 있다. LRU 구현엔 Hardware로 구현한다. 

  - Counter 

    해당 Frame이 사용되었을 때 시간을 Counter 로 기록한다. 가장 오래 머무른 Frame을 이 시간으로 판단해서 오래된 Frame이 Victim frame 이 된다.

    -  Counter Implementation 

      모든 Frame은 Counter 를 가지고 있다. 단점으론 Frame이 Replace 될때 마다 모든 Frame을 Search 해야한다. 이 Search 비용이 크다.  

  - Stack

    새롭게 시작되는 Frame을 Stack 에 쌓는 것이다. Victim frame으로 가장 밑에 있는 Frame을 삼는다. 

    - Stack Implementation 

      메모리 복사가 크다. 구현엔 더블 Linked list로 구현한다. 하지만 Search 할 필요없다. 

- LRU Implementation

  Counter 과 Stack 은 Memory reference 마다 업데이트 해줘야 한다. 

  - LRU Approximation 

    LRU는 아니지만 LRU 처럼 동작하게 하는 것이다. 이는 Reference bit 으로 구현한다. 

    - Second-Chance Page-Replacement Algorithm

      Frame이 처음 올라오면 Reference bit = 1 로 초기화 된다.  여기에 Next victim 이라는 Pointer가 이 Frame을 순회 하는데 Reference bit = 1 을 만나면 Reference bit = 0 으로 만든다. Reference bit = 0 을 만나면 걔를 Victim frame으로 선택한다.   

- Counting Algorithms

  - Least Frequently Used (=LFU) Algorithm

    LRU Algorithm 은 과거에 아주 많이 사용 되었더라도 최근에 사용되지 않는다면 Victim frame이 될 수 있다. 하지만 LFU는 사용된 횟수를 Count 한다. 가장 적게 사용된 Frame을 Victim frame으로 선택한다. 

    - Aging 

      2진수로 표기된 bit vector에서 해당 Frame이 사용될 때마다 가장 큰 자리수의 bit에 1을 넣어준다. 사용되지 않을 때마다 0을 넣어준다. 

  - Most Frequently Used (=MFU) Algorithm 

    특정 기간동안 가장 많이 사용된 Frame을 내보낸다. 이미 사용할 만큼 해서 내보낸다. 

## Sharing and Memory - Mapped Files 

- Fork()

  Process를 생성한다. 새로운 Process를 만들 때 Child Process 는 Parent Process와 같은 Page를 가리킨다. 값이 달라지는 순간 새로운 Page copy 해서 Memory에 올린다. 

  - Copy-on-Write 

    Fork()를 수행해주면 동일한 Physical memory 를 가리키도록 한다. 만약 달라지는 상황이 생기면 해당 Process가 새로운 Page를 가진다. 

- Memory - Mapped Files 

  File을 수정할 때 Disk 에서 read, write 하는 것이 느리다. 그래서 이 File을 Memory에 Mapping시켜서 속도를 빠르게 한다. 

- File Sharing via Memory-Mapped Files

  Memory - Mapped 된 File 들도 Sharing 이 가능하다. 

- Memory - Mapped I/O

  Device 는 Register를 가지고 있다. 이 Register 를 Memory 에 Mapping 시킨다. 그 다음 Memory에 값을 읽고 쓰는 것으로 Device의 값을 수정한다. 

