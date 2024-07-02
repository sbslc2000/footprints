---
상위 링크: "[[Algolia]]"
---
# Nested Attributes
Nested Attributes는 속성의 서브카테고리를 정의할 수 있는 방법이다. 예를 들어, price라는 하나의 속성에 대해서 `price.net`, `price.gross`, `price.margin`과 같은 다른 값들을 설정할 수 있다. 서브 카테고리를 부모 요소와 구분 짓기 위해서는 'dot notation'을 사용한다.

## How to create nested attributes
Nested Attribute는 다음과 같은 방식으로 JSON에서 표현된다.
```json
[
  {
    "country": "CA",
    "price": {
      "net": 2.30,
      "gross": 2.62
    }
  },
  {
    "country": "US",
    "price": {
      "net": 1.99,
      "gross": 1.75
    }
  }
]
```

## An example of filtering nested attributes with an API client

다음과 같은 방식으로 nested value에 대한 필터링 결과를 얻을 수 있다.
```javascript
const search = async () => {  
    const hits = await index.search('', {  
        filters: 'price.gross < 2.31',  
});  
  
    console.log(hits);  
}

//hits: [
//    {
//      country: 'US',
//      price: [Object],
//      objectID: //'1d35d0a3e25002_dashboard_generated_id'
//    }
//  ],
//  nbHits: 1,
// ... 

```

## Where you can use nested attributes
Nested Attribute는 searchableAttribute나 attributesForFaceting과 같은 일반적인 속성들에 모두 사용할 수 있다. 해당 값들을 가리키기 위해서 dot notation을 쓰는 것에 주의해라.

Record의 nested attribute의 깊이에 대해서는 제한이 없으니 price.net.us.ca 와 같이 사용할 수도 있다.