---
상위 개념: "[[CSS]]"
---
# position
CSS의 position 속성은 요소를 문서 내에서 어떻게 배치할지를 제어하는 핵심 속성이다. 기본 흐름에서 벗어나 정확한 위치 지정이나 고정 배치, 겹치기 등을 구현할 수 있게 해준다.

```css
position: static | relative | absolute | fixed | sticky;
```
position이 적용된 요소에는 top, right, bottom, left를 통해 위치를 상세히 지정할 수 있다.

## static 
기본 위치 지정 방식이며, 요소는 일반적인 문서 흐름에 따라 배치된다. top, left와 같은 속성은 무시된다.

## relative
자기 자신의 원래 위치를 기준으로 이동한다. 문서의 흐름을 깨지 않으며, 주변 요소의 레이아웃에 영향을 주지 않는다. 보통 absolute 요소의 기준점으로 자주 사용된다.

## absolute
가장 가까운 relative, absolute, fixed를 가진 조상 요소를 기준으로 배치된다. 해당 요소는 문서 흐름에서 제거되며, 다른 요소들과 겹칠 수 있다.

## fixed
브라우저 뷰포트를 기준으로 위치가 고정된다. 해당 요소는 스크롤 해도 항상 같은 위치에 표시된다.

## sticky
sticky 속성을 받은 요소는 지정된 기준점에 도달하기 전까지는 relative처럼 동작하고, 도달하면 fixed처럼 고정된다.