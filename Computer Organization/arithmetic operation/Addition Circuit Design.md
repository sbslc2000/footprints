---
상위 링크: "[[../Computer Architecture|Computer Architecture]]"
---
# Addition Circuit Design

## Basic Concept
컴퓨터에서 덧셈 연산은 이진수의 합을 도출하는 과정으로 이루어진다.
![](https://i.imgur.com/YWpr19Y.png)

## Half Adder
![](https://i.imgur.com/5extqVm.png)

Half Adder은 XOR과 AND 회로를 사용하여 하나의 비트에 대한 덧셈 결과를 반환하는 회로 설계 방식이다. 합과 받아올림을*carry* 반환하지만, 입력에서 받아올림을 넣을 수 있는 방법이 없다.

| A   | B   | S = A XOR B | C = A AND B |
| --- | --- | ----------- | ----------- |
| 0   | 0   | 0           | 0           |
| 0   | 1   | 1           | 0           |
| 1   | 0   | 1           | 0           |
| 1   | 1   | 0           | 1           |

## Full Adder
![](https://i.imgur.com/ar23s5N.png)

Full Adder은 더욱 복잡한 회로를 통해 받아올림을 반영한 결과를 반환한다.

| A   | B   | C$_{in}$ | S = (A XOR B) XOR C$_{in}$ | C$_{out}$ = (A AND B) OR ((A XOR B) AND C$_{in}$) |
| --- | --- | -------- | -------------------------- | ------------------------------------------------- |
| 0   | 0   | 0        | 0                          | 0                                                 |
| 0   | 0   | 1        | 1                          | 0                                                 |
| 0   | 1   | 0        | 1                          | 0                                                 |
| 0   | 1   | 1        | 0                          | 1                                                 |
| 1   | 0   | 0        | 1                          | 0                                                 |
| 1   | 0   | 1        | 0                          | 1                                                 |
| 1   | 1   | 0        | 0                          | 1                                                 |
| 1   | 1   | 1        | 1                          | 1                                                 |

## N-bit parallel binary adder
결국 N 개의 비트로 이루어진 정수에 대한 연산을 수행하기 위해서는 Full Adder가 병렬적으로 연결된 구조의 회로가 필요하다.
![](https://i.imgur.com/eyF91ca.png)

최초의 받아올림 값은 0이고, 이후의 덧셈기들은 이전 덧셈기의 받아올림 값을 입력값으로 하여 동작해야하므로 기다려야 한다.