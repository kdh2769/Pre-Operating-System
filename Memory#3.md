# **Memory-management strategies #3**



- Page Table in Main Memory

  Paging 을 사용하면 Memory를 2번 참조해야한다. CPU가 Memory에 1번 접근하는 시간은 CPU가 100~400 cycle을 돌릴 수 있는 시간이기에 2번 접근하는 것은 문제가 된다.  

  Memory에서 CPU로 가기 전에 더 빠르게 CPU에 접근하는 Cache를 사용할 수 있다. MMU에서 Cache의 역할은 TLB가 한다. 

- TLB (Translation Look-aside Buffer)

  TLB 는 SRAM 처럼 Cache 이다. Page table만 담당하고 Cache 인 것이다. TLB는 비싸서 용량이 클 수 없다. TLB는 CPU 안에 들어있다. 원래 CPU는 CPU - Memory - Memory 로 접근 했다면 TLB가 들어가면 CPU - TLB - Memory 순으로 접근한다. 

  - TLB hit 

    TLB Page table에 내가 찾는 Address가  있는 상황이다. 

  - TLB miss

    TLB Page table에 내가 찾는  Address가 없는 상황이다. 이땐 오히려 TLB에 간 시간이 더 걸리게 된다. 

  - TLB의 도박성 

    TLB는 비싸기 때문에 저장할 수 있는 Page table의 크기는  32 ~ 1024KB 정도다. 따라서 TLB에 모든 Page table이 없기 때문에 찾고자 하는 Data가 있을 수 있고 없을 수 있다. 따라서 시간이 오히려 더 걸릴 수 있다.  
  
- TLB with Cache

  TLB는 MMU에서, Cache는 Memory에서 사용하는 용어다. 

  - Memory access order 

    1. TLB Check 

       TLB에서 내가 찾는 Address가 있는지 확인한다. 만약 없다면 Memory가서 Page table을 확인한다. 
    
    2. Cache Check 
    
       Cache에서 내가 찾는 Data가 있는지 확인한다. 만약 없다면 Memory 가서 Data를 찾는다. 
    
    

- Effective Access Time (EAT)

  TLB Check 와 Cache Check 의 과정을 거쳐서 Data을 얻을 때 평균적으로 어느정도의 시간이 걸리는가. 이를 위해서 Hit ration 를 사용한다. 

  - Hit ration 

    얼마만큼 TLB hit을 하는가. 이때의 값을 'a' 라 둔다면 TLB miss 값은 '1-a' 가 된다. 
    
  - Assume 

    TLB access time을 1ns , Memory access time 을 100ns 라 하자. 계산 해줄 때 Address 변환 후 Memory에 Data를 얻는 시간을 고려한다.  그렇다면 EAT = a * (1 + 100) + (1- a) * ( 1 + 100 + 100 )  임을 알 수 있다. 
    
    - EAT Calc 
    
      EAT = TLB Hit * (가장 빨리 걸리는 시간) + TLB MISS * (가장 느리게 걸리는 시간) 

- Memory Protection 

  Page table에서 자체적으로 접근할 수 없는 Address를 변환 해달라고 하는 경우를 차단할 수 있다. 이때 Valid - Invalid bit , PTLR을 사용한다.  

  - Valid - Invalid 

    Page table의 Address 마다 Valid - Invalid bit를 표기한다 . Valid bit 은 해당 Frame이 사용 가능 하다는 것이다. 

    - Invalid case 

      Invalid case에는 2가지 상황이 있다. 첫번째는 Memory 에 Process가 올라갔지만 Invalid 한 경우가 있다. 이는 해당 Frame 이 Disk 로 옮겨갔기 때문이다. 두번째로 아직 해당 Address가 할당되지 않았다는 것이다. 

  - PTLR 

    Page table의 저장 범위에 맞지 않는 주소가 들어오면 이를 막는다. 

- Shared Page (Shared Memory)

  Page table을 이용하여 Process간 Memory share를 할 수 있다. 이는 다른 Process의 Page 가 Page table에서 서로 같은 Frame 으로 변환해주면 된다. 

  - Shared code 

    Process간 Code 는 같이 사용하지만 Data만 따로 가지는 경우이다. 40개의 Process가 올라갔고 Process 하나당 Code 가 150KB 이고 Data가 50 KB 라면 총 8 MB 가 필요하다. 하지만 Code share를 한다면 2150KB 만 필요하다. 

