---
상위 링크: "[[Javadoc]]"
---
# Javadoc Tag
JavaDoc의 태그는 Java 소스 코드 파일에 삽입되는 특수 주석으로, JavaDoc 도구를 사용하여 자동으로 생성되는 문서에 포함되는 메타데이터를 제공한다. 이 태그들은 Java 클래스, 메소드, 변수 등의 설명을 보다 효과적으로 기술하고 조직화할 수 있게 만든다. 

## 목록
| 태그                  | 설명                               | 사용 위치    |
| ------------------- | -------------------------------- | -------- |
| @param              | 메서드의 파라미터 설명                     | 메서드      |
| @return             | 메서드의 반환값을 설명                     | 메서드      |
| @throws, @exception | 메서드에서 발생할 수 있는 예외를 설명            | 메서드      |
| @see                | 관련된 클래스, 메서드, 문서들을 참조            | 클래스, 메서드 |
| @since              | 이 기능이 언제부터 제공되었는지 명시             | 클래스, 메서드 |
| @author             | 작성자 정보를 명시                       | 클래스, 메서드 |
| @version            | 버전 정보를 명시                        | 클래스      |
| @deprecated         | 해당 요소가 더 이상 사용되지 않으며 삭제될 예정임을 명시 | 클래스, 메서드 |
| @serial             | 직렬화 필드에 대한 설명                    | 필드       |
| @serialData         | 직렬화 데이터 형식에 대한 설명                | 메서드      |
| @serialField        | 직렬화 가능한 필드들에 대한 명시               | 클래스      |