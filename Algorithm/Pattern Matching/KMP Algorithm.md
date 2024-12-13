---
상위 개념: "[[String Matching Algorithm]]"
---
Knuth-Morris-Pratt Algorithm
# KMP 알고리즘
KMP 알고리즘은 오토마타를 이용한 알고리즘과 유사한 동기를 갖는 알고리즘이다. 오토마타에서는 문자열 매칭 과정을 각각의 상태로 비유하여 실패했을 때 과거의 특정 상태로 돌아가도록 하지만, KMP 알고리즘에서는 문자열 매칭에 실패했을 때 돌아갈 곳을 알려주는 일차원 배열을 준비하고 그것을 통해 텍스트 문자열을 훑어나간다.
![](https://i.imgur.com/cY2V5O6.png)
위의 그림 표현은 마치 패턴 문자열이 일부를 건너뛰고 점프하는 것처럼 표현되어있지만, 실제로는 패턴 문자열에서 c까지 움직였던 포인터가 두번째 a로 이동하는 동작을 한다.. 위 그림은 비유적인 표현일 뿐이다.
![](https://i.imgur.com/QbbUOiG.png)

## Partial Matching Table
Partial Matching Table, PMT는 문자열 매칭에 실패했을 때 돌아갈 곳을 알려주는, 일종의 오토마타이다. KMP 알고리즘을 수행하기 위해서는 먼저 이 일차원 배열을 만들어야한다.
![](https://i.imgur.com/6XA07fu.png)

## Pseudo Code
![](https://i.imgur.com/3dhRR2A.png)
![](https://i.imgur.com/lc6Effs.png)
![](https://i.imgur.com/ZwWwHqa.png)


### 참고자료
https://chanhuiseok.github.io/posts/algo-14/