---
상위 링크: "[[Software Development Process Model]]"
---
# Waterfall Model

## Concept
Waterfall model은 가장 고전적인 소프트웨어 프로세스의 생애주기를 보여주는 모델이다.

![](https://i.imgur.com/jYEWE7V.png)

1970년 Winwston W Royce에 의해 정립된 폭포수 모델은 소프트웨어 개발이 각각의 단계를 선형적으로 수행한다는 관점으로 바라본다. 폭포수 모델을 통해 수행되는 프로세스에서 각각의 단계는 전(before) 단계가 완전히 종료되며 리뷰와 검증을 거친 뒤 시작된다. 

각각의 관심사를 가진 단계들은 프로젝트의 진척도를 보여주기도 한다. 이 방법은 전형적인 계획-주도적 방법*plan-driven approach*이다.

## Advantages
Waterfall Model은 개념적으로 간단하고, 문제들이 각각의 단계에서 뚜렷하게 구분된다. 가장 직관적인 바업이며, 순차적으로 수행되므로 관리자가 개발 프로세스를 관리하기에 쉽다.

### Usage
Waterfall은 가장 간단한 모델인 만큼 널리 사용된다. 특히 요구사항이 간단하거나, 기술선택에서 어려움이 없는 경우에 잘 맞는다. 또한 Military나 Aerospace industries와 같은 다수의 하부조직이 있는 대규모 프로젝트에서 사용되기에 유리하다.

### Disadvantages
하지만 Waterfall은 몇가지 단점을 갖는다. 최근의 소프트웨어는 개발 초기단계에서 요구사항을 특정하기 어려운데, 초기부터 설계를 수행하고 구현으로 이어지는 Waterfall에서는 요구사항이 변경되었을 때 유연하게 대처하기 어렵다. Waterfall은 하드웨어나 기술들을 제일 먼저 확정한 뒤 다음 내용을 이어가야하기 때문이다.

또한 big bang approach라고 불리는 방법은 소프트웨어의 위험을 초기에 관리하기 어렵게 만든다. Waterfall Model에서 테스트 가능한 소프트웨어는 프로젝트의 후반부에 등장한다. 이는 개발 기간을 3년으로 친다면 그 전에는 아무것도 존재하지 않다가 세 달을 남기고 마지막에 빰하고 등장한다는 것이다!! (all or nothing) 후반부에 치명적인 문제가 발견된다면, 프로젝트는 실패할지도 모른다.

또한 요구사항을 초기에 확정해야하기 때문에, 고객은 실제로 사용될지도 모르는 요구사항을 더 많이 제시*Requirement Bloating*할 지도 모른다. 