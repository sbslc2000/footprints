# Interrupt  
* ISR들은 매우 빈번하게 호출되므로 빠르게 처리되어야 하기 때문에, 포인터들의 테이블을 만들어 이용한다. 이 테이블은 메인 메모리의 하위에 저장된다. 이 테이블을 **인터럽트 벡터**라고 함.
* 인터럽트 루틴을 처리할 때에 프로세서의 레지스터를 변경할 일이 있을 수 있으므로 문맥을 저장하고 이를 복구하는 과정이 필요하다.
* **Interrupt Request Line**을 CPU가 매 연산이 끝난 뒤 체크하여 인터럽트 발생 여부를 체크한다.
* Maskable Interrupt vs Non-maskable Interrupt
	* Non-maskable Interrupt는 복구할 수 없는 메모리 오류와 같은 이벤트를 위한 인터럽트이며,
	* Maskable은 그 반대개념이다. Maskable은 장치 컨트롤러의 서비스 요청시와 같은 이벤트를 처리할 때 사용되며, Non-maskable Interrupt에 의해 CPU에 의하여 무시될 수 있다.
* 컴퓨터에는 인터럽트 벡터의 주소보다 많은 routine이 있으므로, 이 문제를 해결하기 위해서는 **인터럽트 체인**을 사용해야한다. 주로 연결리스트로 구현하는 듯.