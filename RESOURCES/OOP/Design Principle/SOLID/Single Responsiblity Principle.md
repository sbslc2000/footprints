---
상위 개념: "[[SOLID]]"
---
# Single Responsibility Principle (SRP)

소프트웨어는 마치 맥가이버칼과 같아야하나?

> **A Class should have one, and only one, reason to change**

클래스(모듈)은 단 하나의 변경될 이유만 가져야한다.

이 변경될 이유는 responsibility로부터 결정되어야한다. → 각각의 모듈은 하나의 responsibility만 가져야한다.

## Concept

Responsibility란 클래스의 의무이다. R.C. Martin은 Responsibility를 클래스를 변경할 요인으로 봤다.

→ 클래스가 많은 Responsibility를 갖는다면, 자주 변경될 것이다. 자주 변경된다면, 버그를 만들 확률이 높아질 것이며, 변경에 대해 다른 클래스들도 영향을 미칠 것이며, 그는 또다른 버그를 만들어낼 지도 모른다.

따라서 클래스는 적은 책임을 가져야 하고, 이상적으로는 한 종류의 책임만 가져야 한다.

- **Responsibility**
    - “a contract or obligation of a class”
    - **reason to change**
        - More responsibilities == More likelihood of change
        - **The more a class changes, the more likely we will introduce bugs**
        - Changes can impact the others

여러 reponsibilities가 한 클래스에 묶여있다면, 개별적인 클래스로 분리하라.

이것을 측정하기 위해서는 **Cohesion**을 봐야한다.

Cohesion: 하나의 클래스 혹은 모듈이 강하게 관련된 것들의 책임만들 갖고 있는가

하지만 이런것을 극도로 따른다고 해서 디자인이 좋아지는 것은 아니다. 각각의 principle을 너무 과도하게 지키다 보면 더 안좋은 설계가 될 수도 있다. → 클래스의 개수가 많아져 복잡해지고, 관리하고 이해하기 어려워질 수 있다

- **SRP: Separate coupled responsibilities into separate classes**
    - Related measure – **Cohesion**: how strongly-related and focused are the various responsibilities of a module

## Example of SRP violation: Case 1

학생을 sorting 하는 responsibility 를 어디에 부여해야할까?

GRASP에 의하면, sorting을 할 때에 필요한 데이터가 있는 곳에 reponsibility를 부여하는 것이 좋을 수 있다. 이 경우 Student에 Comparable interface를 implement하여 sorting 에 대한 책임을 부여할 수 있다.

이경우는 SRP를 만족하는가?

- Often we need to sort students by their name, or SSN. So one may make Class Student implement the Java Comparable interface.

```java
class Student implements Comparable {
… int compareTo(Object o) { … } …
}
```

SRP는 business entity이고, Student의 client가 sorting의 requirement가 있다고 해서 Student가 가져야한다는 법은 없다.

Student 가 sorting의 책임을 지니게 되면, sorting이 변화할때마다 student를 갖고 사용하는 class들이 모두 recompile되고 테스트되어야한다.

이러한 문제는 Student 라는 클래스가 두가지 다른 responsibility를 bundled하고 있기 때문이다. ( business entity + sorting) 따라서 이러한 설계는 SRP의 위반이다.

- Student is a **business entity**, it does not know in what order it should be sorted since the order of sorting is imposed by the client of Student.
- Worse: every time students need to be ordered differently, we have to recompile Student and all its client.
- Cause of the problems: **we bundled two separate responsibilities** (i.e., student as a business entity with ordering) into one class – a violation of SRP
![[Pasted image 20230920234209.png]]

이 경우 Student가 변경되는 것에 대해 Register과 AClient가 영향을 받게 된다.

Register 는 Student의 sorting에 대한 내용을 사용하지도 않지만, sorting에 대해 알게 된다. (change 의 대상이 된다.)

→ comopareTo 에 대한 책임을 분리해야하지 않나

사실 sorting이란 기능은 AClient가 원하던 기능이다.

### Example of design following SRP

![[Pasted image 20230920234220.png]]

Student class는 그대로 놔두기로 한다. 이 경우 Register 혹은 Student를 알고있는 다른 클래스들은 Recompile 되거나 testing 될 필요가 없다.

