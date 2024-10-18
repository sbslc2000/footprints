# Manchester

![](https://i.imgur.com/4btffLi.png)

Manchester은 bit가 0이면 올라가는 신호, bit가 1이면 내려가는 신호를 보내어 인코딩하는 방식이다. 

![](https://i.imgur.com/nIyYtZz.png)

이러한 신호는 보내려는 bit를 NRZ로 표현한 것을 clock과 exclusive-or 연산을 통해 만들 수 있다. exlusive-or은 데이터를 분해할 수 있는 특성을 가지므로, 수신자는 sender의 clock을 뽑아내어 복구를 할 수 있다.

NRZ에서 발생하는 0과 1이 연속되는 문제를 해결한다.