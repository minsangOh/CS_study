## [CPU 스케쥴링 #1](https://core.ewha.ac.kr/publicview/C0101020140328151311578473?vmode=f)

## **CPU and I/O Bursts in Program Execution**

- 프로그램 실행 시 CPU 및 I/O Bursts의 흐름을 아래와 같다.

![alt text](image-14.png)

- load store, add store 이런거는 system의 instraction을 실행하는 기계어
- CPU burst는 CPU만 연속적으로 사용하는 단계
- I/O burst는 I/O가 실행되는 단계

## **CPU-burst Time의 분포**

![alt text](image-15.png)

- 위 그래프는 컴퓨터에서 돌아가는 프로그램들의 CPU burst 타임을 그래프로 plotting
- 위 그래프를 해석하자면 CPU burst가 짧은(중간에 I/O burst가 많은 경우) 경우가 많다.
- I/O 작업 없이 CPU burst만 계속 있는 경우는 매우 적다.
- CPU를 짧게 쓰고 I/O가 중간에 끼어드는 작업을 I/O bound job이라고 한다.
- CPU를 굉장히 오래 쓰는 작업을 CPU bound job이라고 한다.

<aside>
💡 —> 그래서 여러 종류의 job(~process)가 섞여있기 때문에 CPU 스케줄링이 필요하다.

</aside>

<br/>

<aside>
💡 그럼 위 그래프를 통해 무엇을 추론 할 수 있을까?

</aside>

- 대부분의 CPU 사용 시간은 I/O bound job이 다 쓴다.
    - 사실 그건 아니다.
    - I/O Bound job은 그저 중간 중간 자주 등장 하다 보니 CPU 사용 시간을 짧게 난도질 해서 빈도수가 높은 것임.
        - 그저 짧게 쓰는데 빈도수가 잦은 것 뿐이다.
    - CPU bound job은 CPU를 오래 써서 빈도수가 적은 것임.
        - CPU는 CPU bound job이 많이 쓴다.
    
- Interactive job에게 적절한 response를 제공 해야 한다.
    - 왜?? 사람이 답답해하니깐.
- CPU와 I/O장치 등 시스템 자원을 골고루 효율적으로 사용해야 한다.

## **프로세스의 특성 분류**

- 프로세스는 그 특성에 따라 2가지로 나눌 수 있다.
    1. **`I/O-bound process`**
        - CPU를 잡고 계산하는 시간보다 I/O에 많은 시간이 필요한 job
        - many short CPU bursts(빈번하지만, 짧다)
    2. **`CPU-bound process`**
        - 계산 위주의 job
        - few very long CPU bursts(적지만, 매우 길다)

## **CPU Scheduler & Dispatcher**

- 위와 같은 이유로 스케줄링이 필요하다.
    
    ### 1. CPU Scheduler
    
    - Ready 상태의 프로세스 중에서 어떤 프로세스에게 CPU를 할당해줄지 고르는 역할을 수행
    
    ### 2. Dispatcher
    
    - CPU 제어권을 CPU Scheduler에 의해 선택된 프로세스에 CPU를 넘겨주는 역할을 수행한다
    - 이 과정을 문맥교환(Context Swiching) 이라고 한다.
    - 그렇다면 **CPU Scheduler, Dispatcher**는 하드웨어일까 하나의 독립적인 소프트웨어 프로그램일까?
        - 정답은 하드웨어, 소프트웨어 프로그램처럼 독립적인 뭔가가 아닌, 운영체제 안에 CPU Scheduling, Dispatching 하는 코드가 있는데 이걸 그냥 **CPU Scheduler, Dispatcher** 라고 부른다.
    

<aside>
💡 그렇다면 이러한 스케줄링은 언제 필요한 걸까?

</aside>

- 프로세스에 상태변화가 있는 경우에 스케줄링이 필요하다.
ex)
    1. Running —> Blocked (I/O를 요청하는 System Call)
    2. Running —> Ready (할당시간 만료로 timer interrupt)
    3. Blocked —> Ready (I/O 완료 후 interrupt)
    4. Terminated

- 1, 4의 경우에는 스케줄러가 강제로 뺏지않고 자진반납하는 경우이며,
**`nonpreemptive`**

2, 3의 경우에는 스케줄러가 강제로 뺏거나 주는 경우에 해당한다.
**`preemptive`**

### 스케쥴링 성능 척도

- 이용률
- 처리량
- 소요 시간
- 대기 시간
- 응답 시간

## [CPU 스케쥴링 #2](https://core.ewha.ac.kr/publicview/C0101020140401134252676046?vmode=f)

# 1. Scheduling Criteria

