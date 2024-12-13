---
상위 개념: "[[String Data Types]]"
---
# CHAR

* \[NATIONAL] CHAR\(M) \[CHARACTER SET _`charset_name`_] \[COLLATE _`collation_name`_]

Character의 약자인 CHAR은 고정된 길이를 갖는 문자열로 0부터 255의 길이를 가질 수 있다. M을 생략한다면 기본 값은 1이다. 고정된 길이보다 작은 문자열이 들어오는 경우 남은 공간은 빈 문자열이 저장된다.

![UqHnkx7.png](https://i.imgur.com/UqHnkx7.png)

CHAR(0)은 과거에 존재했던 컬럼이 현재에는 사용되지 않지만 열 자체는 존재해야 호환이 될 때에 사용하기에 유용하다. 혹은 2가지의 값만 가질 수 있는 열이 필요할 때에도 유용하다. (NULL 혹은 빈 문자열)
# VARCHAR

* \[NATIONAL] VARCHAR(M) \[CHARACTER SET _`charset_name`_] \[COLLATE _`collation_name`_

VARCHAR은 가변 문자열이다. M은 문자열이 가질 수 있는 최대 길이를 의미하며 0부터 65535까지의 값을 가질 수 있다.

VARCHAR의 유효길이는 charset 을 어떤 것을 쓰냐에 따라서 달라지는데, 예를 들어 utf8mb3을 사용하는 경우에는 문자당 최대 3 byte를 사용하므로 VARCHAR 형태로는 최대 21844개의 문자를 담을 수 있다.

VARCHAR을 저장할 때 MySQL은 1바이트 혹은 2바이트 길이의 공간에 길이에 대한 값을 저장한다. 만약 M이 255 이하인 경우 하나의 바이트만 사용하지만 그 이상인 경우 2개의 바이트를 사용한다.
