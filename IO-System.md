# **IO Systems** 



- How Does Processor Communicate With I/O

  - ATmega128

    8 bit CPU 로 아두이노에 사용된다.  CPU엔 용도에 맞는 Pin 들이 있다. 이 Pin 에 다른 Device를 연결해서 사용한다. 하지만 대부분 이 방식은 사용하지 않는다. 

    현재는 CPU와 DRAM 사이를 선으로 연결한다. 그리고 그 선에 Device를 연결해서 사용한다.

- I/O Hardware

  Input/Output Device는 매우 많다. CPU와 Memory를 제외한 거의 대부분의 장비이다. 

  - Basic I/O Hardware

    1. I/O Device 

       I/O Device 그 자체를 말한다.  

    2. Port

       I/O Device의 Port 는 장비에서 선을 꼽을 수 있도록 한 것이다. 보통 Device를 제어하는건 Device controller 이고 Register로 되어 있다. 

    3. Bus

       Device와 PC 사이에 Data가 흐르는 선 자체를 말한다.  

- Device Controller

  - Register

    Control, Status, Read, Write 로 구성되어 있다. 

  Processor는 Device controller를 통해서 Device와 정보를 주고 받는다. 

- Techniques for Performing I/O

  - Program I/O

    - Direct I/O

      Device - CPU - Memory 로 연동이 될때 Direct I/O는 CPU를 거쳐서 Memory에 값을 받는다. 

    - Memory Mapped I/O

      CPU가 Device 가 있는지 없는지 모르고 Memory에 Mapping 된 Device에 바로 값을 주는 것이다. 

  - Direct Memory Access (DMA)

    Device와 Memory가 바로 Data를 주고 받는 것이다.

- Direct I/O 와 Memory mapped I/O 차이점 

  Direct I/O 방식에선 Device가 Memory와 별도의 주소체계를 가진다. CPU가 Device 와 Memory 접근을 둘다 한다. 

  - Special I/O Instructions 

    Direct I/O에선 CPU 가 별도로 Device와 연결하기 위해 Special I/O instructions를 사용한다. 이것때문에 속도가 느려질 수 있다. 

  - Memory - mapped I/O

    속도가 빠르다. CPU가 Device를 Memory처럼 사용한다. 

  - I/O Port Registers

    1. Status Register (Device 측면)

       busy-bit 으로 Device가 현재 어떤 상태인지 나타낸다. 1 이면 busy, 0 이면 idle 이다. 

    2. Control Register (CPU 측면)

       command-ready-bit으로 CPU 측면에서 준비됨을 나타낸다. 1 이면 ready, 0 이면 not ready 이다.
       
    3. Data in register 

       Device 밖에서 Data가 오면 저장해둔다. 

    4. Data out register 

       CPU 밖으로 보낼 Data 이다. 

- Polling

  - 과정

    Busy bit = 1  이면  해당 Device는 사용 못한다. Busy bit = 0  이어야 사용 가능하다. 

    Busy bit = 0 이면 CPU에서 Data-out register 에 값을 쓴다.  Data를 다 섰다면 Command-ready-bit = 1로 바꾼다. 그리고 Busy bit = 1로 만들고 Data out register에 있는 정보를 Device로 다 보낸다. 이후에 Busy bit = 0과 Command-ready bit = 0으로 바꾼다. 

  - 특징

    구현하기 간단하다. 하지만 Device를 사용할 수 없는 시간이 길어지면 CPU낭비다. 

- Interrupt - Driven I/O Cycle

  - Interrupt - Request Line 

    CPU에는 Interrupt-request line이 있다. Interrupt-request line에 전류를 흘린다. 이 신호가 잡히면 Interrupt를 발생한다는 것이다.  그리고 Interrupt vector의 번호를 통해서 Interrupt handlers를 실행한다. 

  - Interrupt Handling Example - Page Fault 

    1. Page fault interrupt 발생 
    2. 현재 Running  Process의 내용을 저장하고 Wait 상태로 바꾼다. 
    3. Disk 내용을 패치하고 
    4. Restart 한다. 

  - Interrupt Handling Example - System Call

    

- Direct Memory Access (DMA)

  CPU 없이 Device Controller가 Memory 에게 명령을 내린다. 

  - 이게 가능한데 왜 CPU가 중재를 하나 

    1. 어떻게 CPU 없이 이게 가능한가 

       사실은 불가능하다. DMA는 CPU에서 Controller와 하는 일을 밖으로 따로 때어 낸 것이다. 

  

- Device Drivers

  Device가 다 다르지만 OS가 이를 처리해준다. 

  

  데이터를 전송하는 방식에 따라 다시 나뉜다. 

  - Character devices

    Data가 많지 않거나 간헐적으로 사용할 때 사용한다. 

  - Block devices

    블럭단위로 Data를 읽고 쓴다. 

    Memory - mapped file , DMA에서 사용한다. 

- Buffering 

  네트웍과 Device의 처리 속도가 다르면 Buffer가 필요하다. 근데 Buffer에서 정보가 쌓이면 한번에 보내기 때문에 띄문띄문 정보가 오는 것이다. 

