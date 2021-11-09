# Memory-Management Strategies #1

Keywords : Logical Memory, Paging, MMU

## Background

- Memory Technology 

  메모리는 위로 갈 수록 빠르고 비싸다. 밑으로 갈 수록 느리고 크다. 그리고 휘발성으로 나눌 수 있다. (SRAM - DRAM - Flash memory - Magnetic disk ). SRAM 만큼 빠르고 비휘발성이고 가격이 싼 Memory가 있으면 좋겠지만 존재하지 않는다. 

- DRAM 

  현재 Main memory 는 DRAM 이다. 

  - Dynamic random access memory 

    무작위로 위치에 접근할 수 있다. Dynamic은 충전시켜줘야 한다. 개념적으로 DRAM은 1 byte의 저장공간이 쌓여있다. 이 저장공간은 DRAM의 물리적 위치에 해당한 주소를 가지고 있다. 

  - CPU instruction-execution cycle

    CPU는 Memory로 명령을 받고 동작을 수행하고 결과 값을 Memory에 보낸다. 

  - Background Summary 

    Main memory 는 CPU에 바로 접근할 수 있는 유일한 저장 장치이다. 

    - Stall 

      Stall은 CPU가 Memory에 읽고 쓰는 시간동안 멈춰있는 것을 말한다. 이를 보완하기 위해 Hyper thread를 사용한다 . 

  - Sharing/Protecting the Memory 

    1. 어떻게 Main memory에 Process 여러개를 올릴까.

    2. 어떻게 Process 를 Protection 할 수 있을까. 

       -> Logical(= Virtual) address 를 사용한다.  모든 Process는 동일한 Logical address를 가지지만 실재로는 다른 Physical address를 가진다.

  - Base and Limit Registers 

    Memory address에 접근할 때 Base보다 작거나 Base + limit 보다 큰 address에 접근하려 한다면 Trap을 발생시킨다. 

    - Protection 

      1. User 공간에서 Process는 Protect 되어야한다. 
      2. User 공간의 Process 는 Kernel 공간으로 갈 수 없다. 

    - Base register 

      Process의 시작 주소를 가리킨다. 

    - Limit register 

      Process의 크기를 나타낸다. 

  - Logical vs Physical address space 

    - Logical address

      Physical address에 상관없이 Process내부의 주소다 

    - Physical address

      실재 Memory의 주소다. 

    - Address Space 

      int a = 3; 이라면 변수 a 는 Logical address를 가리키고 있고 실재로는 Physical memory에 값이 저장되어 있다. 

      - MMU 

        Memory-management unit으로 Logical address를 Physical address로 변환시켜준다. 

  - MMU

    Logical address를 Physical address로 변환 하는건 빈번하게 일어난다. 그래서 Hardware로 하는것이 Software로 구현하는 것 보다  빠르다. 

    - Relocation register

      Logical address에서 Relocation register를 더해서 Physical address로 변환한다. 

  - Address Binding 

    CPU는 Logical address를 모른다. 언젠가는 Logical address가 Physical address로 변환되어야 한다. 이때의 시점으로 Compile할때, Load 할때, Execution 할 때 될 수 있다. 

    - Compile time 

      작성한 Source code를 Binary code로 변환 할 때 Physical address로 변환하는 것이다. 하지만 프로그램을 실행할 때 Memory에 어디에 생성될 지 모르기 때문에 이는 불가능하다. Compile time 은 Multiprogramming 이 시작하기 전에 사용할 수 있던 방법이다. 

    - Load time 

      프로그램을 실행할 때 Memory에서 어디에 올라갈지 알게되면 주소를 변환하는 것이다. 가능은 Memory를 더 할당 받아야 하는 상황이 생기면 Process 전체를 Memory의 다른 공간으로 옮기기 때문에 Physical address를 다시 할당받아야 하는 문제가 있다. 

    - Execution time 

      그 code가 실행될때, runtime될 때 address를 변환한다 .   

