---
상위 개념: "[[Non-ordered Index]]"
---
# Bitmap Index
비트맵 인덱스란 다중 키를 가진 질의를 쉽게 처리하기 위해 고안된 특수화된 형태의 인덱스이다.

## 동작 원리
비트맵 인덱스는 다음과 같이 구성된다.
* 릴레이션의 특성 속성이 가질 수 있는 값에 대해 하나의 비트맵을 유지한다.
* 비트맵은 레코드의 개수와 같은 수의 비틀르 갖는다.
* 비트맵의 i 번째 비트는 i 번째 튜플이 해당 값을 가진 경우 1을, 아닌 경우 0을 저장한다.

![](https://i.imgur.com/CUErqwh.png)

위 그림은 비트맵 인덱스의 예시이다. gender는 m 혹은 f의 값을 갖는다. 따라서 두 값에 대한 각각의 비트맵을 갖는다. m 비트맵의 0번째가 1인 이유는 0번 레코드가 m의 값을 가지기 때문이다. m 비트맵의 1번째가 0인 이유는 1번 레코드가 m이 아니기 때문이다. 이와 같은 원리로 income_level에 대한 비트맵도 구성할 수 있다.

```sql
select * from instructor_info where gender="f" and income_level = "L2";
```

위와 같은 질의를 처리해야할 때에는 gender의 f 비트맵과 income_level의 L2 비트맵을 교집합하여 매우 빠르게 대상을 찾아낼 수 있다.

