# **Secondary-Storage Architecture**

DRAM을 제외한 System 바깥에 있는 저장 장치다. 

- Secondary-Storage

  1. 저장공간이 100GB 이상이다. 
  2. 비용이 저렴하다. 
  3. 전원이 공급되지 않아도 저장된게 사라지지 않는다. 
  4. 느리다. 

- Disks

  HDD 구조는 Data를 기록할 수 있는 단위가 동심원으로 되어있다 . 

  - Platter 

    동심원 판이다. 이 판 위엔 여러개의 동심원 이 있고 이를 Track 이라 한다. 이 Track 에 정보를 새긴다. 

  - Cylinder 

    여러개의 Track의 집합이다. 

  - Arm 

    기계적 장치로 Arm에 끝에 부착된 Head 가 Track 을 따라 정보를 읽는다. 

- Disk Performance 

  - Seek 

    Arm 이 움직이는 거리를 말한다.  바깥쪽 Cylinder에서 안쪽 Cylinder로 Data를 읽기 위해선 Arm 이 물리적으로 움직여야 한다. 

  - Rotation 

    내가 원하는 Sector가 Head에 읽히기 위해선 돌아야 한다. 돌아오는데 시간이 걸린다. 

  - Transfer

    Head 로 읽은 Data를 전송하는 시간이다. 

- Disk Scheduling 

  읽어야 하는 Sector는 접근을 빨리해서 읽어야 한다. 같은 시간동안 더 많은 Data를 읽어야 한다. 

  - Minimize seek time

    시간을 줄이기 위해서 Arm이 움직이는 시간을 최소화 해야한다. 대부분 Disk scheduling 하는 방식이다. 

  - Disk Bandwidth 

    대역폭을 늘려서 더 많은 Data가 이동할 수 있게 하는 것이다.  

    - 계산

      Last Transfer - First request / 수행 시간

  - Head Pointer

    현재 Head가 어느 Cylinder 위에 있는지다. 

  위의 방법들은 Hardware의 소재를 바꾸거나 해야 해서 Disk shceduling algorithm은 ARM이 움직이는 방법을 정한다. 

- FCFS 

  Queue 에 들어온 순서대로 읽는 것이다. 

  - 장점 

    구현하기 쉽다. 

  - 단점 

    Seek 값이 크다. 

- Shortest Seek Time First(SSTF)

  현재 위치에서 가장 적게 움직일 수 있는 곳으로 움직인다. 

  - 장점 

  - 단점 

    Starvation이 발생할 수 있다. 

- SCAN

  위아래로 그냥 왔다 갔다 하는 방식이다. 

  - C-SCAN (Circular)

    무조건 한 방향으로만 움직이는 것이다. 

- LOOK

  읽어야할 Data 의 Min, Max 사이만 왔다갔다 움직이는 것이다. 

  - C-LOOK

    한 방향으로 LOOK를 수행한다. 

- RAID : Redundant Array of Independent Disks

  원래의 RAID의 의미는 Inexpensive 로 값싼 Disk의 중복된 배열이었다. 지금은 독립적인 Disk의 묶음을 말한다. 

  즉 여러개의 Disk 를 묶은 것이다. 

  - Improvement of Reliability via Redundancy  

    Mirroring 을 통해 Reliability 를 높였다. 

    - Mirroring 

      같은 Data를 여러 Disk 에 저장하는 것이다. 

  - Improvement of Performance via Parallelism

    - Mirroring 

      Data를 Write 할땐 시간이 동일하지만 Read 할땐 시간을 단축시킬 수 있다. 한 Disk 에서 Data를 읽을 때 다른 Disk 에서 다음 Data를 읽으면 된다. 

    - Data striping 

      RAID 는 하나지만 여러개의 Disk가 연결되어 있다. 이 Disk 마다 Controller가 붙어있다. Striping이란 저장해야 될 Data를 나눠서 다른 Disk 에 저장하는 것이다. 
      
      이 Data를 읽을 때도 동시에 Disk에서 읽을 수 있기 때문에 성능이 좋다. 

- Disk Controller vs RAID 

  여러개의 Disk 가 있다면 Disk Controller는 Disk 마다 관리를 해주는 것이다. RAID Controller는 여러개의 Disk 에 하나의 Controlle가 관리해주는 것이다. 

- RAID 0 

  Striping 만 한 RAID 이다. 

  - 장점 

    속도가 매우 빠르다. 

  - Problem

    어떤 Disk 가 고장이 난다면 전체 파일일 읽을 수 없다. Disk persistent가 깨진다. 

- RAID 1

  Mirroring 만 하는 것이다. 

  - 장점 
    1. 안정성이 높다. 
    2. Data를 저장할때 2군데에 저장하는 시간은 동일하겠지만 읽을때 빠르게 읽을 수 있다. 

  - Problem 

    저장공간을 반밖에 사용하지 못한다. 

- RAID 4

  Parity라는 값을 만들어 특정 Disk 전체에 저장한다. 

  Parity 값은 모든 Disk 의 값을 연산해서 나온다. 

- RAID 5

  Parity 값을 저장할 Disk 를 Disk 마다 돌아가면서 저장시킨다. 그러면 한 Disk 가 고장이나도 Parity 값으로 복구가 가능하다.

- RAID 6

  Parity 를 2개 배치하는 것이다.  Disk 가 2개 고장나도 복구가 가능하다. 

- RAID 0+1 ( = RAID 01)

  RAID 0을 구성하고 RAID 1으로 구성한다.  Striping 하고 Mirroring 한다. 

- RAID 1+0 ( = RAIN 10)

  Mirroring 하고 Striping 한다. 

  - RAID 10 이 더 Reliability 하다 

    RAID 0은 Disk 하나에서 Error 가 발생하면 멈춘다. RAID 1은 오류가 생기면 복사를 해서 복구한다. 

    1. Striping 관점 
    
       Error 가 발생하면 RAID 01 은 RAID 0 에서 읽을 수 없다. 
    
    2. Error 복구 관점 
    
       RAID 01 에선 RAID 0 을 Mirrioing 했기 때문에 RAID 10 대비 복구할 때 더 많은 DISK를 복사해야 한다. 

- Selecting a RAID Level

  적당하게 용도에 맞게 만들어라 

- Direct Attached Storage (DAS) & Network-Attached Storage (NAS) 

  NAS 는 Client 가 Network를 통해서 Storage 에 접근 하는 것이고  DAS는 Server 를 통해서 Server 뒤에 있는 Storage에 접근 하는 것이다. 

