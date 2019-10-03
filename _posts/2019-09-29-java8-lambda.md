---
layout: post
title: 람다 표현식
categories: [Java] 
---

익명 클래스로 다양한 동작을 구현할 수 있지만 코드가 깔끔하지는 않다. 
더 깔끔한 코드로 동작을 구현하거나 전달할 수 있게 하는 자바 8의 새로운 기능인 람다 표현식이 있다. 

<br>

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

### 1\. 함수형 인터페이스

많은 디폴트 메소드가 있더라도 추상 메소드가 오직 하나인 인터페이스를 함수형 인터페이스라고 부른다.

람다 표현식으로 함수형 인터페이스의 추상 메소드 구현을 직접 전달할 수 있으므로 전체 표현식을 함수형 인터페이스의 인스턴스로 취급(함수형 인터페이스를 
concrete 구현한 클래스의 인스턴스)할 수 있다.

<br>

### 2\. 함수 디스크립터

함수형 인터페이스의 추상 메소드 시그너처는 람다 표현식의 시그너처를 가리킨다. 
람다 표현식의 시그너처를 서술하는 메소드를 함수 디스크립터(function descriptor)라고 부른다.

<br>

	@FunctionalInterface란?
	- 함수형 인터페이스임을 가리키는 어노테이션
	- 실제로 함수형 인터페이스가 아니면 컴파일 에러가 발생한다.

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

### 2\. Consumer

java.util.function.Consumer<T>

제네릭 형식 T 객체를 받아서 void를 반환하는 accept라는 추상 메소드를 정의한다.

T 형식의 객체를 인수로 받아서 어떤 동작을 수행하고 싶을 때 Consumer 인터페이스를 사용할 수 있다.

```java
@FunctionalInterface
public interface Consumer<T> {
	void accept(T t);
}

public static <T> void forEach(List<T> list, Consumer<T> c) {
	for (T s : list) {
		c.accept(t);
	}
}

forEach(
	Arrays.asList(1, 2, 3, 4, 5), 
	(Integer i) -> System.out.println(i)
);
```

### 3\. Function

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