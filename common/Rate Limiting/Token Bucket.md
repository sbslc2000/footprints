---
상위 링크: "[[Rate Limiting/Rate Limiting]]"
---
# Token Bucket
Rate Limiting 알고리즘으로, 구현과 이해가 쉬워 Amazon의 DynamoDB 등 다양한 서비스에서 사용된다.

Token Bucket은 Buekct이라고 불리는 토큰들을 모으는 공간이 있다. 이 공간에는 현재 처리 가능한 요청 수를 담고 있다. 요청이 들어오면 토큰이 남아있는지 확인하고, 남아있다면 토큰을 소비하고 요청은 처리된다. 토큰이 부족하다면 요청은 실패시킨다.

토큰은 일정 시간에 따라 충전되어야 하며, 최대 용량을 넘기는 충전은 허용되지 않아야 한다.