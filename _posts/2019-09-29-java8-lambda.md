---
layout: post
title: 람다 표현식
categories: [Java] 
---

## 람다란?
람다 표현식은 메소드로 전달할 수 있는 익명 함수를 단순화한 것이라고 할 수 있다.

### 람다의 특징

1. 익명
	- 이름이 없다.
2. 함수
	- 람다는 메소드처럼 특정 클래스에 종속되지 않는다. 하지만 메소드처럼 파라미터 리스트, 바디, 반환 형식, 가능한 예외 리스트를 포함한다.
3. 전달
	- 람다 표현식을 메소드 인수로 전달하거나 변수로 저장할 수 있다.
4. 간결성
	- 익명 클래스처럼 이런저런 코드를 구현할 필요가 없다.

<br>

### 람다의 기본 문법

(parameters) -> expression 또는 (parameters) -> { statements; }

<br>

## 람다를 어디에 어떻게 사용할까?

함수형 인터페이스라는 문맥에서 람다 표현식을 사용할 수 있다.

<br>

## 함수형 인터페이스 사용

### 1\. Predicate

java.util.function.Predicate<T>

test라는 추상 메소드를 정의하며 test는 제네릭 형식 T의 객체를 인수로 받아 boolean을 반환한다.

T 형식의 객체를 사용하는 boolean 표현식이 필요한 상황에서 Predicate 인터페이스를 활용 할 수 있다.

```java
@FunctionalInterface
public interface Predicate<T> {
	boolean test(T t);
}

public static <T> List<T> map(List<T> list, Predicate<T> p) {
	List<T> result = new ArrayList<>();

	for (T s : list) {
		if (p.test(s)) {
			result.add(s);
		}
	}
	return result;
}

Predicate<String> nonEmptyStringPredicate = (String s) -> !s.isEmpty();
List<String> nonEmpty = filter(listOfStrings, nonEmptyStringPredicate);
```

### 2\. Function

java.util.function.Function<T, R>

제네릭 형식 T를 인수로 받아서 제네릭 형식 R 객체를 반환하는 apply라는 추상 메소드를 정의한다.

입력을 출력으로 매핑하는 람다를 정의할 때 Function 인터페이스를 활용 할 수 있다

```java
@FunctionalInterface
public interface Function<T, R> {
	R apply(T t);
}

public static <T, R> List<R> map(List<T> list, Function<T, R> f) {
	List<R> result = new ArrayList<>();

	for (T s : list) {
		result.add(f.apply(s));
	}
	return result;
}

List<Integer> numbers = map(
	Arrays.asList("lambdas", "in", "action"), 
	(String s) -> s.length()
);
```