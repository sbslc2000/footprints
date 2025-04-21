---
상위 개념: "[[Join Operation Processing]]"
---
# Block Nested-loop Join
Block Nested-loop Join은 Nested-loop Join의 확장이다. Nested-loop Join이 외부 루프는 레코드 단위로 수행되어 내부 릴레이션이 외부 릴레이션의 레코드 개수만큼 읽히는 문제가 있었다. 

Block Nested-loop Join은 이러한 문제를 방지하고자 내부 릴레이션의 모든 블록이 외부 릴레이션의 모든 블록과 쌍을 이루9