개선된 design에서는 AClient가 sorting하는 기능을 두기 위해 SortStudentBySSN이라는 클래스를 만든다. 이 클래스는 주민등록번호로 Student를 정렬할 수 있는 기능을 갖고있고, Comparator를 implement한다.

SRP에 의하면 새로운 기능은 새로운 클래스에 할당해야한다. AClient는 SortStudentBySSN을 사용하게 된다. SortStudentBySSN의 변경은 AClient에게 영향을 준다. 하지만 이것은 불필요한 change propagation이 아니다.

SortStudentBySSN은 Student에 의존한다. SortStudentBySSN의 변경은 Student에는 영향을 주지 않는다.

이를 확장하여 만약 Name을 기준으로 sorting해야하는 requirement 가 발생하는 경우, 이 책임을 구현할 새로운 클래스를 만들 수 있다.

## Example of SRP violation: Case 2

![](https://i.imgur.com/0Qm8BiG.png)


이 경우 responsibilities 가 두개가 혼재해있기 때문에 문제가 발생한다. 만약 area() 의 시그니처가 변화한다면, Rectangle 클래스를 변화시켜야하고, 이로인해 CGA 와 GA가 recompile되어야 한다.

![](https://i.imgur.com/sJZo8Ri.png)

### Example of design following SRP (not enough yet)

![](https://i.imgur.com/t1dnJCF.png)
![](https://i.imgur.com/zBoM2bt.png)

### Example of design following SRP
![](https://i.imgur.com/QmfXsSH.png)

이 그림은 SRP의 적용 결과이다.

draw의 책임은 GraphicRactangle이 갖도록한다.

반면 CGA의 부분은 분리하기 어려워보인다. area라는 메서드는 length와 width를 필요로하기 때문이다.

이러한 설계를 통해, 만약 GA의 변화로 인해 Graphic Rectangle의 draw가 변화해야한다고 하자. 이 경우 Graphic Rectangle의 변화는 GA까지 영향을 미친다. 반면 Geometric Rectangle로는 change propagation 이 파급되지 않는다. 따라서 조금더 개선할 필요가 있다

![](https://i.imgur.com/S0OwpgN.png)
![](https://i.imgur.com/puJh9c9.png)

Rectangle 을 추상화하여 abstract class로 만들고, Geometric Rectangle과 Graphic Rectangle을 이를 상속하게 한다.

이런 구조로는 변경의 여파가 양측에 영향을 끼치지 않는다.

## Identifying Responsibility Can Be Tricky

SRP를 적용할 때, 미묘한 문제들을 맞닥뜨릴 수 있다.

초기에는 그것이 같은 종류의 responsibility로 판단이 됐지만, 확장하다보니 알고보니 서로 다른 종류의 repsonsibility라는걸 나중에 깨달을 수 있다.

- Responsibility (in SRP)
    - A reason for change
    - Note: **sometimes hard to see multiple responsibilities**

![](https://i.imgur.com/Qh0zz3o.png)

이러한 내용들은 Modem이 가져야하는 행동으로 하나의 Responsibility라고 생각할 수 있다. 하지만 이와같이 system 을 design하고 나서, 이후 확장시 다음과 같은 일이 벌어질 수도 있다.

![](https://i.imgur.com/glQhexy.png)

어떠한 모뎀은 dial과 hangup을 가질 필요가 없는 경우가 있다. 따라서 이 메서드들은 서로 다른 책임을 구현하는 것이라는걸 알게 되었다

- Two responsibilities
    - Connection management
    - Data communication

요구사항에 따라서 문제가 앞으로 어떻게 진화할 것이냐에 대한 통찰을 하는 것, 그를 통해 동종의 responsibility와 서로다른 responsibility를 구분하는 것이 필요하다.

- **Lesson: It depends on how the application is changing!**

불필요한 complexity가 생기는 것을 피하라!

DedicatedModem이 생길지 안생길지도 모르는데 미리 분리할 필요는 없다.

- **Beware: Avoid Needless Complexity**
    - **If there is no symptom, it is not wise to apply the SRP or any other principle!**
