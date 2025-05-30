---
상위 개념: "[[Data Anomaly]]"
---
# **Delete Anomaly**
삭제 이상은 삭제로 인해 발생하는 데이터의 이상현상을 의미합니다.
![](https://i.imgur.com/InKHbAe.png)
위 릴레이션에서 389번에 해당하는 Faculty를 삭제하려고 합니다. 하지만 389번 Faculty를 삭제하기 위해서는 그 Course Code 속성도 함께 삭제됩니다. 이 경우 ENG-206이라는 Course Code는 데이터베이스 상에서 삭제되어 찾을 수 없게 됩니다. 이는 삭제자가 의도했던 바가 아닙니다. 이러한 문제를 삭제 이상이라고 합니다.