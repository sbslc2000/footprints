---
상위 링크: "[[Processor Scheduling]]"
---

# FCFS
First-Come-First-Served

## 동작원리
가장 단순한 형태의 스케줄링 정책으로써 FIFO로 동작한다. 큐의 순서를 엄격하게 지키며 비선점모드로 동작한다. 

## 평가
짧은 프로세스보다 긴 프로세스에게 유리하며 (짧은 프로세스는 정규화된 반환 시간이 길어지는 경향이 있다) 처리기 중심(Processor-Bounded)의 프로세스를 우대하는 경향이 존재한다(processor와 I/O 모두에게 있어서 비효율적인 스케줄링 정책). 