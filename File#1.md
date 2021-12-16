# **File System#1**

File은 무엇이고 File system 이란 무엇인가. File system이 어떻게 동작하는가. File allocation 방법. File system 의 종류.



## File and File System Concept

- Physical Storage Unit: Hard Disks 

  File 이 존재하는 공간은 HDD 혹은 SSD이다. HDD는 원형 Platter 위에 Track 으로 정보를 새길 수 있는 곳이 있다. 이 Track 을 잘게 쪼갠 것이 Sector 이다. Disk 는 Read/write를 이 Sector 에 한다. 그리고 이러한 Sector를 Block이라 한다. File은 이러한 Block에 저장된다. 

- File Concept

  - What is a File

    우린 컴퓨터에서 File을 주고 받는다. 운영체제에선 File을 사용하지 않고 HDD/SSD에 정보를 기록 할 수 없다.  

  - Logical View 

    사용자 관점에서 File은 정보가 구분지어지는 단위 이다. {file 이름 , offset} 형태로 어떤 file에서 어느 정도의 offset으로 떨어진지 나타낸다. 

  - File System 

    File System 은 사용자 관점의 File을 DISK와 같은 실재 Device에 새겨주는 것 이다. Disk 는 Block 단위로 구성되기 때문에 사용자 관점에서 File 정보인 {file, offset} 형태가 File system에 들어오면 변환 시켜준다. 
    
    File System 의 역할은 File을 관리한다는 면에서 아주 큰 역할을 한다. 그 중 Disk의 Block에 주소를 Translate 해주는 역할도 한다. 

- Internal File Structure

  - File : Logical View 

    사용자 관점에서 File 은 그저 Bytes stream 이다.  

  - Block 

    File system 에서는 사용자 관점의 File을 DISK 의 Block 단위로 관리한다. 

- File Operations 

  File 의 여러가지 동작 중 일부다. 

  - Create 

    File을 만드는 것이다. 만들 File을 DISK에 새기고 File System에 등록하는 것이다. 
    
  - Read/Write

    File Pointer를 이용해서 File을 Read 하고 Write 한다. 

- File Information 

  - File pointer 

    Program counter는 Process의 Code에서 어디를 읽고 있는지 그 위치를 가리킨다. File pointer는 File의 어디를 읽고 쓰겠다는 것인지 그 위치를 가리킨다. 기본적으로 File pointer는 읽으면 자동으로 다음으로 간다. 

  - File-open count 

    File은 공유하는 경우가 많다. 현재 몇명이 해당 File을 사용하고 있는지 샌다. 

  - Disk location of file 

    File이 Disk의 어디에 저장되어 있는지 그 Block number를 가지고 있다. 

  이 외에도 다양한 정보를 가지고 있다. 

- File Types 

  - Files may have types 

    Unix system에선 device, directory, 바로가기 같은 것들도 파일로 구분한다. 

    File의 형태로는 exe, dll, source code, text 등이 있다. 

    실행파일 형태로는 jpg, mpg, mp3 등이 있다. 

  - Windows encodes type in name 

    윈도우는 파일의 확장자로 해당 파일이 어떤 파일인지 결정한다. 

  - Unix encodes type in contents

    Unix는 파일의 가장 앞단의 정보로 해당 파일이 어떤 파일인지 결정한다.

- File System

  File system은 굉장히 많은 일을 한다. 주요한 일은 물류창고를 떠올리면 된다. File은 물류다. 크기가 한정된 DISK에 어떤식으로 File을 저장하고 관리할지가 File system 이다. 즉 File system엔 관리 방법이 여러가지일 수 있다. 

  - Data structure on disk 

    File system은 DISK를 어떤식으로 나눠서 사용할 것인가로 볼 수 있다. 또한 DISK에는 여러 종류의 File system이 사용 가능하다.  DISK에 여러가지 File system이 올라갔을 때 관리해주는 단위를 Unix에서는 Volume 이라 하고 Windows 에서는 Partition이라 한다. 

    Unix 에서는 Root Directory 를 Root로 트리 구조 이다. 각기 Sub tree에서 다른 File system 을 쓸 수 있다. 

    Windows에서는 같은 DISK 에서는 Drive 단위로 나누도 이 Drive에 여러 File system을 사용할 수 있다.

- File System Implementation 

  Unix, Window 모두 Storage를 나눠서 사용하는 것은 비슷하다. 

  - On-disk structures

    DISK 를 나눠서 File을 관리한다. 

  - On-disk File-system 

    DISK는 다음 4가지로 나눈다. 

    1. Boot Control 

       Booting을 할 수 있는 정보가 Storage 가장 상단에 저장되어있다. 전원이 들어오면 냅다 상단의 정보를 읽는다. 

    2. Superblock (= volume control block)

       Data와 File과 관계없이 DISK 자체를 관리하기 위한 정보를 가지고 있다. Block 의 개수, Block의 Size, Free space pointers 를 관리한다.

       SSD 용량은 제조사가 알린 용량보다 적게 사용될 수 있다. 예로 32GB 짜리가 30GB인 것이다. 이는 1KB 를 1024 가 아닌 1000으로 읽어서다. 결국 32 * 1024 * 1024 * 1024 가 아닌 32 * 1000 * 1000 * 1000 인셈이다.  또한 Super block 과 Boot control 이 차지하는 공간이 있기 때문에 저장할 수 없는 공간이 있다. 

    3. Metadata 

       Metadata 는 File을 관리하기 위한 Data 이다. Process의 PCB 역할과 비슷하다. 

       - Directory structure

         File structure는 File이 아니기 때문에 별도의 공간이 필요하다. File의 위치에 관한 구조적인 정보를 가지고 있다. 

       - File Control Block (FCB)
  
         File에 관한 정보가 담겨있다. File의 접근 권한 , File 정보, File size, File 의 소유주 및 그룹, File 이 Data block 어디에 저장되어 있는지. 
    
    4. Data blocks
    
       File은 DISK에서 Data blocks에 저장되어 있다. 이때 Block 이 쪼개져 있을 수도 Contiguous 할 수 있다.  
  
- Metadata block 

  - Directory

    File 이름을 가질 수 있고 보통 Tree 구조이다. Folder 안에 File이 있고 Folder가 있는걸 떠올리자. 찾고자 하는 File이 있다면 Directory를 따라가서  해당 File이 위치한 Directory로 가야한다. 

  - File control  block (FCB)

    Directory 엔 실재 File이 있지 않고 FCB가 있다. 이는 용량이 너무 커지기 때문이다. 

- Layered File System

  Layered system은 주어진 역할만 수행하는 모듈로 구성되어 있다. 

  Application program 이 실행 된다 -> Logical file system -> file -organization module -> basic file system -> I/O control -> devices

  - Logical file system 

    Metadata (Directory & FCB) 를 관리한다. 

  - File organization module

    Logical block 을 Physical block으로 변환한다. 그리고 Free space 관리하고 File을 DISK에 어떤 방식으로 할당할 것인지 관리한다. 

  - Basic file system 

    Logical 한 Block 이 Physical 하게 변환 되었을 때 Device에 맞는 주소로 변환시켜준다. 그리고 Buffer caches 를 관리한다. 

    - Buffer cache 
  
      최근 사용한 DISK  block을 Memory에 Cache 한 것을 말한다. 
  
  - I/O control (Device drivers)
  
    Device 와 Memory가 Mapping이 되어서 전기적 신호로 Data를 읽고 쓰는 것을 Device driver가 수행한다. 