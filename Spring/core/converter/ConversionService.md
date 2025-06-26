---
"": "[[Spring Type Converting]]"
---
# ConversionService
ConversionService 인터페이스는 개별 컨버터들을 모아두고 그것들을 묶어서 편리하게 사용할 수 있는 기능을 제공한다.

```java
void conversionService() {  
    DefaultConversionService conversionService = new DefaultConversionService();  
    conversionService.addConverter(new StringToIntegerConverter());  
  
    Integer result = conversionService.convert("10", Integer.class);  
    assertThat(result).isEqualTo(10);  
}
```
사실 `addConvert`의 경우 `ConversionService` 인터페이스의 멤버 메서드는 아니고, `ConversionRegistry`의 메서드이다. addConverter를 통해 Converter 구현체를 등록하고, 이를 서비스 인터페이스를 통해 사용할 수 있다.

컨버팅 기능을 사용하는 측에서 구체적인 컨버터를 모르게 만드는 효과를 갖는다.

