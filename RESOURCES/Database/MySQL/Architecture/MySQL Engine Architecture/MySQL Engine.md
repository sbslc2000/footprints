---
상위 개념: "[[MySQL Architecture]]"
---
# MySQL Engine

MySQL Engine은 MySQL의 두뇌에 해당하는 부분으로, 클라이언트로부터의 접속 및 쿼리 요청을 처리하는 Connection Handler와, SQL Parser, SQL Preprocessor, Optimizer 등으로 이루어져 있다.

# 이모저모
* MySQL의 5 버전 대에서는 쿼리 캐시를 제공했었지만, 동시 처리 성능 저하와 많은 버그의 원인이 되어 8.0버전 이후로는 삭제되었다.
* 엔터프라이즈 버전에서는 스레드 풀 기능을 제공한다. 이를 해결하기 위해서 Percona Server에서 제공하는 스레드 풀 기능을 플러그인 형태로 사용할 수 있다.
