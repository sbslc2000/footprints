동기화는 OS에서 다루는 주요한 이슈 중 하나입니다.

# 동기화 개요

동기화는 하나의 공유된 자원을 이용하고자 하는 사용자들 사이에서 자원의 무결성을 보장하는 것입니다. 

## Critical Section Problem

자원의 무결성은 여러 이유에 의해서 깨질 수 있습니다. 단일 코어에서 자원을 수정하는 도중 Context Switch가 발생할 때, 다중 코어에서 여러 사용자가 하나의 자원을 동시에 수정할 때 값이 덮어 씌워지는 문제가 발생할 수 있습니다. 이러한 상황을 Race Condition이라고 하며, 이 때 여러 사용자에 대해 공유되는 코드 영역을 Critical Section, 그리고 이러한 상황을 해결하는 문제를 Critical Section Problem이라고 합니다.

## Mutual Exclusion과 발생하는 문제점

CSP는 본질적으로 공유 영역에 여러 사용자가 접근해서 발생하는 문제이므로 Critical Section에 한 순간에 하나의 사용자만 접근할 수 있게 만들면 문제는 해결됩니다. 이러한 접근 방식을 구현한 기술이 Mutex, Semaphore 등의 기술입니다. 

하지만 이러한 기술은 프로세스의 흐름을 제어하므로, Deadlock과 Starvation 등의 문제를 야기할 수 있습니다. OS의 주요 목표는 CSP가 발생하지 않게 하면서 동시에 위와 같은 side effect를 방지하는 것입니다.


