---
상위 개념: "[[Class Diagram]]"
---
## Generalization Class Diagram
클래스 간 AS-IS 관계를 갖는 형태를 표현하기 위한 다이어그램 작성 방식들을 설명한다.

## Inheritance

![](https://i.imgur.com/2plw5VU.png)
클래스의 상속 관계는 비어있는 삼각형을 통해 표현한다.

## Abstract Class

> An abstract class is used, for modeling purpose, as a container for definitions, but no instances of the class are of interest.

추상 클래스는 클래스 이름을 이탤릭 체로 표현한다. 추상 클래스는 인스턴스화 될 수 없음에 주의.

## Disjointness Coverage
![](https://i.imgur.com/k4sPG7i.png)

서브클래스가 '분리'되어있는지의 여부를 표현할 때에는 overlapping과 disjoint를 표기할 수 있다. overlapping이 표기된 클래스 구조라면, 서브클래스는 동시에 다른 서브클래스일 수 있다.

![](https://i.imgur.com/rFCMR0T.png)
한 편, disjoint가 표기된 클래스에서 서브클래스는 다른 클래스와 겸 할 수 없다.

## Completeness Coverage 
완전성은 서브 클래스들이 전체를 포괄하는지에 대한 여부를 표현한다.
![](https://i.imgur.com/t4FMptH.png)
complete로 표기된 경우, 모든 수퍼 클래스의 인스턴스는 서브 클래스에 속한다.

![](https://i.imgur.com/zFUcNFe.png)

한 편 incomplete로 표기된 경우, 모든 수퍼 클래스의 인스턴스는 표기된 서브 클래스 외에 존재할 수도 있다.

