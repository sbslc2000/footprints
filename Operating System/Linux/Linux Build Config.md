# Linux Build Config
리눅스를 빌드할 때에는, 필요한 기능이나 드라이버만 포함시킬 수 있다. 설정 옵션은 CONFIG라는 접두사가 붙는다. (ex. CONFIG_SMP)

일부 설정 옵션은 boolean이거나 tristates일 수 있다. boolean은 *yes*와 *no* 둘 중 하나를 입력받으며, 해당 기능을 on-off하는데에 사용된다.tristates는 *yes*, *no*와 함께 *module*을 입력받을 수 있다. module 설정은 해당 옵션을 사용은 하지만, 분리된 동적 객체(separate dynamically loadable object)로 빌드한다. module로 설정된 기능은 부팅시 자동으로 로드되지 않으며, 특정 상황에서 동적으로 로드/언로드 된다. 주로 USB 장치 드라이버나 네트워크 카드 드라이버에 사용된다.

일부 설정 옵션은 문자열이나 정수 일 수 있고, 이들은 빌드 과정을 제어하지는 않지만 커널 소스가 전처리기 매크로 형태로 접근할 수 있는 값들이다. (ex. 정적으로 할당된 배열의 크기 지정)
