# Searchable Attributes
만약 넷플릭스 서비스를 이용하고자 하는 사용자가 있다면, 그들은 제목, 줄거리, 장르, 출 연진 등에 대해 검색하여 결과물을 얻고자 할 것이다.

알고리아의 searchableAttributes 파라미터를 사용하면, 이러한 필드들에 대해 더욱 가중치를 부여하여 검색 결과를 만들어 낼 수 있다. 기본적으로 알고리아 엔진은 전체 레코드의 내용을 모두 포함하여 검색하지만, searchableAttributes 파라미터와 같은 조정을 통해 더욱 사용자가 원하는 결과를 반환해낼 수 있고, 그 과정에서 이상한 결과가 나오는 것을 줄일 수 있다.

일반적으로 넷플릭스에서 '일본'을 검색하는 경우 그 사용자는 일본 영화가 아닌 제목에 '일본'이 포함된 영화를 찾고 싶어할 가능성이 높다. 따라서 이런 검색에는 'title', 'synopsis', 'director' 등을 searchableAttributes로 제공함으로써 '제작 국가'와 같은 속성에서 값이 검색되는 것을 줄일 수 있다.

## Setting Searchable attributes

### with the API
```javascript
const algoliasearch = require('algoliasearch');  
const {APPLICATION_ID, WRITE_API_KEY} = require("../key");  
  
const client = algoliasearch(APPLICATION_ID, WRITE_API_KEY);  
  
const index = client.initIndex("example-index");  
  
index.setSettings({  
    searchableAttributes: [  
        'title,comments', // 이 경우 title과 comments는 같은 우선순위를 갖는다.  
        'ingredients',  
    ]  
}).then(() => {  
    //done  
})  
  
index.setSettings({  
    searchableAttributes: [  
        'title',  
        'ingredients',  
        'comments', //이 경우 comments는 가장 낮은 우선순위를 갖는다.  
    ]  
}).then(() => {  
    //done  
})

```