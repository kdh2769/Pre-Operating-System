# **File#2**



## File Operations 

- File Operation Overview

  File 접근 방법

  1. 찾는 File이 있는 Directory로 이동한다. 
  2. Directory에 있는 해당 File 의 FCB를 읽는다. 
  3. 해당 FCB에 나와있는 Block 주소로 간다. 

- File System Implementation 

  Metadata 를 Memory Caching 을  In-Memory Structure가 수행한다. 

  - In-Memory Structure (File 접근 1번 Cache)

    Memory로 Metadata를 Caching 하는 것은 빠른 속도를 사용하기 위해서다. 과정은 먼저 Directory structure를 Caching 한다. 하지만 통째로 가져오기엔 용량이 너무 크기 때문에 개별 Directory 단위위로 Directory structure을 가져온다. 

- File Operations 

  - Create a file 
    1. User space에서 파일 만들어 달라고 System call 호출했다. 
    
    2. Disk에 새로운 FCB를 생성한다. 
    
    3. Cache한 Directory structure에 원하는 위치가 있으면 그곳에, 없다면 Disk에 있는 Directory structure을 가져와서 새로 생성한 FCB를 추가해준다. 
    
    4. 그리고 Disk 에 저장한다. 
    
       

- System-Wide Open-File Table

  DISK에 있는 FCB 를 Main memory kernel 공간에 Cache 한다. System-wide open-file-table에 있는 FCB 들은 전체 System에서 사용해도 된다. 

  Count로 몇개의 FCB가 올라왔는지 측정한다.

- Per-Process Open-File Table 

  하나의 File 을 여러 Process 가 Share 할 수 있도록 Kernel memory에 공간을 만든다. 

- File Operations 

  - Open()

    File을 열어라 라는 명령이 들어왔다. 
  
    - 찾는 File이 system-wide open-file table 에 있다. 
      1.  Open() System call 로 System-wide open-file-table 에 FCB를 찾으러 간다. 
      2.  System-wide open-file-table 에 FCB 있다면 Per-process open-file table에서 해당 FCB를 가리킨다. 그리고 Count 를 1 증가시킨다.  
    - 찾는 File이 system-wide open-file table 에 없다.
      1. Directory structure을 업데이트 한다. 
      2. System-wide open-file table에 FCB를 가져온다. 
      3. Per-process open-file table 와 System-wide open-file table의 FCB를 연결한다. 

  - Close ()

    System-wide open-file-table 에서 Count = 0 이되면 더이상 해당 FCB를 사용하는 Process가 없다. 이때  Per-process open-file table pointer를 지워준다. 

## File Access and Allocation Methods 

File을 어떻게 Access 하고 Allocation 할 것인가. 

- File Access Methods 

  File에 어떻게 접근할 것인가. 

  - Sequential Access

    File pointer로 Byte를 순차적으로 읽는 것이다. 대부분 이 방법을 사용한다. 

  - Direct Access(= Random Access)

    DISK의 원하는 Block 을 읽는 것이다. 주로 Database에서 쓰인다. 

- Allocation Method

  Files 을 어떻게 DISK에 할당 할 것인가(= 배치 할 것인가). 

  - Allocation goal
    1. DISK의 모든 용량을 활용 한다. 
    2. 빠르게 Search 할 수 있어야 한다. 

- Contiguous Allocation 

  Directory에 File의 저장 위치를 가리키기 위해 Start와 Length 값으로 DISK의 어느 Block 에 저장되어 있는지 나타낸다. 

  - 장점 

    1. Direct access 가 사용하기 적합하다. 

       Start와 Length 를 알고 있으므로 바로 Start block에 접근하면 된다. 

  - 단점 

    1. External Fragmentation 이 발생한다. 

       Compaction (디스크 조각 모음)을 통해 DISK의 Free space 를 Contiguous하게 만들어 주어야 한다. 

    2. File 생성시 적합한 Space 를 찾아야 한다. 

       용량과 가장 최적화된 Space 에 File을 할당할지 아니면 먼저 나오는 Space 에 File을 할당할지와 같은 알고리즘으로 적합한 DISK의 Free Space를 찾아야 한다. 

    3. Handling File Size Extension 

       File을 사용 하다보면 File의 용량이 늘어날 수 있다. 처음 할당할 때 늘어날 용량을 예측해서 잡아주기도 한다. 하지만 예측 까지 File의 용량이 늘어나지 않는다면 그건 자원 낭비로 이어진다. 