## Performance Index(=Performance Measure, 성능 척도)

### 1-1. 시스템 입장에서의 성능 척도

- CPU utillization(이용률)
    - 전체 시간 중에서 CPU가 일한 시간의 비율
- Throughput(처리량)
    - 주어진 시간동안 몇 개의 작업을 처리했는지에 대한 양

### 1-2. 프로세스 입장에서의 성능 척도

- Turnaround time(소요시간, 반환시간)
    - CPU를 쓰기 시작해서 다 쓰고 반환하는데 걸리는 시간
- Waiting time(대기 시간)
    - CPU를 쓰기위해 Ready Queue에서 기다리는 시간
- Response time(응답 시간)
    - Readt Queue에 들어와서 CPU를 최초로 얻기까지 걸리는 시간

<aside>
💡 Waiting time 이랑 Response time 이 뭔 차이????

</aside>

- Waiting time은 CPU를 얻었다가 뺏겼다를 반복하는데 그 여러차례 기다린 시간을 합친 개념
- Response time은 단순히 최초로 CPU를 얻기까지 기다린 시간

# 2. Scheduling Algorithms

## 2-1. FCFS (First-Come First-Services; 선입선출)

| Process | Burst Time |
| --- | --- |
| P1 | 24 |
| P2 | 3 |
| P3 | 3 |
- 프로세스 도착 순서가 P1, P2, P3 일 때, 스케줄 순서를 GAntt Chart로 나타내면 다음과 같다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/3cd0ec37-df50-4482-8be9-d79ee7061854/d92ffcf7-90bd-4549-80dd-4e84161c71f4/Untitled.png)

- Wating Time
    - P1 = 0
    - P2 = 24
    - P3 = 27
- Average Waiting Time
    - (0 + 24 + 27) / 3 = 17
- P2, P3의 경우 대화형 프로세스일 경우 burst time이 짧지만, P1때문에 응답시간이 너무 오래걸려서  사용자 경험이 좋지 못하다.

<aside>
💡 만약 저 프로세스 도착 순서가 P3, P2, P1 일 때는 어떻게 될까?

</aside>

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/3cd0ec37-df50-4482-8be9-d79ee7061854/64137a98-559c-4904-be9b-e88f809c2487/Untitled.png)

- Wating Time
    - P1 = 6
    - P2 = 0
    - P3 = 3
- Average Waiting Time
    - (6 + 0 + 3) / 3 = 3

- 긴 프로세스가 먼저 도착해서 짧은 프로세스가 오래 기다려야 하는 현상을 **Convoy Effect** 라고 한다.
- Convoy Effect (호송 효과)
    - 긴 프로세스가 먼저 도착하여 짧은 프로세스가 오래 기다려야 하는 현상
    - FCFS 스케줄링의 단점 중 하나로, 특히 대화형 시스템에서 사용자 경험을 저하 시킬 수 있다.

<aside>
💡 이 방식을 보면 CPU를 짧게 쓰는 프로세스에게 먼저 할당하면 좋은 거구나?

</aside>

## 2-2. SJF (Shortest-Job-First)

- 각 프로세스의 다음번 **`CPU burst time`**을 가지고 스케줄링에 활용
- **`CPU burst time`** 이 가장 짧은 프로세스를 제일 먼저 스케줄
- Two Schemes
    - **Nonpreemptive**
        - 더 짧은 프로세스가 도착하더라도 일단 CPU를 잡으면 이번 CPU burst가 완료 될 때까지 CPU를 선점 당하지 않는다.
            
            
            | Process | Arrival Time | burst Time |
            | --- | --- | --- |
            | P1 | 0.0 | 7 |
            | P2 | 2.0 | 4 |
            | P3 | 4.0 | 1 |
            | P4 | 5.0 | 4 |
            
            ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/3cd0ec37-df50-4482-8be9-d79ee7061854/c5dad0e3-fc1c-4d2f-81cc-5dba5d224951/Untitled.png)
            
        - Average wating time = (0 + 6 + 3 + 7) / 4 = 4
        
    - **Preemptive**
        - 현재 수행중인 프로세스의 남은 burst time보다 더 짧은 CPU burst time을 가지는 새로운 프로세스가 도착하면 CPU를 빼앗긴다.
        - 이 방법을  **`Shortest-Remaining-Time-First` (SRTF)** 라고도 부른다.
            
            
            | Process | Arrival Time | burst Time |
            | --- | --- | --- |
            | P1 | 0.0 | 7 |
            | P2 | 2.0 | 4 |
            | P3 | 4.0 | 1 |
            | P4 | 5.0 | 4 |
            
            ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/3cd0ec37-df50-4482-8be9-d79ee7061854/f5f3d0b9-24c8-44ce-982f-04ce976833dc/Untitled.png)
            
        - 2초가 되면 P2가 도착한다.
            - P1 —> 5초
            - P2 —> 4초
            - 가장 짧은 P2로 전환
        - 4초가 되면 P3가 도착한다.
            - P1 5초
            - P2는 2초
            - P3는 1초
            - 가장 짧은 P3로 전환
        - 5초가 되면 P3 종료 및 P4가 도착한다.
            - P1 5초
            - P2 2초
            - P4 4초
            - 가장 짧은 P2로 전환
        - 7초가 되면 P2 종료
            - P1 5초
            - P4 4초
            - 가장 짧은 P4로 전환
        - 11초가 되면 P4 종료
            - P1 전환
        - Average wating time = (9 + 1 + 0 + 2) / 4 = 3
        - 문맥 교환(Context Swiching)이 빈번하게 발생한다.
