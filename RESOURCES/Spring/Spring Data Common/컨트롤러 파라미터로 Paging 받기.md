## RequestHandler 메서드에서 Pageable과 Sort 매개변수 사용
Spring Web MVC에서는 HandlerMethodArgumentResolver를 통해 컨트롤러의 파라미터로 다양한 바인딩 방법을 제공한다. Spring Data는 추가적인 MethodArgumentResolver를 제공하여 Pageable 과 Sort도 컨트롤러의 파라미터로 받을 수 있게 한다.

```java
@GetMapping("/posts")  
public Page<Post> getPosts(Pageable pageable) {  
    return postRepository.findAll(pageable);  
}
```

