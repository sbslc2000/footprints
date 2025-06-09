---
상위 개념: "[[Heap]]"
---
## **Generations**
힙 메모리는 여러 세대(Generations)로 구분된다. 각 세대는 논리적으로 분리된 메모리 공간에 할당되며, 가비지 컬렉션(Garbage Collection)의 성능을 향상시키기 위해 사용된다. 이는 대부분의 객체가 짧은 수명을 갖는다는 ‘객체의 생존 시간’에 대한 통찰을 기반으로 한다.

### **Young Generation**
새로운 객체가 생성되면 항상 Young Generation에 위치한다. 이 영역은 다시 Eden, Survivor0, Survivor1의 세 가지 구역으로 나뉜다. 대부분의 객체는 Eden 영역에서 생성되고, 살아남은 객체는 Survivor 영역으로 이동한다. 일정 횟수 이상 Survivor 영역을 통과한 객체는 Old Generation으로 승격(Promotion)된다.

Young Generation의 공간이 가득 차면 마이너 가비지 컬렉션(Minor GC)이 발생하며, 이때 살아남지 못한 객체는 즉시 제거된다. 마이너 가비지 컬렉션은 상대적으로 자주 발생하지만, 수집 대상이 적고 영역이 작기 때문에 속도가 빠르다.

### **Old Generation**
Young Generation에서 살아남은 객체들은 Old Generation으로 이동한다. 이 영역은 장기 생존 객체들이 존재하는 공간으로, 크기가 크고 GC가 드물게 발생한다. Old Generation이 가득 차게 되면 Major GC 또는 Full GC가 발생하며, 이는 애플리케이션의 전체 동작을 멈추는 Stop-The-World를 유발하므로 성능에 큰 영향을 줄 수 있다.
