---
상위 링크: "[[MySQL Lock]]"
---
# Named Lock
Named Lock은 GET_LOCK() 함수를 이용하여 사용자가 지정한 문자열에 대해 잠금을 획득하고 반납할 수 있다. 주로 여러 클라이언트로부터의 상호 동기화 처리가 필요할 때 사용한다.
```sql
-- 'my-lock' 이라는 문자열에 대해 잠금을 획득한다.
-- 이미 잠금을 사용 중이면, 2초 동안만 대기한다.
SELECT GET_LOCK('my_lock',2); 

-- 'mylock'이라는 문자열에 대한 잠금 여부를 확인한다.
SELECT IS_FREE_LOCK('my_lock');

-- 'mylock'이라는 문자열에 대해 획득했던 잠금을 반납한다.
SELECT RELEASE_LOCK('my_lock');
```