- Why Paging 3 - Protectoin & Shared memory

  1. Paging provides protection to memory 

     1. Valid - Invalid 를 구분지어서 Page의 Frame 변환 유무를 가른다. 
     2. PTLR 바깥의 Bit는 Invalid 이므로 접근이 불가능하다.

  2. Paging procides shared memory 

     Page table 에서 같은  Frame으로 변환한다. 

## Structure of  the Page Table 

- How to Store Page Table 

  32bit, 64bit 운영체제에서 Frame size 는 2^12 로 대략 4KB 로 동일하다. 다른 점은 32bit 주소체계에선 Page table number은  2 ^20 으로 대략 100만 개이고 64bit는 2^52 이다.  대략 32 bit에선 4MB 정도 Page table 이다. 

  - Hierarchical paging 

    Page table은 Contiguous 하기 때문에 Page table을 다층 구조로 만드는 것을 Hierarchical paging 이라 한다. 따라서 Page table number 에 또 다른 Page table을 만드는 것이다. 

- Two - Level Paging Example 

  32 bit 운영체제에서 Page number 가 2 ^ 20 이라면 2 ^ 10, 대략 1KB 을 Inner page table의 Offset 으로 두고 2 ^ 10을 Inner page table의 number로 잡을 수 있다. 
  
- Hased Page Table 

  Page number에 해당하는 부분을 Key값으로  Hased page table에 검색하면 해당 Page number로 Address 값을 찾을 수 있다. 

- Inverted Page Table 

  Page 번호로 Frame 을 찾는 것이 아니고 Frame 번호로 어떤 Process 가 사용중인지 그 Pid 값과 Page를 찾는 것이다. Page table 입장에서 관리하는 것이다. 
  
  - 단점 
  
    Page table을 다 찾아서 Pid 를 찾아야 한다. 그리고 Shared memory가 불가능하다. 

## Segmentation 

사용자 관점에서 Memory를 Segment화 해서 관리한다. Segment의 집합이 Program 이다. 

- Paging 의 문제 

  한 Process를 Physical memory에 Paging하면 Page 안에 Text, Data같이 다른 영역의 내용이 들어갈 수 있다. 이렇게 되면 Memory Share 가 불가능하다.  
  
- Segmentation Architecture

  Stack, Heap, Text, Data 처럼 같은 부류로 묶는 것이 Segment 이다. Segment의 주소 체계는 <segment-number, offset> 이다. Segment의 크기는 다 다르기 때문에 Table에 Contiguous한 방식으로 저장된다. 이때 Table에는 Segment-table base register  (STBR)과 Segment-table length register (STLR) 값도 저장된다. Memory에 할당 될 때는 Segment 가 통째로 저장된다. 

  - Relocation 

    Code가 돌아갈 때 dynamic 하게 segment table에 맞게 할당한다. 

  - Sharing 

    같은 Segment에 있으면 Share가 가능하다. 

  - Allocation 

    1. First fit 

       Memory 에 있는 Hole 중 찾다가 먼저 찾는 Hole에 할당한다.

    2. Best fit 

       여러 Hole 중 External fragment가 가장 작은 Hole에 할당한다.

  - Protection 

    Segment table에도 Valid - Invalid bit이 있다. 이것으로 권한을 설정한다. Segment도 잘 사용하지 않으면 Disk로 옮길 수 있다. 

  - 문제 

    Segment는 Memory에 할당할 때 시간이 오래걸릴 수 있다. 

  - Example of Segmentation 

    Limit > Offset 해야 한다.  만약 Limit < Offset 이라면 Trap 을 발생 시킨다. 
  
  - Advantage of Segmentation 

    1. Handling 하기 용이하다. 
    2. Segment가 같은 특성을 가지고 있기 때문에 Share , Protect가 용이하다. 
    3. Internal fragmentation 이 발생하지 않는다. 
  
  - Disadvantage of Segmentation 

    1. External Fragmentation이 발생할 수 있다. 
    2. Table 크기가 커질 수 있다. 
    3. Segment간 접근이 어려워서 Indirect addressing 을 사용해야 한다. 

  - Hybrid approach 

    현실은 Paging 과 Segmentation 기법을 같이 쓴다. 
  
    Process는 Segment화 하고 Segment를 Memory에 할당할 땐 Paging 한다. 이렇게 Exteral fragment를 만든다.  Memory를 3번 접근해야한다. 
  

