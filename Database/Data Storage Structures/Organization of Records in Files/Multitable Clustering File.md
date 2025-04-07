---
상위 개념: "[[Organization of Records in Files]]"
---
# Multitable Clustering File
Multitable Clustering File은 각 블록에 두 개 혹은 그 이상의 릴레이션에 관련되는 레코드를 저장하는 파일 구성 방법이다.
![](https://i.imgur.com/7JM2rp7.png)
![](https://i.imgur.com/W5k1U7Q.png)

위 와 같은 두 릴레이션이 있고, 이들이 dept_name에 대해서 매우 자주 join 연산이 발생한다고 하자. 만약 두 릴레이션이 서로 다른 블록에 있으면 매번 각기 다른 블록을 읽어야 한다. Multitable Clustering File은 위 레코드들을 다음과 같이 저장한다.
![](https://i.imgur.com/eXpC4dt.png)

서로 다른 릴레이션을 하나의 파일에 저장하며, 동일한 dept_name을 갖는 레코드들을 인접하여 저장한다. 이 구조에서는 join 연산이 발생하더라도 더 적은 블록 접근을 발생시킨다.

다중 테이블 군집 구성은 특정한 조인 처리를 빠르게 할 수 있지만, 다른 형태의 질의문은 느려지는 결과를 초래할 수 있다. 가령
```sql
select * from department
```
와 같은 질의문은 결과와 상관없는 instructor 레코드들로 인해 더 많은 블록 접근을 요구하게 된다. department의 순수한 크기가 1블록이고, instructor의 크기가 9 블록일 때, `select * from department`는 최악의 경우 10개의 블록을 읽어야 결과를 반환하게 될 지도 모른다.