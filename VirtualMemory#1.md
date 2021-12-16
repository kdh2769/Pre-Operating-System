# **Virtual Memory #1**

Virtual memory system이 무엇인지. Demand paging이 가장 중요하다. Page-replacement 알고리즘이 다양하다. 

## Background 

Virtual Memory와 Virtual Address는 다른 개념이다. 

- Virtual Memory 

  Logical (= Virtual) address는 한 Process 내에서 주소체계를 만든 것이다. Virtual memory는 DRAM과 별개로 존재하는 가상의 Memory다. DRAM 의 용량이 8GB라 해도 Virtual memory는 Disk를 사용해서 10GB의 용량이 될 수 있다. 

  이것으로 Logical address는 Physical address 보다 커도 Virtual memory에는 올릴 수 있다. 

- Swapping 

  Main memory 와 Disk 사이를 이동하는 과정을 말한다.  여기에 Swap in/out 이 있다. Swapping 할 땐 Whole process를 옮긴다. 

  1. Swap out 

     Main memory에서 Disk 로 보내는 과정이다. 이때 Process의 모든 정보를 보낸다. 

  2. Swap in 

     Disk에서 Main memory 로 정보를 가져오는 것을 말한다 .

- Swapping Context Switching Time 

  Swapping을 하면 DISK와 DRAM을 오가는 시간인 Context switching time이 존재한다. 최대한 DISK에서 DRAM으로 옮기는 시간을 줄이기 위해서 Demand Paging 을 사용한다. 

## Demand Paging 

Virtual memory의 DISK 영역에 Program 500MB가 있다면, 사용자가 이 Program의 모든 기능을 필요로 하지 않을 수 있다. 그렇다면 DRAM으로 Page in 할때 필요한 정보 100MB 정도만 올리면 된다. 즉 필요할 때 마다 부분적으로 Program을 올리는 것이다. 

- Demand Paging Concept 

  DRAM에 Program을 올리는 것엔 2가지 방법이 있다. 

  1. 모든 Program을 올린다. 
  2. 필요한 Page만 올린다. 이것이 Demand paging 이다. 

- Demand Paging 장점 

  1. Program을 부분적으로 올리니 빠르게 시작할 수 있다. 
  2. Program을 부분적으로 올리니 더 많은 Program을 올릴 수 있다.

- Pager

  Demand paging을 처리하는 역할이다. 

- Demand paging usage: Heap/Stack

  Process에서 Heap과 Stack 사이에 남는 Address 공간을 미리 Physical memory에 할당하면 그건 공간 낭비다. Heap 과 Stack 공간이 필요할 때 Demand paging을 사용할 수 있다. 

- Demand paging usage: Dynamically linked libraries (DLL)

  cv::Imread 는 Code 에 Library가 없기 때문에 읽는 순간 Library가 있는 Page를 가져온다. Library를 가져오기 위해 Stub으로 가서 Library에 접근한다. 

  - Stub 

    필요한 Library가 어디 있는지 그 정보들을 모아둔 것이다. 

- Valid-Invalid Bit  

  어떤 Page가 Memory에 있고 Disk 에 있는지 어떻게 아는가. valid-invalid bit 을 사용한다. 

  Hardware로 Valid-invalid bit이 구성된다. 처음엔 모두 i 로 초기화된다. 만약 Page table에서 i 에 접근하면 Page fault를 발생시킨다. 

  1. v : 현재 Memory에 있다. 
  2. i : 현재 Memory에 없다. 

- Page Fault 

  Invalid 한 Frame에 접근하면 Page fault를 발생시킨다. OS 에서 Trap을 발생시켜서 DISK 에 해당 Frame을 찾아온다. 그리고 Physical memory에 Frame을 채우고 Page table에 해당 Frame 주소를 새기고 i -> v로 업데이트 한다. 그리고 다시 Restart 한다. 

- Aspects of Demand Paging 

  Virtual memory 로 Disk를 Memory로 사용해도 어쨌든 Disk 에서 동작 한다. Disk는 정보를 저장하는 방법이 있다. Disk 는 오로지 File 형태만 읽을 수 있기 때문에 File system을 따라서 정보를 저장한다. 하지만 Memory에서 File을 저장할 땐 File system을 따르지 않기 때문에 Virtual memory의 Disk 영역은 Disk 가 접근할 수 없다. 

- Performance of Demand Paging 

  Page fault는 적게 일어나야 한다. 

  - EAT for Demand Paging 

    p : Page fault가 일어날 확률이다. 

    page fault time : Page out + Page in + restart overhead 이다. 

    EAT = (1-p) x memory access + p x (page fault time) 

  - Example 

    Memory access time을 200ns 라 하고 Average page-fault service time 을 8ms 라 하자.  p = 0.001 이라 한다면 

    EAT = 8200 ns 이다. Page fault 가 발생할때 마다 성능저하가 심해지는 걸 알 수 있다. 
    
  - How to Improve EAT
  
    - Anticipatory paging 
  
      Paging할 때 연관된 Page도 같이 가져온다. 

