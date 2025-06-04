---
상위 개념: "[[JVM]]"
---
# Heap
힙 공간은 객체와 JRE 클래스의 동적 메모리 할당에 사용된다.

런타임에 새로운 객체는 항상 힙에 저장되며, 이들은 참조값이 있는 한 어디에서든 접근할 수 있다. 스택 영역에는 이 데이터들의 참조값(주소값)만이 저장된다.

## Generations
힙 메모리는 다음과 같은 세대(Generations)로 구분된다. 각 세대는 논리적으로 구별된 메모리 공간에 할당되어, 가비지 컬렉션의 성능을 높인다.

### Young Generation
새로운 객체가 생성되면 항상 Young Generation에 할당된다. 객체가 일정 시간 머무르면(aged), Old Generation으로 이동한다.

Young Generation 영역이 가득 차면 마이너 가비지 컬렉션(Minor GC)가 발생한다.

### Old Generation
오랜 기간 동안 존재하는 객체들이 저장된다.

### Permanent Generation
JVM의 메타데이터(클래스 정보, 메서드 정보)가 저장된다.
> [!info]
> Java 8 이후부터는 메타스페이스(Metaspace)로 대체되었다.
