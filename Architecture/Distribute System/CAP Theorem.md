# CAP Theorem
![](https://i.imgur.com/ms2P7vh.png)

CAP 이론은 대규모 시스템이 갖는 Consistency, Availability, Partition Tolerance의 속성을 설명한다.

* **Consistency**(일관성) : 스토리지와 관련된 영역에서 상태의 일관성을 의미
* **Availability**(가용성) : 서비스가 어떠한 상황에서도 잘 동작하는 상태를 의미
* **Partition Tolerance**(분단 내성) : 컴포넌트가 분단된 상태에서도 의도한 동작을 수행할 수 있는 능력을 의미

최대 **두 가지 속성만 동시에 만족 가능**하며, 세 가지 모두를 만족하는 서비스는 불가능하다.

CAP 이론은 분산 시스템에서 기본적인 논리로 적용된다. 분산 시스템에서 분단 내성은 필수이므로, 일관성과 가용성간 trade-off를 고려해야 한다.

CAP 이론은 굉장히 넓은 범위의 전제이며, 유용하지 않을 수는 있지만 분산 시스템의 컨셉 이해에 도움을 줌.