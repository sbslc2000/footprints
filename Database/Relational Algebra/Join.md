---
상위 개념: "[[Relational Algebra]]"
---
# Join
릴레이션 하나로 원하는 데이터를 얻을 수 없어 관계가 있는 여러 릴레이션을 함께 사용해야 하는 경우 조인(join)연산을 사용한다. 조인 연산은 조인 속성(join attribute)을 이용해 두 릴레이션을 조합하여 하나의 결과 릴레이션을 구성한다. **조인 연산한 결과 릴레이션은 피연산자 릴레이션에서 조인 속성의 값이 같은 튜플만 연결하여 만들어진 새로운 튜플을 포함한다.**

조인 연산은 기호 $\Join$ 을 사용하여 다음과 같이 표현한다.
$$R \Join S$$ 
## Natural Join
Relation Article

| ARTICLE_ID | ARTICLE_TITLE | ARTICLE_CONTENT |
| ---------- | ------------- | --------------- |
| 1          | A             | AAAA            |
| 2          | B             | BBBB            |
| 3          | C             | CCCC            |
Relation Comment

| COMMENT_ID | ARTICLE_ID | COMMENT_CONTENT |
| ---------- | ---------- | --------------- |
| 1          | 1          | DDDD            |
| 2          | 1          | EEEE            |
| 3          | 2          | FFFF            |

 $$ Article \Join Comment $$

위 연산의 결과 릴레이션은 같은 속성명을 갖는 조인 속성에 대해서 값이 같은 투플끼리 연결하여 생성된 새로운 투플들로 구성된다. 조인 속성은 중복되지 않고 한 번만 표현된다. 조인 속성에 대해 연결될 튜플이 존재하지 않는 3번 Article은 제외되었다.

| ARTICLE_ID | ARTICLE_TITLE | ARTICLE_CONTENT | COMMENT_ID | COMMENT_CONTENT |
| ---------- | ------------- | --------------- | ---------- | --------------- |
| 1          | A             | AAAA            | 1          | DDDD            |
| 1          | A             | AAAA            | 2          | EEEE            |
| 2          | B             | BBBB            | 3          | FFFF            |
 
이러한 방식의 조인은 더 정확히 분류하면 자연 조인(natural join)이고, $\Join_N$ 으로 표현해야 한다. 하지만 자연 조인이 많이 사용되기에 단순히 조인 기호로만 표현하기도 한다.

## Theta Join
세타 조인(theta-join, $\theta$-join)은 자연조인에 비해 더 일반화된 조인이다. 세타 조인은 주어진 조인 조건을 만족하는 두 릴레이션의 모든 튜플을 연결한 새로운 튜플로 결과 릴레이션을 구성한다.
$$ R \Join_{A\theta B}S$$
$A \theta B$는 조인 조건으로 A는 릴레이션 R의 속성이고 B는 릴레이션 S의 속성이다. 그리고 $\theta$는 비교 연산자이다. 결과 릴레이션의 차수는 두 릴레이션의 차수를 합한 것과 같다. 만약 두 릴레이션에 이름이 같은 속성이 존재하는 경우, 결과 릴레이션에서는 '릴레이션이름.속성이름'과 같이 표현하여 둘을 구분한다.

세타 조인의 특수한 예로, $\theta$ 연산자가 = 인 세타 조인을 동일 조인(equi-join)이라고도 부른다.

### Example
$$ Article \Join_{Article.ARTICLE\_ID = Comment.ARTICLE\_ID} Comment $$

| ARTICLE.ARTICLE_ID | ARTICLE_TITLE | ARTICLE_CONTENT | COMMENT_ID | COMMENT.ARTICLE_ID | COMMENT_CONTENT |
| ------------------ | ------------- | --------------- | ---------- | ------------------ | --------------- |
| 1                  | A             | AAAA            | 1          | 1                  | DDDD            |
| 1                  | A             | AAAA            | 2          | 1                  | EEEE            |
| 2                  | B             | BBBB            | 3          | 2                  | FFFF            |

