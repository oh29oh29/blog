---
layout: post
title: 동작 파리미터화 코드
categories: [Java] 
---

아직은 어떻게 실행할 것인지 결정하지 않은 코드 블록을 의미한다.
코드 블록은 나중에 프로그램에서 호출한다. 

메소드가 다양한 동작을 받아서 내부적으로 다양한 동작을 수행할 수 있다.

### 변화하는 요구사항에 대응

1\. 녹색 사과만 필터링

	```java
	public static List<Apple> filterGreenApples(List<Apple> inventory) {
	    List<Apple> result = new ArrayList<>();
	    for (Apple apple : inventory) {
	        if ("green".equals(apple.getColor())) {
	            result.add(apple);
	        }
	    }
	    return result;
	}
	```

<br>

2\. 원하는 색으로 필터링 (파라미터화)

- 파라미터를 추가하여 원하는 색을 입력받는다.

```java
public static List<Apple> filterApplesByColor(List<Apple> inventory, String color) {
    List<Apple> result = new ArrayList<>();
    for (Apple apple : inventory) {
        if (color.equals(apple.getColor())) {
            result.add(apple);
        }
    }
    return result;
}
```

<br>

3\. 원하는 무게도 필터링

- 원하는 색 필터링 코드와 동일한 형태로 추가한다.

```java
public static List<Apple> filterApplesByWeight(List<Apple> inventory, int weight) {
    List<Apple> result = new ArrayList<>();
    for (Apple apple : inventory) {
        if (apple.getWeight() > weight) {
            result.add(apple);
        }
    }
    return result;
}
```
- 중복 코드가 발생하여 좋은 코드는 아니다.

<br>

4\. 가능한 모든 속성으로 필터링

```java
public static List<Apple> filterApples(List<Apple> inventory, String color, int weight, boolean flag) {
    List<Apple> result = new ArrayList<>();
    for (Apple apple : inventory) {
        if ((flag && color.equals(apple.getColor())) || (!flag && apple.getWeight() > weight)) {
            result.add(apple);
        }
    }
    return result;
}
```

- 매우 좋지 않은 형태의 코드이다.
	- 불분명한 flag의 의미
	- 앞으로 요구사항이 바뀌었을 때 유연한 대응 X

<br>

5\. 추상적 조건으로 필터링 (동작 파라미터화)

```java
public static List<Apple> filterApples(List<Apple> inventory, ApplePredicate p) {
    List<Apple> result = new ArrayList<>();
    for (Apple apple : inventory) {
        if (p.test(apple)) {
            result.add(apple);
        }
    }
    return result;
}
```

- 첫 번째 코드에 비해 더 유연한 코드로 변하였고 가독성도 좋아졌고 사용하기도 쉬워졌다.
- 그러나 filterApples 메소드로 새로운 동작을 전달하려면 ApplePredicate 인터페이스를 구현하는 여러 클래스를 정의한 다음에 인스터스화해야 한다.


```java
List<Apple> heavyApples = filterApples(inventory, new AppleHeavyWeightPredicate());
List<Apple> greenApples = filterApples(inventory, new AppleGreenColorPredicate());
...
```

<br>

6\. 익명 클래스 사용

```java
List<Apple> greenApples = filterApples(inventory, new ApplePredicate() {
	public boolean test(Apple apple) {
		return "green".equals(apple.getColor());
	}
});
```

- 여전히 많은 공간을 차지한다.

<br>

7\. 람다 표현식 사용

```java
List<Apple> greenApples = filterApples(inventory, (Apple apple) -> "green".equals(apple.getColor());
```

- 람다 표현식을 사용하여 훨씬 간결하게 작성할 수 있다.

<br>

8\. 리스트 형식으로 추상화

- Apple 이외에 다양한 물건에서 필터링이 작동하도록 리스트 형식을 추상화할 수 있다.
	
```java
public static <T> List<T> filter(List<T> list, Predicate<T> p) {
    List<T> result = new ArrayList<>();
    for (T e : list) {
        if (p.test(e)) {
            result.add(e);
        }
    }
    return result;
}
	
List<Apple> greenApples = filter(inventory, (Apple apple) -> "green".equals(apple.getColor());
List<String> evenNumbers = filter(numbers, (Integer i) -> 1 % 2 == 0);
```	