---
상위 링크: "[[MIPS]]"
---
# MIPS Instruction Format
## R-format
R-format은 오직 레지스터 연산자만 사용자는 명령어 구조이다.
![](https://i.imgur.com/XlNTrux.png)

* op (opcode) : 명령어가 수행하는 연산의 타입
* rs : 연산의 대상인 첫번째 레지스터
* rt : 연산의 대상인 두번째 레지스터
* rd : destination register
* shamt : shift 오퍼레이션에서 사용되는 shift amount
* funct : function code (op에 따라서 특별한 값을 제공할 수 있음)

## I-format
I-format은 상수 연산자를 포함하고 있는 명령어 구조이다.
![](https://i.imgur.com/o5OqKQn.png)
* op : 명령어가 수행하는 연산의 타입
* rs : 연산의 대상인 첫번째 레지스터
* rt : 연산의 대상인 두번째 레지스터
* 이하 16bit는 상수

