---
상위 링크: "[[Computer Architecture]]"
---

# Performance
## 컴퓨팅 성능을 평가하는 두 요소
1. 응답시간*Response Time*
	1. 어떠한 태스크가 시작되고 종료되기 까지의 시간
		
![](https://i.imgur.com/PAV7hJf.png)

2. 처리량*Throughput*
	1. 하나의 단위 시간에 처리한 작업의 양이며, 주로 DB 서버의 성능을 평가하는데 사용됨
![](https://i.imgur.com/h2XRrKW.png)

## 정의

성능과 실행시간은 반비례 관계를 갖는다.
**Performance = 1 / execution time (response time)**

- Relative performance : “X is N time faster than Y”
![](https://i.imgur.com/CcN9CYw.png)


During the execution of programs, many things happen
프로그램의 수행 중에는 여러가지 일이 발생한다. 프로그램이 다른 리소스를 획득하느라 CPU에서 이탈할 수도 있다. 이러한 측면에서 성능은 어떻게 정의할 수 있을까?

![](https://i.imgur.com/j0QTCo6.png)

- **Elapsed time** = System Performance = t1 + t2 + t3 + t4 
	- 작업의 시작과 종료에 소요된 모든 시간을 집계한 수치
- **CPU time** = CPU performance = t1 + t4
	- 프로세서에서 수행된 시간만 순수하게 집계한 수치

**Performance = CPU performance = 1 / CPU time**

CPU Time은 또한 다음과 같이 나눌 수 있다.
- **User CPU time**
	- 순수하게 작업의 명령어를 수행하는데 소요된 시간
- **System CPU time**
	- OS에서 해당 작업을 수행하는데에 소요된 총 시간 

## CPU 성능 측정하기

### clock cycle
CPU 성능을 측정하는데에 가장 기본적인 단위는 Clock Cycle이라고 불리는 단위이다. 이는 tick, clock tick, cycles라고도 불린다.

이는 CPU가 기본적인 동작을 수행하는데 필요한 고정적인 시간 단위이이다.

- A discrete time interval in which a CPU can perform a basic operation
- operation of digital hardware governed by a constant-rate clock

![](https://i.imgur.com/0SIBV4p.png)


- **Clock Period** : the duration of a clock cycle
	- 컴퓨터가 하나의 기본적인 동작을 수행하는데에 걸리는 시간
    - how long the computer takes to perform a single basic operation
- **Clock Rate (frequency):** cycles per second = 1 / clock period
	- 1초에 얼마나 많은 기본 동작을 수행할 수 있는지
    - how many basic operations can be performed in 1 second

### Clock Cycle을 통해 분석한 CPU Time
위 공식을 Clock Cycle을 통해 재정의하면 다음과 같은 결과가 나온다.
CPU time for executing a specific program on a specific computer,

**CPU time = Clock Cycles * Clock period * Clock Cycles / Clock rate**

- clock cycles
    - the number of clock cycles required to process the program on the computer
    - how many cycles need to process the program

### how to calculate the number of clock cycles
그렇다면 clock cycles의 개수는 어떻게 계산할까? 이것은 사실 HW의 설계에 따라 달라진다.
In the equation in CPU time = Clock cycles / Clock rate

The value of clock rate is given from HW vendors.
Then, how to calculate **the number of clock cycles** to measure CPU time?

![](https://i.imgur.com/KwcQ1Jv.png)

We can explicitly know the number of machine instructions executed for the program.

The CPU time depends on **the number of machine instructions.**

### calculating CPU time with instruction count
can we say that CPU time = Clock cycles/Clock rate = Instruction count /Clock rate?

Nope

- **Different machines can take different amounts of time**
    - 같은 add operation이라도 프로세서에 따라 다른 clock cycles를 가질 수 있다.
    - even for the same instruction
        - Clock cycles_x ≠ Clock cycles_y
        - instruction count_x ≠ instruction count_y
- **Different instructions can require a different number of clock cycles**
    - add operation 과 mul operation은 clock cycles가 다르다.

### calculating cpu time with CPI
Clock Cycles Per Instruction (CPI)

- The average number of clock cycles each instruction takes to execute
- instruction의 clock cycle의 평균

**CPU time = Clock cycles / Clock rate = Instruction Count * CPI / clock rate**

CPI can be affected by

1. Cost for each instruction type : $CPI_i$
    * this information is given by HW vendors
2. The frequency of each type of instructions : $F_i$ = Instruction Count$_i$ / Instruction count
	* this information can be obtained after compilation
	     	     
![](https://i.imgur.com/MMQIPha.png)    
