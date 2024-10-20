---
상위 링크: "[[Similarity]]"
---
# Eudclidean Distance

![](https://i.imgur.com/DVlNOZT.png)


Euclidean space 내의 서로 다른 두 점의 길이를 의미한다.

## Formula

$$d = \sqrt{\sum_{i=1}^{n} (x_i - y_i)^2}$$

n : number of dimensions 
$x_k$ , $y_k$ : $k^{th}$ attributes of data objects x and y

## 유의사항

특정 값이 너무나도 크다면 다른 값이 상수화되어 전체 크기에서 무시되는 경우가 있다. 따라서 attribute간 데이터 스케일이 너무 다른 경우 이를 정규화해줄 필요성이 있다.