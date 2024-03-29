---
상위 링크: "[[System call]]"
---
프로세스의 종료를 알리는 Unix 계열 운영체제의 system call이다.


# return 0 과의 차이
C 프로그램에서 `return 0`과 `exit(0)`은 비슷한 동작을 수행하지만, 몇 가지 중요한 차이점이 있습니다:

1. **사용되는 위치**:
    - `return 0`: `main` 함수 내에서 사용되며, `main` 함수에서 반환 값을 지정하는 데 사용됩니다.
    - `exit(0)`: 어떤 함수에서든 사용 가능하며, 프로그램을 종료하고 반환 값을 지정하는 데 사용됩니다. `exit` 함수는 `main` 함수가 아닌 다른 함수에서도 호출할 수 있습니다.
2. **종료 시점**:
    - `return 0`: `main` 함수에서 `return` 문을 통해 반환되면, `main` 함수의 실행이 완료되고 프로그램이 종료됩니다.
    - `exit(0)`: 어떤 함수에서든 호출되면, 프로그램 전체가 즉시 종료됩니다. 따라서 `exit` 함수를 호출하면 현재 실행 중인 함수의 실행이 중단되고, 프로그램이 종료됩니다.
3. **프로그램 종료 상태**:
    - `return 0`: `main` 함수에서 사용되는 경우, `main` 함수의 반환 값으로 간주되어 프로그램의 종료 상태(exit status)가 0으로 설정됩니다. 이는 프로그램이 정상적으로 종료되었음을 나타냅니다.
    - `exit(0)`: `exit` 함수를 호출할 때 명시적으로 종료 상태(exit status)를 0으로 지정합니다. 이 역시 프로그램이 정상적으로 종료되었음을 나타냅니다.
4. **활용**:
    - `return 0`: 주로 `main` 함수에서 사용되며, 함수 내에서의 반환 값으로 사용됩니다. 일반적으로 `main` 함수의 반환 값이 0이면 프로그램이 성공적으로 실행되었음을 나타냅니다.
    - `exit(0)`: 프로그램을 강제로 종료하고자 할 때 사용됩니다. 예를 들어, 에러 조건이 발생하면 `exit(1)` 또는 다른 비 0 값을 사용하여 프로그램을 종료할 수 있습니다.

요약하면, `return 0`은 `main` 함수에서 사용되고, `main` 함수의 반환 값으로 프로그램의 종료 상태를 설정하는 데 주로 사용되며, `exit(0)`은 어떤 함수에서든 프로그램을 즉시 종료하고 종료 상태를 명시적으로 설정하는 데 사용됩니다.