- SJF is optimal
    - 주어진 프로세스들에 대해 **`minimum average waiting time`** 을 보장한다.(preemtive)

<aside>
💡 그럼 이게 제일 좋은 건가?

</aside>

- 2가지의 중대한 결점을 가지고 있다.
    1. **Starvation**
        - SJF는 극단적으로 CPU 사용 시간이 짧은 프로세스를 우선순위로 배치하기 때문에 CPU시간이 긴 프로세스의 경우 영원히 실행되지 못할 수도 있다.
    2. **다음 CPU Burst Time을 예측할 수 없다.**
        1. 앞서 설명하기로는 burst Time를 기준으로 설명했는데, 사실 정확하게 말하자면, 얼마나 사용할지 예측이 안되서 예상만 한다.
        2. 과거 CPU burst time을 이용해서 예상한다.

## 2-3. Priorty Scheduling

- 우선순위 스케줄링
- 각 프로세스마다 우선순위를 정하는 숫자(**`priority number`**)가 주어진다.
    - 보통 가장 작은 정수가 우선순위가 높다.
- 우선순위가 가장 높은 프로세스에게 CPU 할당
- Two Schemes
    - **Nonpreemptive**
    - **Preemptive**
- 앞서 설명한 **`SJF`** 는 일종의 우선순위 스케줄링에 해당한다.
    - priorty = CPU burst time
- SJF 처럼 Starvation 문제가 있는데, 이를 해결하기 위해 **`Aging`** 기법을 사용한다.
- **`Aging`**
    - 아무리 우선순위가 낮은 프로세스라도 오래 기다리면 우선순위를 올려주는 기법
    - 마치 경로우대 같은 느낌

## 2-4. RR (Round Robin)

- 현대적인 컴퓨터에서 사용되는 CPU 스케줄링은 Round Robin 기반한다.
- 각 프로세스는 동일한 크기의 할당 시간(**`time quantum`**)을 가진다.
(일반적으로 10 ~ 100  ms)
- 할당시간이 지나면 프로세스는 timer interrupt에 의해 **`preemtive`**  당하고 ready queue의 제일 뒤로 간다.
- n개의 프로세스가 ready queue에 있고 할당시간이 **`q time unit`**인 경우 각 프로세스는 최대 q time unit 단위로 CPU 시간의 1/n을 얻는다.
    - 즉, 어떤 프로세스도 (n-1)q time unit 이상을 기다리지 않는다.

| Process | Burst time |
| --- | --- |
| P1 | 53 |
| P2 | 17 |
| P3 | 67 |
| P4 | 24 |

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/3cd0ec37-df50-4482-8be9-d79ee7061854/fc64c459-7485-488b-9c11-188ef190138f/Untitled.png)

- 일반적으로 SJF보다 average waitng, Turnaround time은 길어질 수 있지만,
response time은 더 짧다.

- 만약 q time unit이 클 경우
    - FCFS 처럼 돌아간다.
- q time unit이 작으면
    - 문맥 교환(Context Switching)이 빈번하게 발생하여, 시스템의 오버 헤드가 증가 해서 시스템 전체 성능이 떨어질 수 있다.

## 2-5. Multilevel Queue

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/3cd0ec37-df50-4482-8be9-d79ee7061854/5359fdfa-0fdf-4a2e-95aa-c5ee420d0fb3/Untitled.png)

- Ready queue를 여러개로 분할
    - **foreground**(**interactive**)
    - **background**(**batch - no human interaction**)
- 각각의 queue는 독립적인 스케줄링 알고리즘을 가진다.
    - **foreground** - **RR**(Round Robin)
    - **background** - **FCFS**(First-Come First-Services)
