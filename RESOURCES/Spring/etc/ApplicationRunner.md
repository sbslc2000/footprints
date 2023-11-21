# ApplicationRunner
```java
package org.springframework.boot; 
import org.springframework.core.Ordered;
import org.springframework.core.annotation.Order; 
/** * Interface used to indicate that a bean should <em>run</em> when it is contained within * a {@link SpringApplication}. Multiple {@link ApplicationRunner} beans can be defined * within the same application context and can be ordered using the {@link Ordered} * interface or {@link Order @Order} annotation. * * @author Phillip Webb * @since 1.3.0 * @see CommandLineRunner */ @FunctionalInterface 

public interface ApplicationRunner { 
	/** * Callback used to run the bean. 
		* @param args incoming application arguments 
		* @throws Exception on error 
		* */ 
	void run(ApplicationArguments args) throws Exception; 
}
```

ApplicationRunner는 FunctionalInterface로 run이라는 메서드를 갖고 있으며, 이 인터페이스를 구현한 빈을 등록해놓으면 스프링부트의 초기화 작업이 끝난 후 run 메서드가 실행된다.

이를 통해 초기화 작업이 끝난 후 간단하게 무언가 확인하고 넘어가고 싶을 때 활용하기에 좋다. 빈으로 등록되기 때문에 다른 빈을 주입받아서 사용할 수도 있다.