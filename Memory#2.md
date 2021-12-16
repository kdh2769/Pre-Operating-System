# **Memory-management strategies #2**



## Contiguous Memory Allocation 

- Memory Allocation 

  User process가 Physical memory에 할당되는 것을 말한다. 어디에 할당 시킬 것인가. (앞에서 MMU에 관해서 배운 것은 Address 변환을 살펴 본 것이다. )

- Contiguous Memory Allocation 

  Logical memory를 연속해서 통째로 Physical memory로 옮기는 것이다. 

  - Multiple-partition allocation 

    Contiguous 방식에선 Memory에 Process가  연속적으로 올라간다. 

    - Degree of multiprogramming 

      Memory에 올릴 수 있는 Program  최대 개수를 의미한다.    

    - Variable-partition 

      Process마다 차지하는 Memory 공간이 다른 것을 의미한다.

    - Hole 

      Memory에서 할당되지 않고 남아있는 공간을 말한다. Process 보다 Hole의 크기가 더 크면 Process를 올릴 수 있다. 

    

- External Fragmentation

  Total hole size는 Memory 상의 모든 Hole size를 더한 것이다. Total hole 상으로는 Process를 할당할 공간이 있지만 Hole 이 Contiguous 하지 않아서 Process 를 할당할 수 없는 이런 것을 External Fragmentation 이라 한다.  

  - 50-percent rule 

    N개의 block이 생성되면 그 중 0.5N(절반) block이 External fragmentation 이 된다.
    
    - Example
    
      전체 150 MB 중 100MB가 Process 로 사용되면 50 MB이 External fragmentation 이 된다. 결과적전체의 1/3이 사용불가능한 공간으로 남는다.   

- External Fragmentation Solutions 

  External Fragmentation은 Memory 낭비가 심하기 때문에  발생하지 않게 해야한다.

  1. Compaction 

     Hole 이 생기면 매꿔주기 위해 Process들이 이동하는 것이다. Process들이 자리를 이동하는 것은 Binding을 Dynamic 하게 받아야 한다.  Load time 시 Binding 하는 방식이라면 Compaction 사용이 가능하다. 하지만 Process를 전부 이동시키기 때문에 자원낭비가 커서 잘 사용하지 않는다.

  2. Noncontiguous

     Contiguous Allocation을 사용하지 않고 불연속적으로 Process를 Memory에 할당한다. 이를 Paging 이라 한다.

## Paging 

Paging은 Noncontiguous memory management 이다. 따라서 External fragmentation이 생기지 않는 방식이다. 따라서 Compaction할 필요가 없다. 

- Basic Method 

  Logical memory와 Physical memory를 Fixed size chunk로 나눈다. Page와 Frame을 대응시킨다.  

  1. Frame

     Physical memory 에서 다루는 단위다. Frame은 불연속적이다.  

  2. Page

     Logical memory 에서 다루는 단위다.  Page는 연속적이다. 

  3. Page table 

     어떤 Page가 어떤 Frame을 가르키고 있는지 기록한 표다. Memory의 Kernel 공간에 있다. 

     

- Page Number and Offset 

  Page table을 통해 Page와 Frame을 매칭시켜주었다. 실재 Data address는 Page number를 정해주고 그 안에 어디 인지도 정해줘야 한다. 

  - Offset

    Offset이란 그 Page 안에 몇번째 Byte 인지 가리킨다. 이때 Page와 Frame 의 Offset은 동일하다. 

- Page number 

  Page number 는 Logical address space 와 Offset size에 따라 결정된다. Logical address space : n , Offset(=frame, page) size : m 이라면 Page number 는  n-m 이 된다. 

  2 Byte(= 16 Bit)의 Logical address에서 Offset size가 4 bit 라면 Page size  는 12 bit 이다. 

- Why Paging 1 - Provides Transparency  

  Paging은 Transparency 하기 때문에 Physical address에 Process가 조각난 채로 올라갔어도 프로그래머는 신경 쓰지 않고 Logical memory만 생각해주면 된다.  

  - Transparency

    투명하기 때문에 신경 쓰지 않아도 된다는 뜻이다. 

- Contiguous vs Paging 

  Contiguous의 장점은 Page table을 만들지 않고 시작 주소인 Relocation register 값만 알고 있으면  된다. MMU에서 Relocation register만 Logical address의 시작 주소를 더해주면 된다. 

  Paging일 경우 Local address를 Offset과 Page size로 분리시키고 Page table에서 Physical address로 변환 시킨다.  

- Free-frame management

  Paging 을 하기 위해서 OS 는 비어있는 Page가 어디 있는지 알고 있어야 한다. 이를 위해 Free-frame list가 있다.  

- Why paging 2 - No external fragmentation 

  Paging은 Logical address 와 Physical address를 규격화 시킨 다음 Noncontiguous하게 Memory allocate 한다. 이로 인해 External fragmentation이 발생하지 않는다. 

  하지만 Internal fragmentation 이 발생한다.

  - Internal fragmentation

    Process를 Paging 시켰을 경우 가장 마지막 Page가 Page size와 같지 않다면  Memory 공간이 남게된다. 이 공간을 Internal fragmentation이라 한다.  

- Internal fragmentation solution 

  Page size를 작게 해서 Internal fragmentation이 작게 발생하게 한다. 하지만 Page size가 작으면 Page table이 커지는 문제가 있다. 

  보통 Page size는 4 ~ 8KB를 사용한다. 

- Implementation of Page Table 

  Process는 개별적으로 Page table을 가지고 있다.  통상 Page table 은 Main memory에 올라간다. CPU가 Page table을 읽는다. 

  - Page-table base register

    Page-table이 시작한 위치 주소다. 

  - Page-table length register 

    Page-table의 길이 주소다. 

  PTLR, PTBR 은 CPU register에 저장되어 있다. 

- Page Table in Main Memory 

  Physical address로 변환 과정에서 Page-table 정보를 얻기위해 Memory를 1번 가고 다시 해당 주소에 접근하기 위해 Memory에 1번 더 접근한다. 

  결과적으로 2번 접근해서 시간이 오래 걸리는 문제가 발생한다. 

  