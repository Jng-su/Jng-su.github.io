---
title: ☕️ Java | Comparator , StringBuilder
date: 2024-09-27 00:00:00 +0800
toc: true
pin: false
published: true
categories: [ONLINEJUDGE]
tags: [coding]
image: https://github.com/user-attachments/assets/dfeebf23-3816-44af-8fb2-f87a845ede8e

---

<br>

---

<br>

![image](https://github.com/user-attachments/assets/ca2ba9a0-45ab-41ae-b03f-3bf395ccc89f)

[https://www.acmicpc.net/problem/1181](https://www.acmicpc.net/problem/1181)

백준이나 프로그래머스에 자주 등장하는 자료구조인 정렬에 대해 매번 찾아보다가 정리하는게 편할 거 같아서 작성 😀

> ## Comparator

java에서 객체의 정렬 기준을 정의할 때 사용하는 인터페이스, **숫자를 내림/오름차순 정렬, 문자열 길이/사전 순 정렬**

- Comparator 인터페이스는 두 객체를 비교하는 메서드인 compare(T o1, T o2)를 제공
- 이 메서드는 두 객체 o1과 o2를 비교하고 그 결과를 기반으로 정렬

    - o1이 o2보다 작으면 **음수**
    - o1이 o2보다 같으면 **0**
    - o1이 o2보다 크다면 **양수**

### How to use 

백문불여일견 일단 해 봐

#### 오름 차순

```java
import java.util.Arrays;

public class TEST {
    public static void main(String[] args) {
        Integer[] numbers = {5, 1, 4, 3, 2};
        
        // 기본 오름차순 정렬 (내부적으로 Comparable이 사용됨)
        Arrays.sort(numbers);
        
        System.out.println(Arrays.toString(numbers));  // 출력: [1, 2, 3, 4, 5]
    }
```

`Arrays.sort`를 가장 많이 사용

#### 내림 차순

```java
import java.util.Arrays;
import java.util.Comparator;

public class TEST {
    public static void main(String[] args) {
        Integer[] numbers = {5, 1, 4, 3, 2};

        Arrays.sort(numbers, new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o2 - o1;
            }
        });

        System.out.println(Arrays.toString(numbers));
    }
}
```

- `Arrays.sort(numbers, Comparator)`: `Arrays.sort`는 배열을 정렬할 때, `Comparator`를 **두 번째 인자**로 받아 정렬 기준을 지정
- `compare` 메서드: `o2 - o1`은 두 값을 비교해 내림차순으로 정렬하는 기준입니다. o2가 더 크면 음수를 반환하여 정렬 순서를 바꿈


#### 사전 순

```java
System.out.println("apple".compareTo("banana"));  // -1 출력 
System.out.println("grape".compareTo("grape"));   //  0 출력 
System.out.println("orange".compareTo("apple"));  //  14 출력 
```

- `compareTo` 함수는 앞A의 문자열과 뒤B의 문자열을 비교
    - A가 B보다 사전순이 우선이면 **음수**
    - A와 B가 같으면 **0**
    - A보다 B가 사전순이 우선이면 **양수**
- 그 차이만 큼 반환하는 것을 볼 수 있다.

<br>

> ## StringBuilder

Java에서 문자열을 효율적으로 처리하기 위해 제공되는 클래스, **문자열을 여러 번 수정, 추가, 삭제, 변경 등** CRUD?

- **String** 객체는 **불변(immutable)**. 즉, String 객체의 값을 변경할 수 없으며, 새로운 값을 할당하거나 수정할 때마다 새로운 객체를 생성
- **StringBuilder**는 **가변(mutable)** 클래스. 즉, 문자열을 수정할 때 객체를 새로 생성하지 않고 내부 버퍼에서 값을 직접 수정

    - `append(String str)`: 문자열을 추가
	- `insert(int offset, String str)`: 특정 위치에 문자열을 삽입
	- `delete(int start, int end)`: 문자열의 특정 범위를 삭제
	- `toString()`: StringBuilder의 내용을 String으로 변환하여 반환

#### append

```java
public class TEST {
    public static void main(String[] args) {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < 5; i++) {
            sb.append(i); 
        }
        System.out.println(sb.toString());
    }
}
```

```
01234
```

#### insert

```java
public class TEST {
    public static void main(String[] args) {
        StringBuilder sb = new StringBuilder("Hello World");
        sb.insert(6, "Java ");
        System.out.println(sb.toString());  
    }
}
```

```
Hello Java World
```

#### delete

```java
public class TEST {
    public static void main(String[] args) {
        StringBuilder sb = new StringBuilder("Hello Java World");
        sb.delete(6, 10);
        System.out.println(sb.toString());
    }
}
```

```
Hello World
```

### toString

```java
public class StringBuilderToStringExample {
    public static void main(String[] args) {
        StringBuilder sb = new StringBuilder("Hello World");
        String result = sb.toString();
        printString(result);
    }

    public static void printString(String str) {
        System.out.println(str);
    }
}
```

```
Hello World
```



<br>

> ## Solve

문제에선 "길이가 짧은 거" 부터라고 했으니 입력받은 문자열.length하면 되겠다. 즉, 오름차순

```java
Arrays.sort(str, new Comparator<String>() {
    @Override
    public int compare(String o1, String o2) {
        return o1.length() - o2.length();
    }
});
```

<br>

근데 `Comparator` 인터페이스는 **사전 순** 정렬이 가능하므로 두 기능을 합칠거임.

```java
Arrays.sort(str, new Comparator<String>() {
    @Override
    public int compare(String o1, String o2) {
        if (o1.length() == o2.length()) {
                return o1.compareTo(o2);
        }
        return o1.length() - o2.length();
    }
});
```

<br>

중복 제거

```java
StringBuilder sb = new StringBuilder();
for (int i = 0; i < N; i++) {
    if (i == 0 || !str[i].equals(str[i - 1])) {
        sb.append(str[i]).append("\n");
    }
}
System.out.println(sb);
```

같지 않으면 `sb` 에 `append` 하고 같으면 무시