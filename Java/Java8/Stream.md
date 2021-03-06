# Java Stream

기존에 배열, 컬렉션을 다루는 방법은 for, foreach를 통해서 요소를 하나씩 꺼내서 다루었으나    
자바8 이후로는 Stream의 람다를 통해 좀 더 간결하게 해결할 수 있다.    
스트림은 **데이터의 흐름**이다. 배열, 컬렉션에 함수 여러개를 조합해서 원하는 결과를 얻을 수 있다.   
람다를 활용하여 코드를 간결하게 하고 배열과 컬렉션을 함수형으로 처리할 수 있다.    
또 병렬처리가 가능하여 쓰레드를 이용해 많은 요소들을 빠르게 처리할 수 있다.   

스트림은 세 단계로 나뉜다.
1. 생성 : 스트림 인스턴스 생성
2. 가공 : 필터링, 맵핑 등 원하는 결과를 만든다.
3. 결과 : 최종 결과를 만들어 낸다.   

## Stream 생성 방법
```java
    
    List<Integer> list = Arrays.asList(5, 1, 4, 2, 3);
    list.stream();  // Collection에서 스트림 생성

    Integer [] arr = {5, 1, 4, 2, 3};
    Arrays.stream(arr); // 배열로 스트림 생성

    Stream<Integer> stream = Stream.of(5, 1, 4, 2, 3);  // 스트림 직접 생성
```

**기존에 사용하던 방식**
```java
    List<String> list = Arrays.asList("5", "1", "4", "2", "3");
    long count = 0;

    for (String item : list) {
        if (item.contains("3")) {
            count++;
        }
    }
    System.out.println("count : " + count);  // 1
```

**Stream 사용 방식**
```java
    List<String> list = Arrays.asList("5", "1", "4", "2", "3");
    long count = 0;

    count = list.stream().filter(x -> x.contains("3")).count();
    System.out.println("count : " + count);  // 1
```


## Stream 사용 방식

**Collections같은 객체 집합.스트림 생성().중개 연산().최종 연산();** 이런 파이프라인 방식으로 사용한다.   

중개 연산과 최종 연산에 어떤것들이 있는지 살펴보자.   


### 중개 연산
##### 1. Filter
조건에 맞는 것만 걸러서 반환한다.
```java
List<String> list = Arrays.asList("5", "1", "4", "2", "3");

Stream<String> result = list.stream().filter(x -> x.contains("3"));  // 3 반환하여 result에 저장.
```

##### 2. Map
map은 스트림 각 요소를 연산하는데 사용된다. 다음 코드는 각 숫자를 *2 하는 연산을 하고 출력한다. 
```java
List<Integer> list = Arrays.asList(5, 1, 4, 2, 3);

list.stream().map((x) ->{return x*2;}).forEach(x -> System.out.println(x));  // 10, 2, 8, 4, 6
```

```java
List<String> alphabet = Arrays.asList("a", "b", "c");
Stream<String> stream = alphabet.stream().map(String::toUpperCase);  // [A, B, C]
```

#### 3. flatMap
새로운 스트림을 생성해서 리턴하는 람다를 넘겨야 한다. flatMap은 중첩 구조를 한 단계 제거하고 단일 컬렉션으로 만들어주는 역할을 한다.
```java
List<List<String>> list = Arrays.asList(Arrays.asList("a"), Arrays.asList("b"));  // [[a], [b]]
```
이제 flatMap을 사용하여 중첩 구조를 제거한다.

```java
List<String> flatList = list.stream().flatMap(Collection::stream).collect(Collectors.toList());  // [a, b]
```

다음은 성적표에 국영수 점수를 가져와 새로운 스트림을 만들어 평균을 구하는 코드이다.
```java
reportCard.stream().flatMapToInt(report -> IntStream.of(report.getKor(), report.getMath(), report.getEng()))
.average().ifPresent(avg -> System.out.println(Math.round(avg * 10)/ 10.0));
```

### 최종 연산
##### 1. reduce
reduce는 값을 누적하여 계산할 때 사용한다. Collection에 있는 값 중 2개를 꺼내서 계산을 한다.   
a,b에 2개의 값을 넣어서 a+b를 진행하고, 그 결과 값이 a가 되고, 스트림에서 꺼낸 다음 값이 b가 되어 연산이 진행 된다.
```java
List<Integer> list = Arrays.asList(1, 2, 3);
list.stream().reduce((a,b) -> a+b).get();  // 6
```

##### 2. collect
Stream의 값들을 toMap, toSet, toList로 다른 Collection으로 바꿔 준다.
```java
List<Integer> list = Arrays.asList(1, 2, 3);

Set<Integer> set = list.stream().collect(Collectors.toSet());  // list -> set
set.forEach(x-> System.out.println(x));  //1, 2, 3
```

```java
String [] arr = {"1", "2", "3"};
arr.stream().collect(Collectors.toList()); // ["1", "2", "3"]
```

##### 3. min, max, count
가장 작은 값, 큰 값, 갯수를 구한다.
```java
List<Integer> list = Arrays.asList(5, 9, 1);

System.out.println(list.stream().count());  // 3
System.out.println(list.stream().min(Integer::compare).orElse(-1));  // 1
System.out.println(list.stream().max(Integer::compare).orElse(-1));  // 9
```

## 활용
**1.여러개의 리스트를 하나의 리스트로 병합 할 수 있다.**
```java
List<String> list1 =  Arrays.asList("one", "two");
List<String> list2 =  Arrays.asList("three","four","five");
List<String> list3 =  Arrays.asList("six");
List<String> finalList = Stream.of(list1, list2, list3).flatMap(Collection::stream).collect(Collectors.toList());
System.out.println(finalList);  // [one, two, three, four, five, six]
```

**2.Map의 Value들을 하나의 리스트로 병합할 수 있다.**
```java
Map<String, List<Integer>> map = new LinkedHashMap<>();
map.put("a", Arrays.asList(1, 2, 3));
map.put("b", Arrays.asList(4, 5, 6));

List<Integer> allValues = map.values() // Collection<List<Integer>>
        .stream()                      // Stream<List<Integer>>
        .flatMap(List::stream)         // Stream<Integer>
        .collect(Collectors.toList());

System.out.println(allValues);  // [1, 2, 3, 4, 5, 6]
```

**3.Map, List를 단일 연속 Stream으로 병합**
```java
        List<Map<String, String>> list = new ArrayList<>();
        Map<String,String> map1 = new HashMap();
        map1.put("1", "one");
        map1.put("2", "two");

        Map<String,String> map2 = new HashMap();
        map2.put("3", "three");
        map2.put("4", "four");
        list.add(map1);
        list.add(map2);


        Set<String> output= list.stream()  //  Stream<Map<String, String>>
                .map(Map::values)              // Stream<List<String>>
                .flatMap(Collection::stream)   // Stream<String>
                .collect(Collectors.toSet());  //Set<String>
        // [one, two, three,four]
```


#### 주의 사항
1. Stream은 재사용이 불가능하다.   
2. parallelStream은 여러 쓰레드가 작업한다.    

list.**parallelStream()**.map((x) ->{return x*2;}).forEach(x -> System.out.println(x));  // 10, 2, 8, 4, 6    
이렇게 하면 여러 쓰레드가 스트림에서 요소를 작업하고 쓰레드끼리 각자 작업한 값들을 처리해서 리턴한다.    
그렇기 때문에 결과과의 순서가 예상하는 바와 다를수가 있다.