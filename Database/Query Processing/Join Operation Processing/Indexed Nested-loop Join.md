---
상위 개념: "[[Join Operation Processing]]"
---
# Indexed Nested-loop Join
중첩 루프 조인에서 만약 내부 반복문의 조인 속성에 대해 인덱스가 구축되어 있는 경우, 인덱스 검색은 파일 스캔을 대치할 수 있다. 외부 릴레이션 r의 각 튜플 $t_r$에 대해 릴레이션 s의 튜플 중 $t_r$과 조인 조건을 만족하는 튜플을 인덱스를 사용해 얻어낼 수 있다.

이 때에는 이미 구축되어 있는 인덱스를 사용할 수 있을 뿐만 아니라, 조인을 수행하기 위해 임시 인덱스를 만들어 사용할 수도 있다.
