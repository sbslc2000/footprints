---
상위 개념: "[[Evaluation of Expressions]]"
---
# Pipelining Evaluation
파이프라이닝 평가란 하나의 연산의 결과를 파이프라인을 통해 다음 연산으로 바로 넘겨주는 방식을 의미한다. 예를 들어 외부-정렬 합병 연산의 결과를 추출해야하는 질의 계획이 있을 때, 합병의 결과를 디스크에 저장해놓지 말고, 바로 추출 연산의 입력값으로 보내는 것이다. 

이를 통해 디스크에 임시 릴레이션을 저장하는 행위를 줄여 질의 처리에 효율성을 향상할 수 있으며, 경우에 따라 질의 처리가 다 끝나지 않은 시점에도 결과를 사용자에게 제공할 수 있다.