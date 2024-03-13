
# Two metrics for defining computer performance

1. **Response Time**
	* The time between the start and completion of a **task**
		* e.g., how long it takes todo a **single task**
		
![](https://i.imgur.com/PAV7hJf.png)

2. **Throughput**
	* A total amount of works done per unit time
        * e.g., **tasks** per hour
        * usually used in DB server
![](https://i.imgur.com/h2XRrKW.png)

## Defining performance

### **Performance = 1 / execution time (response time)**

성능과 실행시간은 반비례 관계를 갖는다.
- Relative performance : “X is N time faster than Y”
![](https://i.imgur.com/CcN9CYw.png)


During the execution of programs, many things happen

프로그램 실행 중 cpu에서 다른 task가 실행되거나, task가 다른 resources를 사용할 수 있다.

![](https://i.imgur.com/j0QTCo6.png)

- **Elapsed time** = System Performance = t1 + t2 + t3 + t4
    - the total time between the start and completion of a task, including everything.
- **CPU time** = CPU performance = t1 + t4
    - the time spent processing a given task **on a processor**

**Performance = CPU performance = 1 / CPU time**

CPU time can be divided into
- **User CPU time**
    - the time spent for processing the code of the program
- **System CPU time**
    - the time spent in the operating system performing tasks for the program

## Measuring CPU Performance

### clock cycle

The basic unit of CPU performance : **Clock cycle** (=tick, clock tick, cycles)

- A discrete time interval in which a CPU can perform a basic operation
- operation of digital hardware governed by a constant-rate clock

![](https://i.imgur.com/0SIBV4p.png)


- **Clock Period** : the duration of a clock cycle
    - how long the computer takes to perform a single basic operation
- **Clock Rate (frequency):** cycles per second = 1 / clock period
    - how many basic operations can be performed in 1 second

### CPU time using clock cycle

CPU time for executing a specific program on a specific computer,

**CPU time = Clock Cycles * Clock period * Clock Cycles / Clock rate**

- clock cycles
    - the number of clock cycles required to process the program on the computer
    - how many cycles need to process the program

### how to calculate the number of clock cycles
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
