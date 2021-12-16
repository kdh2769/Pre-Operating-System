# **File#3**

## Free-Space Management 

- Free-Space List 

  현재 사용가능한 Disk 번호를 가지고 있다. 이를 Bit vector와 Linked list로 구현할 수 있다. 

- Bit Vector 

  Block을 리스트로 쫙 펼친다. 000111010111 이렇다면 0 은 Free한 Block, 1 은 Allocated 된 Block 이다. 

  - 단점 

    Bit map을 위한 별도의 공간이 필요하다. 

  - 장점 

    Contiguous 한 Free block을 찾기 쉽다. 

- Linked List

  Linked list로 Free-space list를 걸어준다. 

  - 단점 

    1. Free-space를 계속 순회 해주어야 한다. 
    2. Contiguous한 Free-space를 찾기 어렵다. 

  - 장점 

    공간 낭비가 없다. 이는 Free space linked list는 어짜피 사용 안하는 Block 을 엮은 것이기 때문에 공간 낭비가 없다. 

## Protection 

Permission 을 통해 접근 권한을 준다.  주로 Multi-user system 에서 사용한다. 

- Controlled Access 

  무엇을 할 수 있는 권한인지, 누구에게 권한을 줄지 결정해야 한다.

- Access Control 

  User 를 구분지어 준다.  

  - Universe 

    접근한 모든 사람들이다. 

  - Group 

    Universe 속 사람들을 그룹화 한 것이다.  
    
  - Owner

    File 을 생성한 사람이다.

  - Mode of Access (rwx)

    Read 권한은 r,  Write 권한은 w, Execute 권한은 x 로 부여한다. 

- Unix/Linux File Protection 

  Unix/Linux 에선 File 을 rwx 로 권한을 설정한다. 이때 권한은 숫자로 준다.

  - Example 1

    Unix/Linux 에서는 Directory listing을 Owner, Group, Public 순으로 나타낸다. 

    -rw-rw-r--이라면 해당 File은 아무도 Execute 권한은 없는 것이다. 
  
    drrwx------ 이라면 해당 Directory는 Owner만 모든 권한을 가진 것이다.
  
  - Example 2
  
    2진수를 10진수로 변환해서 Owner, Group, Public 의 권한을 설정한다.  
  
    761 이라면 111 | 110 | 001 의 값을 가진 것이다. Owner 는 모든 권한, Group은 Read, Write 권한만, Public 은 Execute 권한만 가진다. 

## Efficiency and Performance 

- Buffer Cache and Page Cache 

  File I/O 를 빠르게 해야한다. 

  - Buffer Cache 

    최근에 사용한 DISK Block을 Main memory에 Cache 한다. 

  - Page Cache 

    Memory mapped file and I/O에서 사용한다. 최근에 사용한 Page를 Cache 한는 것인데 Block cache 상단에서 일어난다. 

- I/O Without a Unified Buffer Cache 

  - Problem : Double caching 

    Memory - mapped I/O 시킨 것은 Buffer cache 에서 Page cache한 것을 사요하고 일반 I/O는 그냥 Buffer cache를 사용한다. 

  - Unified Buffer Cache 

    Buffer cache와 Page cache를 통합시켰다. 

- Synchronous writes 

  DISK 에 Buffer나 Cached의 내용들이 저장되기 전까진 다른 동작은 멈추는 것이다. 

- Asynchronous writes 

  대부분 사용하는 방식이다. Cache 내용들이 저장되고 있을 때 다른 일을 할 수 있는 것이다. 

- Optimizing Page Cache for Sequential Access

  - Page Replacement Algorithms in Sequential access
  
    DISK 입출력 시간을 줄이기 위해 LRU를 사용하지 않고 Free-behind 나 Read-ahead를 사용한다. 
  
    - Free-behind
  
      읽은 Page는 삭제시켜 주면서 File을 읽는다. 
  
    - Read-ahead
  
      읽을 여러개의 Page 를 불러온다.
  
- Virtual File Systems 

  OS의 역할은 Virtualization 이다. 실재 System은 여러개로 구성되어 있지만 사용자는 그것을 모른채로 하나의 명령어만 사용하면 OS가 다 맞춰서 돌려주는 것이다. 

  VFS 는 Interface에 여러 종류의 System call 이 들어오면 각기 다른 File system에 맞게 변환 시켜준다. 

- Reliability 

  File system은 까다롭다. File 입출력은 사실상 Disk 와 Memory 2군데를 다 사용하는 것이다. 즉 Sync 가 맞아야 한다. 

  - 배터리 교체 유무 

    교체 유무에 따라서 File system은 큰 차이가 있다. 

    교체가 가능하다면 File system의 Reliability 가 훨신 커야 한다. 

  - Log Structured File Systems 

    File마다 Log를 남겨준다.

- Sun Network File System(NFS)
