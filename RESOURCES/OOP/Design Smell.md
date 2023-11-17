# Design Smell

설계원칙은 왜 필요한가? → 설계 악취

설계 악취란 소프트웨어나 시스템이 잘못 design됐을 때 나타나는 신호, 혹은 그런것을 느끼게 해주는 증상을 의미한다.

Design smells = **various signs and symptoms** of bad design

- **Rigidity** : 시스템을 변경하기 어려운 것. 어떤 변경을 하려했더니, 시스템의 다른 부분까지 영향을 미쳐 변경해야하는 것(Change Propagation이 과도하게 일어나는 것)
- **Fragility** : 설계가 망가지기 쉬운것. 코드가 잘못되기가 쉬운 것. 한 군데를 변경했는데, 이것이 다른 부분이 잘못되는데에 원인이 되는 것.
- **Immobility**: 모듈이 재사용하기 어려운 상태. 모듈을 분리하는데에 수고와 어려움이 존재
- **Viscosity**: 설계를 유지하면서 어떤 시스템을 변경하는게 매우 어려운 상태. 정공법보다 꼼수를 사용해야하는 상황이 많이 발생하는 상태.
- **Needless Complexity**: 과도한 설계. 설계가 유용하지 않은 부분을 포함하여 불필요하게 복잡한 것
- **Needless Repetition** : copy-and-paste 가 반복되어, 코드가 중복하여 나타나는 것
- **Opacity** : 만든 사람의 의도를 파악하기 어려운 상태.

![[Pasted image 20230920234124.png]]
