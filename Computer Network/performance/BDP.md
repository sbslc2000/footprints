---
상위 개념: "[[Network Performance]]"
---
# BDP
BDP는 [[Bandwidth]] * [Delay](Latency) Product의 약자로, 한 순간에 전송이 진행중인 비트의 총량을 의미한다. 영문으로는 'the maximun number of bits that could be transit through the pipe at any given instance'.

이는 네트워크 망을 순간 캡처했을 때, 떠다닐 수 있는 최대 비트 수를 의미한다.

![](https://i.imgur.com/qTBGbf3.png)

네트워크 관을 위 처럼 추상화할 때, 관의 길이는 Latency와 관련이 있으며 너비는 Bandwidth와 관련이 있다. Latency는 sec으로 표현되고 bandwidth는 bits/sec으로 표현하므로 둘을 곱하면 bits가 단위가 되며, 이는 관을 차지할 수 있는 비트의 부피가 된다.

이는 통신 효율 계산에 활용된다. 네트워크가 얼마나 놀고있나 일하고 있나 판단할 수 있는 기준이 된다.