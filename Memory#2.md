# **Memory-management strategies #2**



## Contiguous Memory Allocation 

- Memory Allocation 

  Memory 공간은 kernel memory 영역과 User memory 영역으로 나뉜다. 

- Contiguous Memory Allocation 

  연속한 Logical memory를 통재로 Physical memory로 옮기는 것이다. 

-  Multiple-partition allocation 

  Memory에 Process는 연속적으로 올라간다. 

  - Degree of multiprogramming 

    Memory에 올라갈 수 있는 Process 수 까지만 Memory를 Partition 할 수 있다. 

  - Variable-partition 

    각각의 Process가 차지한 공간을 Partition이라 하면 이 크기는 모두 다르다. 

  - Hole 

    Memory에서 할당되지 못하고 남아있는 공간을 말한다. Process 보다 Hole의 크기가 더 크면 Process를 올릴 수 있다. 

- External Fragmentation

  Total hole size 는 새로운 Process를 올릴 수 있지만 Hole이 Contiguous 하지않아서 Process를 올릴 수 없다. 

  - 50-percent rule 

    N개의 block이 생성되면 그 중 0.5N개 block이 생성된다. 결과적전체의 1/3이 사용불가능한 공간으로 남는다.   

- External Fragmentation Solutions 

  External Fragmentation을 발생하지 않게 하는 방법.

  - Compaction 

    나누어진 Hole을 모아준다. 

  - Noncontiguous

    Contiguous 방식을 사용하지 않는다. 대신 불연속적으로 Process를 Memory에 할당한다. 이를 Paging 이라 한다. 

    

## Paging 

External fragmentation이 생기지 않는 방식이다. 따라서 Compaction할 필요가 없다. 

-  Basic Method 

  Logical memory와 Physical memory를 같은 크기로 조각낸다. Process의 Frames을 Pages로 대응시킨다. 

  1. Frames 

     Logical memory의 조각난 영역을 말한다. 

  2. Pages 

     Physical memory의 조각난 영역을 말한다. 

  3. Page table 

     어떤 Page가 어떤 Frame을 가르키고 있는지 기록한 표다. 

     

- Page Number and Offset 

  이미 정해져 있는 Logical address를 Page Number와 Offset으로 나누어 기록할 수 있다. 

  Address의 bit 수  = Page의 자리 + Offset의 자리 

  

- Page size 

  

- Why Paging 

  Paging 으로 Physical에는 조각나서 올라가 있을 지언정 Logical memory는 이어져 있다. 

  1. Provides Transparency 

     Paging을 사용하면 Logical memory만 생각해주면 된다. 즉  Physical memory가 투명하다는 것이다. 

  2. No external fragmentation 

     Paging은 Non contiguous 하다. External fragmentation이 발생하지 않는다. 하지만 Internal fragmentation이 발생한다. 

  

- Contiguous vs Paging 

  MMU의 동작 방법에 차이가 있다. Contiguous는 시작 주소만 더해주면 되지만 Paging은 Page number와 Offset으로 분리시켜준 뒤 Page table을 읽어야 한다. 

  

- Free-frame management

  OS 는 비어있는 Page가 어디 있는지 알고 있어야 한다. 이를 위해 free-frame list를 만들어서 가지고 있다. 

  

- Internal fragmentation 

  가장 말단의 Page는 작은 공간을 가지고 있을 수 있다. 이때 해당 동간도 Page로 만들지만 Page속에 사용할 수 없는 공간을 말한다. 

  - 해결방법 

    Page size를 작게 해서 촘촘하게 Page 를 만든다. 하지만 이럴 경우 Page table이 커진다. 

    보통 Page size는 4 ~ 8KB를 사용한다. 

- Implementation of Page Table 

  각각의 Process는 고유의 Page table을 가지고 있다.  통상 Page table은 Main memory에 올라간다. 

- Page Table in Main Memory

  Logical address를 Physical로 변환하고 Physical에 접근한다. 

  즉 Logical address를 알아내기 위해 Memory에 한번 접근하고 다시 Physical에 접근하므로 총 2번 접근한다. 

  