---
상위 개념: "[[Pipelining Evaluation]]"
---
# Producer-driven Pipeline
생산자 구동 파이프라인에서 각 연산은 튜플에 대한 요구를 기다리지 않고, 튜플을 즉시 생성한다. 각 연산은 시스템 내의 별도의 프로세스 혹은 스레드로 모델링한다.

이를 지원하기 위해 시스템은 연산자의 출력이자 다음 연산자의 입력이 되는 튜플을 저장할 수 있는 버퍼 공간을 마련해야한다. 연산은 출력 버퍼가 꽉 찰 때까지 진행하며, 버퍼가 꽉 찬다면 기다린다. 병렬 처리 시스템에서 매우 유용하게 사용된다.