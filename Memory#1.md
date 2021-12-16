# **Memory-Management Strategies #1**



메모리 하드웨어를 어떻게 구성할 것인가. 실재 메모리를 관리하는 기법의 장단점을 Logical memory, paging, mmu 을 통해서 보자. 

## Background

- Memory Technology 

  메모리를 속도와 금액으로 분류할 수 있다. 위로 갈 수록 빠르지만 비싸고. 밑으로 갈 수록 느리고  싼 구조이다. 이런 구조를 Hierarchical 하다고 한다. 

  계층별로 메모리를 나누는 기준 외에 메모리의 휘발성으로 나눌 수 있다. (SRAM - DRAM - Flash memory / Magnetic disk ). 

  메모리를 관리해주는 이유는 메모리 세상에서 SRAM 보다 빠르고 비휘발성이면서 가격이 싼 Memory는 없다. 

- DRAM 

  Main memory라 하면 CPU와 직접 주고받고 있는 것이다. 현재 Main memory 는 DRAM 이다. 

  - Dynamic random access memory 

    무작위(원하는 위치)로 저장 위치에 접근할 수 있다. 이와 반대된 형태는 카세트 테잎이 있다.  Dynamic의 의미는 충전이다. 충전시켜줘야 한다. 

    개념적으로 DRAM은 1 Byte의 저장공간이 줄지어진 형태다. 이 1 Byte의 저장공간은 주소값을 가지고 있다. 이때 주소를 Physical 주소라 한다. Physical 주소외에 Logical address 가 있다. 

    - Example

      CPU가 Main memory 4000번지 Physical address에 가서 4 Byte를 읽어오라 할 수 있다. 

    HDD, SSD에 저장된 Program이 Main memory에 Process 형태로 올라간다. Memory 속도가 훨씬 빠르기 때문이다. File도 같은 방식으로 올라간다. SRAM을 사용할 수 있다면 DRAM은 여전히 CPU보다 느리기 때문에 SRAM (=Cache)를 둔다. Cache에 자주 사용하는 Data는 옮겨두는 방법으로 속도를 높인다. 

  - CPU instruction-execution cycle

    결과적으로 Memory는 OS에서 중추적인 역할이다. CPU는 Memory의 값만 읽고 쓰는 것이 가능하다.  I/O Device도 Memory의 저장공간이 매핑되어 CPU가 I/O Device 를 제어할 수 있다. 

    - CPU - Cycle 

      Memory의 값을 CPU가 읽어오고 다시 Memory에 저장하는 과정을 CPU cycle이라 한다. 

  - Background Summary 

    Main memory 는 CPU에 바로 접근할 수 있는 유일한 저장 장치이다. CPU는 Disk, Network , Printer같은 장치에 바로 접근할 수 없기 때문에 Memory를 거쳐야 한다. 

    - Stall 

      Stall은 Memory가 CPU보다 느리기 때문에 읽고 쓰는 시간 동안 낭비된 CPU cycle을 말한다. 이를 보완하기 위해 Hyper thread를 사용 해서 Memory에 읽고 쓰는 시간에 다른 Thread에 일을 시킨다. 

- Sharing/Protecting the Memory 

  Kernel, System/ user Process, Device 들이 Main memory 에 올라갔다. 그렇다면 다음의 문제가 생긴다. 

  1. 어떻게 Main memory에 올라간 것들이 Data share할 수 있을까. 

  2. 어떻게 Protection 할 수 있을까. 

  -> Logical(= Virtual) address 를 사용한다.  모든 Process는 Logical address를 가지지만 실재로는 다른 Physical address를 가진다.

- Base and Limit Registers

    Memory Protection을 만족시키기 위해 Base register와 Limit register을 사용한다.  CPU가 Memory에 접근할 때 Base 보다 낮은 값에 접근하거나 Base + Limit보다 큰 값에 접근 하면 Trap을 발생시킨다.  

    - Protection 

      Protection은 다음 두가지를 만족해야 한다. 

      1. User 공간에서 Process가 다른 User 공간의 Process를 침범할 수 없다. 
      2. User 공간의 Process 는 Kernel 공간을 침범할 수 없다.

    - Base register & Limit register 

      Base register 는 Process의 시작 주소를 가리키고 Limit register 는 Process의 크기를 나타낸다.

- Logical vs Physical address space 

    - Logical address 

      Process에서만 사용되는 주소로 시작 주소는 0 이다.  

    - Physical address

      Memory 실재의 공간 address를 말한다.  

- Address Space 

    'int a = 3' 에서 변수 a 는 Logical address를 가리키고 있지만 실재로는 Physical memory에 값이 저장되어 있다. 따라서 Logical memory를 Physical memory로 변환시켜야 한다.  

- Memory-management unit (= MMU) 

  MMU는 Logical address를 Physical address로 변환시킨다. Memory를 변환 하는 건 빈번하게 일어난다. 그래서 Hardware로 하는것이 Software로 구현하는 것 보다  빠르다.  예전에는 외부에 MMU 장치가 따로 있었지만 요즘은 CPU 내부에 있다. 

  - Relocation Register

    Relocation 방식은 특정 값을 Logical address의 Base 값에 더해주는 방법이다. 그래서 Base register를 Relocation register라고도 한다. 

    - Memory Protection 

      Logical address 가 Limit address보다 작은지 확인하고 작다면 Logical address + Relocation register 해서 변환 시켜준다. 

  - Address Binding 

    Binding은 할당을 말한다 CPU는 Physical address만 읽을 수 있다. 따라서 Logical address를 반드시 Physical address로 변환시켜 주어야 한다. 

    Binding 시점을 기준으로 Logical address를 변환해주는 것은 Compile time, Load time, Execution time이 있다. 

    1. Compile time 

       작성한 Source code를 Binary code로 변환 할 때 Physical address로 변환하는 것이다. 

       하지만 프로그램을 실행할 때 Memory 어느 공간에 할당받을지 모르기 때문에  Compile time 시 Physical memory로 변환하는 건 불가능하다.

       Multiprogramming 을 사용하기전 Memory 하나를 통째로 사용하던 시절에 사용했다. 

    2. Load time 

       SSD에 있던 Program을 실행해서 Memory에 올라갈때 Logical address를 Physical address로 Relocation 하는 방식이다. 

       하지만 현재의 Program은 Memory에 쪼개져서 올라간다. 또한 Process가 더 많은 공간을 필요로 하게되면 해당 위치에서 크기를 키우지 않고 더 넓은 Memory 공간으로 복사이동을 하기 때문에 모든 주소를 다시 계산해야 된다는 문제가 있다. 

    3. Execution time (= Run time)

       가장 많이 사용하는 방식이다. 그 Process가 실행이 될 때  Code level 에서 주소 변환을 하는 것이다. 
    

