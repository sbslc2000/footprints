# Algolia Records
알고리아의 레코드는 key - value 쌍을 갖는 속성들의 모음이다.

```json
{
  "title": "Blackberry and blueberry pie",
  "description": "A delicious pie recipe that combines blueberries and blackberries.",
  "image": "https://yourdomain.com/blackberry-blueberry-pie.jpg",
  "likes": 1128,
  "sales": 284,
  "categories": ["pie", "dessert", "sweet"],
  "gluten_free": false
}
```

## Searchable Attributes
모든 속성들은 기본적으로 검색 가능하지만, 더 나은 검색을 위해서는 오직 검색의 대상이 되어야 하는 속성들만 검색의 대상이 되도록 해야한다. [Searchable Arrtibute](Algolia%20Searchable%20attributes.md)를 통하여 검색가능 속성의 순위를 매김으로써 다른 속성들보다 더욱 가중치를 높게 부여하여 검색의 결과를 바꿀 수도 있다.

## Structuring records
레코드를 구성할 때, 서비스에서 유지하는 모든 데이터를 다 알고리아 레코드로 만들 필요는 없다. 오직 데이터를 검색하기 위한 가치가 있는 정보들만 레코드에 포함시키면 충분하다. 

### rework your data
실전 예시를 통해 알아보자. 다음과 같은 Use Case를 만들고자 한다.
* 어떠한 상품의 제목을 통해 검색을 할 수 있어야 한다.
* 상품의 이미지를 볼 수 있어야 한다.
* 상품의 카테고리를 필터링하여 볼 수 있어야 한다.
* 상품의 리뷰 점수를 기반으로 정렬되어야 한다.

이러한 데이터는 현재 RDB에 저장되어있다고 가정한다. 위 Use Case를 구성하기 위해 필요한 데이터들은 서로 다른 데이터베이스에 존재하며, 이를 수행하기 위해서는 join을 비롯한 복잡한 쿼리가 필요하다. 쿼리 수행 후 나온 결과물은 다음과 같을 것이다.

```json
[
  {
	"id" : "p-1",
	"title" : "상품1",
	"likes" : 30,
	"category" : {
	  "id" : "c-1",
	  "name" : "카테고리1"
    },
    "price" : 10000,
    "image" : {
      "id" : "i-1",
      "link" : "https://..."
      "type" : 'webp'
    },
    "totalReview" : 100,
    "score" : 4.7,
    "createdAt" : "2019-03-01T00:00:00"
  }
]
```

위 데이터에서는 검색에 필요한 것들도 있지만, 그렇지 않은 것들 역시 있다. 예를 들어 likes, createdAt, totalReview, image.type과 같은 필드들은 위 Use Case를 구현하기 위한 검색에 쓸모가 없다. 이러한 필드들은 알고리아 레코드에 포함시키지 않는 것이 더 효과적인 검색에 도움이 될 뿐만 아니라 데이터를 위한 공간을 절약하기도 한다.

### Attributes for searching
만약 넷플릭스 서비스를 이용하고자 하는 사용자가 있다면, 그들은 제목, 줄거리, 장르, 출연진 등에 대해 검색하여 결과물을 얻고자 할 것이다.

알고리아의 searchableAttributes 파라미터를 사용하면, 이러한 필드들에 대해 더욱 가중치를 부여하여 검색 결과를 만들어 낼 수 있다. 기본적으로 알고리아 엔진은 전체 레코드의 내용을 모두 포함하여 검색하지만, searchableAttributes 파라미터와 같은 조정을 통해 더욱 사용자가 원하는 결과를 반환해낼 수 있고, 그 과정에서 이상한 결과가 나오는 것을 줄일 수 있다.

일반적으로 넷플릭스에서 '일본'을 검색하는 경우 그 사용자는 일본 영화가 아닌 제목에 '일본'이 포함된 영화를 찾고 싶어할 가능성이 높다. 따라서 이런 검색에는 'title', 'synopsis', 'director' 등을 searchableAttributes로 제공함으로써 '제작 국가'와 같은 속성에서 값이 검색되는 것을 줄일 수 있다.

### attributes for displaying and filtering
몇몇 속성들은 displaying에 유용하다. 예를 들어 넷플릭스에서 제목, 내용, 카테고리 등이 그렇다. 반면 몇몇 속성들은 filtering에서 유용하기도 하다. 영화 감독, 출시 연도와 같은 경우가 그렇다.

영화 감독과 출시 연도처럼 필터링에 유용한 속성들은 attributesForFaceting 파라미터를 사용하면 유용하다.

필터링 속성들은 어떤 값도 가질 수 있지만, 데이터 일관성을 위해서는 정규화가 필요할 것이다.

### custom ranking attributes
알고리아는 검색어를 기준으로 각 레코드들의 순위를 매겨 결과를 반환한다. 이 과정에서 비즈니스적으로 책정한 수치를 속성으로 제공한다면 검색 결과에 영향을 줄 수 있다.
가령, james bond로 검색하면 제임스 본드가 출연한 영화가 동일한 우선순위를 가지고 반환되겠지만, 이들 속성에 review_score를 함께 제공하여 똑같이 제임스 본드를 검색하더라도 특정 영화가 먼저 결과에 나오도록 만들 수 있다.

## Size
성능을 위해서, aloglia record는 Pricing Plan에 따라 10KB부터 100KB까지의 크기 제한을 둔다.


 
