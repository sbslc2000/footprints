---
상위 링크: "[[Javadoc Tag]]"
---
# @implSpec

@implSpec 태그는 자바 8에서 도입된 태그로, 메서드의 내부 동작 방식을 설명한다. 또한 이 메서드가 내부에서 언제 호출되는지 등을 언급하여 이를 오버라이딩 할 때에 참고할 수 있도록 한다.

## 예시
```java
//AbstractCollection 중

/**  
 * {@inheritDoc}  
 * 
 * @implSpec  
 * This implementation iterates over the collection looking for the  
 * specified element.  If it finds the element, it removes the element 
 * from the collection using the iterator's remove method. 
 * 
 * <p>Note that this implementation throws an  
 * {@code UnsupportedOperationException} if the iterator returned by this  
 * collection's iterator method does not implement the {@code remove}  
 * method and this collection contains the specified object. 
 * 
 * @throws UnsupportedOperationException {@inheritDoc}  
 * @throws ClassCastException            {@inheritDoc}  
 * @throws NullPointerException          {@inheritDoc}  
 */
 public boolean remove(Object o) { ... }
```