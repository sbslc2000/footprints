---
상위 개념: "[[Similarity]]"
---
# Levenshtein Distance
Levenshtein Distance는 한 문자열을 다른 문자열로 바꾸눈 데 필요한 최소 편집 횟수이다. 이 횟수에 해당하는 연산은 3가지로 구분된다.

1. 삽입: 문자 하나를 추가
2. 삭제: 문자 하나를 제거
3. 치환: 문자 하나를 교체

예를 들어 kitten과 sitting의 거리는 3인데, 이는 k를 s로 치환하고 e를 i로 치환하고, g를 삽입한 결과이다.

Levenshtein Distance는 주로 완전히 같지는 않지만 비슷한 문자열을 찾아야 하는 경우, 주로 오타 교정, 맞춤법 교정, OCR 후처리, 어뷰징 탐지 등에 사용된다.