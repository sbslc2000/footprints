---
상위 개념: "[[Heap]]"
---
## **Generations**
힙 메모리는 여러 세대(Generations)로 구분된다. 각 세대는 논리적으로 분리된 메모리 공간에 할당되며, 가비지 컬렉션(Garbage Collection)의 성능을 향상시키기 위해 사용된다. 이는 대부분의 객체가 짧은 수명을 갖는다는 ‘객체의 생존 시간’에 대한 통찰을 기반으로 한다.

### **Young Generation**
새로운 객체가 생성되면 항상 Young Generation에 위치한다. 이 영역은 다시 Eden, Survivor0, Survivor1의 세 가지 구역으로 나뉜다. 대부분의 객체는 Eden 영역에서 생성되고, 살아남은 객체는 Survivor 영역으로 이동한다. 일정 횟수 이상 Survivor 영역을 통과한 객체는 Old Generation으로 승격(Promotion)된다.

Young Generation의 공간이 가득 차면 Minor GC(마이너 가비지 컬렉션)가 발생하며, 이때 살아남지 못한 객체는 즉시 제거된다. Minor GC는 상대적으로 자주 발생하지만, 수집 대상이 적고 영역이 작기 때문에 속도가 빠르다.

### **Old Generation**
Young Generation에서 살아남은 객체들은 Old Generation으로 이동한다. 이 영역은 장기 생존 객체들이 존재하는 공간으로, 크기가 크고 GC가 드물게 발생한다. Old Generation이 가득 차게 되면 Major GC 또는 Full GC가 발생하며, 이는 애플리케이션의 전체 동작을 멈추는 Stop-The-World를 유발하므로 성능에 큰 영향을 줄 수 있다.

### **Permanent Generation**
JVM이 클래스를 로딩하면서 생성되는 클래스 메타데이터, 메서드 정보, 정적 변수, 상수 풀 등과 같은 정보가 저장되는 공간이다. 이 영역은 프로그램 실행 도중 클래스의 정의가 변경되지 않는 한, GC의 대상이 되는 일이 거의 없었다.

그러나 이 구조는 고정 크기 할당으로 인해 클래스 로딩이 많은 애플리케이션에서 OutOfMemoryError를 유발할 수 있다는 한계가 있었다.

> [!info] 
> Java 8부터는 Permanent Generation이 제거되고, 그 대안으로 Metaspace가 도입되었다. Metaspace는 힙 외부의 네이티브 메모리를 사용하여 유연하게 메타데이터를 관리하며, 운영체제 메모리를 기반으로 크기가 자동 조절된다는 점에서 안정성과 확장성 면에서 개선되었다.