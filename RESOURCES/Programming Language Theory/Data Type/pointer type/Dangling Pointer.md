---
상위 링크: "[[Pointer Type]]"
---
# Dangling Pointer 

Dangling Pointer는 허상 포인터로 번역되며, 회수된 기억공간을 가리키는 포인터이다. 이미 해제된 메모리 공간을 가리키고 있으므로, 의도와 다르게 동작할 수 있다.

```c
int * 1; 
sub1() { 
	int j; 
	j = 5; 
	i = &j; 
} 

*i = ??
```