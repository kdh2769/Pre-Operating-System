# **Deadlock #2**



-  Methods for Handling Deadlocks 

  현재의 OS는 Deadlock을 무시한다. Prevention에는 비용이 많이 들고 Avoidance 는 구현하기 까다롭다. 하지만 우리는 배워야 한다. 

  

## Deadlock Avoidance 

Process에서 Request가 왔을 때 Deadlock에 빠질 것 같다면 자원을 할당하지 않는다. 이때 Process는 최대 몇개의 자원을 필요로 하는지 예측한다. 하지만 이 예측이 쉽지 않다. 

- System Model 

  - Resource type state가 존재한다. 

    Process가 최대 5개의 자원을 필요로 하고 자원이 10개가 있다. 이때 10개 중 3개를 Process에게 줬다면 Available 상태는 5개, Allocated는 3개, Needed 는 2 이다. 이를 위해 OS는 모든 상황을 다 알고있다고 가정한다. 

    1. Available 

       Resource가 몇개 더 필요하나. 

    2. Allocated

       이미 할당된게 몇개이냐. 

    3. Needed

       Process마다 필요한개 몇개이냐. 

  - Assume worst case 

    Process가 자원이 Maximum 5개가 필요로 하다고 했다면 최악의 경우를 가정해 5개의 자원이 들어가야 종료가 된다고 가정한다. 여기서 Non-preemptive임을 가정한다. 

- Safe Sequence 

  어떤 System이 Safe 하면 Safe Sequence 가 적어도 하나 존재한다. 

  - Safe 

    현재상황에서 절대 Deadlock이 발생하지 않으면서 모든 Process가 종료 될 수 있는 방법이 적어도 하나 있는 것이다. 

  - Safe Sequence 

    Safe를 실현할 때의 Process순서를 말한다.  Safe Sequence 가 < Pi, Pj,  .., Pk> 라면 Pi가 Terminate 되어야만 Pj 가 자원을 받을 수 있다. 

  - Safe state vs Unsafe state

    Safe state는 Deadlock 에 빠질 수 없고 Unsafe state는 Deadlock에 빠질 수도 있고 아닐 수도 있다. Avoidance는 Safe state를 유지하는 것을 말한다. 

- Resource-Allocation Graph Algorithm

  - Claim 

    Process --> Resource 에서 화살표에 해당하며 Request를 해도 되는지 확인하는 역할이다. Cycle이 형성되는지 확인한다. 

- Banker's Algorithm

  Multiple Instances이면서 

  - 두가지 알고리즘이 존재한다. 

      1. Safety Algorithm

         Safe sequence가 있는지 찾는다. 

      2. Resource-Request Algorithm

         어떤 Request가 왔을때 Simulation을 해서 Safety sequence가 있는지 확인하고 Request를 받는다. 
      
  - Data Structures for the Banker's Algorithm
  
      n : Process의 수 
  
      m :  Resource type의 수
  
- Example of Bankers Algorithm 

- Resource-Request Algorithm for Process Pi 

## Deadlock Detection 

- Wait-for graph 

  Single Instance 라면 모든 Graph를 나태어 Cycle이 존재하는지 확인한다. 

- When, and 

## Deadlock Recovery 

- Process Termination 

  모든 Process를 강제종료한다. 

- Resource preemption 

  Resource 를 강제로 뺏어서 종료한다. 

- Partial Temination 

  모든 Process를 종료하는 것이 아닌 비용이 가장 적은 Process를 찾아서 강제종료 시킨다. 