- Non-Contiguous Allocation 

  Non-Contiguous allocation 으로 Linked 방식과 Indexed 방식이 있다. 

  - Linked Allocation 

    Directory 에 File 의 Start block 과 End block 을 나타낸다. 

    - 장점 

      1. Sequential access 가 효율적이다.  File 접근을 Start block 에서만 할 수 있다. 
      2. No External Fragmentation 이다.

    - 단점 

      1. Direct access에 비효율적이다 .

         File의 5번째 Block에 접근하고 싶지만 5번째 Block을 바로 알 수 없다. 시작 Block으로 가서 2, 3, 4,번째 Block을 거쳐야 한다. 

      2. Block 마다 Pointer를 사용한다. 

         다음 Block 이 어떤 Block을 가리키고 있는지 나타낸다. 

      3. Internal fragmentation이 발생할 수 있다. 
      
      4. Reliability 
      
         File 의 Block 일부분이 날라가면 사용이 불가능하다.

  - File-Allocation Table (FAT)

    Linked - allocation 을 보완한 방식이다. 다음 Block을 가리키고 있는 Table(FAT)를 만든다. 

    - 장점 
      1. FAT를 Memory로 옮겨서 빠른 속도로 접근할 수 있다. 
      2. Direct access가 가능하다. 

  - Indexed Allocation - inode

    Directory 에 해당 File의 Index block을 가리켜 준다. Index block에서 각각의 Block의 위치를 가리켜준다. 

    - 장점 

      1. No External Fragmentation 

      2. Direct Access 를 지원한다. 

         Index block에 가면 주소가 다 있다.

    - 단점 

      1. Index Table 도 공간을 차지한다. 
      
      2. Index block Size를 어떻게 할지 결정해야 한다. 
      
         
      
    - Indexed Allocation - Mapping 

      Index block의 Size를 어떻게 결정하느냐 이다.	 

        1. Linked Scheme 

           Index table이 block size를 초과하면 해당 Block의 마지막 Page는 다음 Block 과 연결시킨다. 

        2. Two - Level Index 

            첫번째 Index table block 에선 다음 Index table block을 가리켜 준다. 

        3. Combined Scheme 

           File size에 따라 작다면 Direct block 로 연결한다. File size가 크다면 Indirect block으로 연결한다. 

           - Direct block 

             inode 에 File 정보를 그대로 담고 있는 Block pointer 값을 준다. 
             
           - Indirect block 
           
             inode에 Index block을 가르켜준다. 

- Access Performance For Allocation Methods 

  - 성능 평가 기준 

    1. Data - Block 접근 시간 
    2. 얼마나 저장을 많이 할 수 있는가. 

  - Contiguous Allocation 

    - 장점 
      1. Disk block에 한번만 접근하면 된다. 첫번째 Block과 마지막 Block 주소를 얻으면 중간 Block 위치는 예측할 수 있다. 
      2. Sequential, direct access 모두 좋다. 

    - 단점 

      External Fragmentation 발생으로 저장효율이 좋지 않다. 또한 늘어날 저장공간을 사용하지 않는다면 이 역시 효율성을 떨어뜨린다. 

  - Linked Allocation 

    - 장점 

      No External fragmentation 이다. 

    - 단점

      Direct access는 효율이 좋지 않다. 중간 Block의 위치를 찾는데 시간이 오래 걸린다. 

  - Indexed allocation 

    - 장점 
      1. Index 가 Memory에 있다면 Sequential, direct access 모두 성능이 좋다. 