- queue 에 대한 스케줄링이 필요하다.
    - **Fixed priorty scheduling**
        - 우선순위가 고정된 방식으로 foreground 대기열의 모든 프로세스를 처리하고,
        그 다음에 background 대기열을 처리
        - **starvation**이 발생할 수 있다.
    - **Time Slice**
        - 각 queue에 CPU time을 적절한 비율로 할당
        - 예)
        CPU 시간의 80% 를 foreground 대기열에 RR 방식으로 할당하고, 
        나머지 20%를 background 대기열에 FCFS 방식으로 할당

## 2-6. Multilevel Feedback Queue

- 프로세스가 경우에 따라서 다른 queue로 이동 가능
- Aging이 이런 방식으로 구현할 수 있다.
- Multilevel Feedback Queue schedular를 정의하는 파라미터
    - Queue 의 수
    - 각 queue의 scheduling algorithm
    - Process를 상위 queue로 보내는 기준
    - Process를 하위 queue로 보내는 기준
    - 프로세스가 CPU 서비스를 받으려 할 때 들어가는 queue를 결정하는 기준
    - …

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/3cd0ec37-df50-4482-8be9-d79ee7061854/978d949b-65f1-4f72-a8ee-a03701c1619d/Untitled.png)

- Scheduling
    - new job이 Q0에 들어간다.
    - CPU를 잡아서 할당 시간 8ms 동안 수행
    - 8ms 동안 끝내지 못했으면 Q1로 내려간다.
    - Q1에서 줄을 기다리고 있다가 CPU를 잡아서 16ms동안 수행
    - 16ms에 끝내지 못한 경우 Q2로 내려감
    - …

# 3. Multiple Processor Scheduling

- CPU가 여러 개인 경우 Scheduling은 복잡해진다.
- **Homogeneous processor 인 경우**
    - 화장실 한 줄 서기, 은행 창구의 번호표 같은 느낌
    - Queue에 한 줄로 세워서 각각의 프로세서가 알아서 꺼내갈 수 있게 할 수 있다.
    - 반드시 특정 프로세서에서 수행되어야 하는 프로세스가 있는 경우에 문제가 더 복잡해진다.
        - 이게 무슨 소리야??
            
            예를 들면, 머리 자르러 갔는데 디자이너가 여러 명
            하지만, 나는 내 전담 디자이너 한테 자르는 경우 
            
- **Load Sharing**
    - 일부 프로세서에 job이 몰리지 않도록 부하를 적절히 공유하는 메커니즘이 필요하다
    - 공동 queue 사용
        - 모든 프로세스가 하나의 대기열에 줄을 서고, 각 프로세서가 여기서 프로세스를 꺼내서 실행
        - 부하가 균등하게 분산
    - 별개의 queue 사용
        - 각 프로세서가 자체적인 대기열을 가지고 있어, 프로세스가 특정 프로세서에 할당
        - 부하가 고르게 분산 되지 않을 수 있다.
- **Symmetric Multiprocessing (SMP)**
    - 모든 프로세서가 대등한 관계로, 각 프로세서는 독립적으로 스케줄링을 결정
    - 각 프로세서가 독립적으로 스케줄링 하기 때문에 시스템 전체 부하를 분산
- **Asymmetric Multiprocessing**
    - 하나의 프로세서가 시스템 데이터의 접근과 공유를 책임지고 나머지 프로세서는 거기에 따름
    - 메인 프로세서가 스케줄링, 메모리 관리등을 담당하고,
    나머지 프로세서가 메인 프로세서가 할당한 작업만 수행
    - 시스템 자원의 접근과 제어가 단순하지만 , 메인 프로세서에 과부하가 걸릴 수있음

# 4. Real Time Scheduling

### 4-1. Hard real time systems

- Hard real time task는 정해진 시간 안에 반드시 끝내도록 스케줄링 해야 한다.

### 4-2. Soft real time computing

- Soft real time task는 일반 프로세스에 비해 높은 priority를 갖도록 해야 한다.

# 5. Thread Scheduling

### 5-1. Local Scheduling

- User level thread의 경우 사용자 수준의 thread library에 의해 어떤 thread를 스케줄 할 지 결정.
- 즉  프로세스 내부에서 어떤 thread한테  cpu를 줄 지 결정
- 윈도우는 User level thread를 지원하지 않는다고 앞서 설명 했는데, 좀 더 정확하게 표현하면
Kernel level thread 위에서 User level thread가 동작한다.
    - 다만 커널이 직접적으로 동작에 관여하지 않는다.

### 5-2. Global Scheduling

- Kernel level thread의 경우 일반 프로세스와 마찬가지로 커널의 단기 스케줄러가 어떤 thread한테 cpu를 줄 지 결정

---