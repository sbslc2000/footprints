AtomicInteger은 원자성이 보장되어 여러 스레드에서 동시에 액세스할 수 있는 Java의 정수형 클래스입니다.

## Public Interface
1. `y = atomic.get();` 와 동등하다 `y = i;`
2. `y = atomic.incrementAndGet();` 와 동등하다 `y = ++i;`
3. `y = atomic.getAndIncrement();` 와 동등하다 `y = i++;`
4. `y = atomic.decrementAndGet();` 와 동등하다 `y = --i;`
5. `y = atomic.getAndDecrement();` 와 동등하다 `y = i--;`
6. `y = atomic.addAndGet(x);` 와 동등하다 `i = i + x; y = i;`
7. `y = atomic.getAndAdd(x);` 와 동등하다 `y = i; i = i + x;`
8. `atomic.set(x);` 와 동등하다 `i = x;`
9. `y = atomic.getAndSet(x);` 와 동등하다 `y = i; i = i + x;`