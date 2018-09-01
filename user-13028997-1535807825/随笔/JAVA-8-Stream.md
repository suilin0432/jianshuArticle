详细介绍JAVA 8 Stream: https://www.ibm.com/developerworks/cn/java/j-lo-java8streamapi/

---

Stream 作为 Java 8 的一大亮点，它与 java.io 包里的 InputStream 和 OutputStream 是完全不同的概念。它也不同于 StAX 对 XML 解析的 Stream，也不是 Amazon Kinesis 对大数据实时处理的 Stream。Java 8 中的 Stream 是对集合（Collection）对象功能的增强，它专注于对集合对象进行各种非常便利、高效的聚合操作（aggregate operation），或者大批量数据操作 (bulk data operation)。Stream API 借助于同样新出现的 Lambda 表达式，极大的提高编程效率和程序可读性。同时它提供串行和并行两种模式进行汇聚操作，并发模式能够充分利用多核处理器的优势，使用 fork/join 并行方式来拆分任务和加速处理过程。通常编写并行代码很难而且容易出错, 但使用 Stream API 无需编写一行多线程的代码，就可以很方便地写出高性能的并发程序。所以说，Java 8 中首次出现的 java.util.stream 是一个函数式语言+多核时代综合影响的产物。

---

- 流的产生:

  数据源 -> 数据转换 -> 执行操作获取想要的结果。
  每次转换过后原有Stream对象不改变，返回一个新的Stream对象，允许操作链式排列成为一个管道。
  产生方法:
 ```
//从Collection和数组
1.Collection.stream()
2.Collection.parallelStream()
3.Arrays.stream(T array) or Stream.of()
4.java.io.BufferReader.lines()
//静态工厂
5.java.util.stream.IntStream.range()
6.java.nio.file.Files.walk()
//其他
7.java.util.Spliterator
8.Random.ints()
9.BitSet.stream()
10.Pattern.splitAsStream(java.lang.CharSequence)
11.JarFile.stream()
 ```
---

- 流的两种操作类型:
  - 1.Intermediate:一个流后面跟随任意数量的intermediate操作。目的是为了打开流，对流进行数据映射/过滤然后返回一个新的流。PS:这类操作是惰性的，仅仅调用到这类方法的时候没有真正开始流的遍历。
  -  2.Terminal: 一个流只能有一个terminal操作，当这个操作执行过后，流就会被使用完毕，不能再再次被操作。只有一个Terminal操作被调用的时候流才会开始真正的遍历，并且最终生成一个结果，或者一个side effect(副作用)。
  - PS:还有一种叫做short-circuiting的操作，他具有 1.对于一个intermediate操作，如果它接受的是一个无限大(infinite/unbounded) 的 Stream，但返回一个有限的新的Stream.
2.对于一个terminal操作，如果它接受的是一个无限大的Stream，但能在优先的时间内计算出结果。

---

- 流的使用:
```
eg: 
//1.Individual values
Stream stream = Stream.of("a","b","c");
//2.Arrays
String[] strArray = new String[] {"a","b","c"}
stream = Stream.of(strArray);
stream = Arrays.stream(strArray);
//3.Collections
List<String> list = Arrays.asList(strArray);
stream = list.stream();
```
```
//流对变量的包装
Stream: IntStream、LongStream、DoubleStream。 或者使用Stream<Integer>、Stream<Long>、Stream<Double>,但是后三者装箱拆箱操作很耗时，所以为基本类型提供了相应的Stream操作
eg:
IntStream.of(new int[] {1,2,3}).forEach(System.out::println);//输出所有其中数字
IntStream.range(1,3).forEach(System.out::println);//range不包含右边界3
IntStream.rangeClosed(1,3).forEach(System.out::println);//rangeClosed包含右边界3
```

```
//1. Array
String[] strArray1 = stream.toArray(String[] ::new);
//2.Collection
List<String> list1 = stream.collect(Collectors.toList());
List<String> list2 = stream.collect(Collectors.toCollection(ArrayList::new));
Set set1 = stream.collect(Collectors.toSet());
Stack stack1 = stream.collect(Collectors.joining()).toString();
```

常用的流操作:
- Intermediate：
map (mapToInt, flatMap 等)、 filter、 distinct、 sorted、 peek、 limit、 skip、 parallel、 sequential、 unordered

- Terminal：
forEach、 forEachOrdered、 toArray、 reduce、 collect、 min、 max、 count、 anyMatch、 allMatch、 noneMatch、 findFirst、 findAny、 iterator

- Short-circuiting：
anyMatch、 allMatch、 noneMatch、 findFirst、 findAny、 limit

---

- map/flatMap :将input Stream中的每一个元素映射成output Stream中的另外一个元素
```
List<String> output = wordList.stream().
map(String::toUpperCase).
collect(Collectors.toList());
```
- filter: 对原始的Stream进行某项测试然后过滤后生下来的生成一个新的Stream流。
forEach: 和Collection的差不多，遍历的功效
- peak:
- findFirst:
等方法
