# **Deadlock #2**



-  Methods for Handling Deadlocks 

  현재의 OS는 Deadlock을 무시한다. Avoidance 에는 비용이 많이 들고 Avoidance 는 구현하기 까다롭다. 하지만 우리는 배워야 한다. 

  

## Deadlock  Avoidance 

Deadlock Aviodance 에서는 해당 Process가 몇개의 Resource type을 필요로 하는지 미리 알고 있어야 한다. 이 정보로 Circular wait 이 절대 발생하지 않도록 하는 것이다. 즉 Process에서 Request가 왔을 때 Deadlock에 빠질 것 같다면 자원을 할당하지 않는 것이다. 하지만 이 정보를 예측하는 것이 쉽지 않다. 

- System Model 

  - Resource type state가 존재한다. 

    Process가 최대 5개의 자원을 필요로 하고 Resource가 10개가 있다. 이때 10개 중 3개를 Process에게 줬다면 Available 상태는 7개, Allocated는 3개, Needed 는 2 이다. 이를 위해 OS는 모든 상황을 다 알고있다고 가정한다. 

    1. Available 

       남아있는 Resource 중 몇개 사용할 수 있냐. 

    2. Allocated

       이미 할당된게 몇개이냐. 

    3. Needed

       Process마다 필요한게 몇개이냐. 

  - Assume Worst Case 

    Process가 자원이 Maximum 5개가 필요로 하다고 했다면 Resource 가 5개가 할당 되어야 Process가 종료된다는, 최악의 경우로 가정한다. 또한  Non-preemptive임을 가정한다. 

- Safe Sequence 

  어떤 System이 Safe 하면 Safe Sequence 가 적어도 하나 존재한다. 

  - Safe 

    현재상황에서 절대 Deadlock이 발생하지 않으면서 모든 Process가 종료 될 수 있는 방법이 적어도 하나 있다면 Safe 이다. 

  - Safe Sequence 

    Safe를 실현할 때의 Process순서를 말한다.  Safe Sequence 가 < Pi, Pj,  .., Pk> 라면 Pi가 Terminate 되어야만 Pj 가 자원을 받을 수 있다. 

  - Safe state vs Unsafe state

    Safe state는 Deadlock 에 빠질 수 없고 Unsafe state는 Deadlock에 빠질 수도 있고 아닐 수도 있다. Avoidance는 Safe state를 유지하는 것을 말한다. 

- Deadlock Avoidance Methods

  - Resource-Allocation Graph Algorithm

    - Claim 

      Process --> Resource 에서 화살표에 해당하며 Request를 해도 되는지 확인하는 역할이다. 확인하는 내용은 Cycle이 형성 되는지 아닌지 이다. 가능하다면 Claim 이 Request 로 바뀐다. 

  - Banker's Algorithm

    Multiple Instances 인 경우 적용한다. 조건으로 Process 가 Maximum 사용가능한 개수를 알고 있어야 한다. 그리고  자원을 사용하기 위해서 Wait 상태에 있고 Process가 종료될 때 모든 Resource 들을 반납해야 한다. 

    - 두가지 알고리즘이 존재한다. 

        1. Safety Algorithm

           Safe sequence가 있는지 찾는다. 현재 상황이 Safe 한지 아닌지 찾는 것이다. 

        2. Resource-Request Algorithm

           어떤 Request가 왔을때 Simulation을 해서 Safety sequence가 있는지 확인하고 Request를 받는다. 

  - Data Structures for the Banker's Algorithm

      n : Process의 수 

      m :  Resource type의 수

      Available, Max, Allocation, Need 를 n x m Matrix로 나타낸다. 

      Need [i, j] = MAX [i, j] - Allocation [i, j]

- Example of Bankers Algorithm

    <p1, p3, p4, p2, p0> 처럼의 Safe sequence 를 찾아내면 된다. 

- Resource-Request Algorithm for Process Pi 

  어떤 Process에서 Request 했다면 그 상황에서 Safety sequence 가 있는지 찾아보면된다. 

## Deadlock Detection 

Deadlock 이 발생 했는지 안했는지 Detection 해야 한다. 

- Wait-for graph 

  Single Instance 라면 모든 Graph를 나태어 Cycle이 존재하는지 확인한다. 이때 Resource 를 다 지워보고 Circle 이 존재하면 Deadlock 이 발생 한 것이다. 

- Detection을 언제 얼마나 자주 해야할까. 

  자주 하면 어떤 Process 때문에 Deadlock 이 발생했는지 알 수 있다. 하지만 Overhead 가 크다. 

## Deadlock Recovery 

Deadlock 을 찾았다면 어떻게 Recovery 할 것인가. 

- Process Termination 

  Process를 강제 종료 한다. 모든 Process를 종료하거나 부분적으로 종료시킨다.  

- Resource preemption 

  Resource 를 강제로 뺏어서 종료한다. 

- Partial Temination 

  모든 Process를 종료하는 것이 아닌 비용이 가장 적은 Process를 찾아서 강제종료 시킨다. 

