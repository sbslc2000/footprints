---
상위 개념: "[[Serializability]]"
---
# Conflict Serializability
만약 스케줄 S가 한 직렬 스케줄과 [충돌 동등](Conflict%20Equivalent)하다면 그 스케줄 S는 충돌 직렬 가능성(Conflict Serializability)를 갖는다고 말한다.

![](https://i.imgur.com/QndMCbo.png)

위 스케줄은 충돌이 발생하지 않는 순서교환을 수행할 수 있다.
1. T1.read(B)와 T2.read(A)의 교환
2. T1.write(B)와 T2.write(A)의 교환
3. T1.write(B)와 T2.read(A)의 교환

교환을 모두 수행하면 다음과 같아진다.

![](https://i.imgur.com/MxNdshf.png)

최초 스케줄과 최종 스케줄은 충돌 동등이며, 최종 스케줄은 직렬 스케줄이므로, 최초 스케줄은 충돌 직렬 가능성을 갖는다.