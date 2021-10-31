# Stream

```java
总结：
# 创建：通过集合、通过数组、通过静态方法of
Stream<Employee> stream = employees.stream();
Stream<Employee> parallelStream = employees.parallelStream();
Arrays.stream(arr);
Stream<Integer> stream = Stream.of(1, 2, 3, 4, 5, 6);


# 使用  
## 中间操作：筛选和切片、映射、排序
filter(Predicate p)——接收 Lambda ， 从流中排除某些元素。
limit(n)——截断流，使其元素不超过给定数量。
skip(n) —— 跳过元素，返回一个扔掉了前 n 个元素的流。若流中元素不足 n 个，则返回一个空流。与 limit(n) 互补
distinct()——筛选，通过流所生成元素的 hashCode() 和 equals() 去除重复元素  
  
map(Function f)——接收一个函数作为参数，将元素转换成其他形式或提取信息，该函数会被应用到每个元素上，并将其映射成一个新的元素。
  
sorted()——自然排序
sorted(Comparator com)——定制排序  
## 终止操作：匹配与查找、规约、收集 
### 匹配与查找 
allMatch(Predicate p)——检查是否匹配所有元素。
anyMatch(Predicate p)——检查是否至少匹配一个元素。
noneMatch(Predicate p)——检查是否没有匹配的元素。
findFirst——返回第一个元素
findAny——返回当前流中的任意元素
count——返回流中元素的总个数
max(Comparator c)——返回流中最大值
min(Comparator c)——返回流中最小值
forEach(Consumer c)——内部迭代  

reduce(T identity, BinaryOperator)——可以将流中元素反复结合起来，得到一个值。返回 T
reduce(BinaryOperator) ——可以将流中元素反复结合起来，得到一个值。返回 Optional<T>
  
collect(Collector c)——将流转换为其他形式。接收一个 Collector接口的实现，用于给Stream中元素做汇总的方法  
  
// 练习：是否所有的员工的年龄都大于18
// 练习：是否存在员工的工资大于 10000 
// 练习：是否存在员工姓“雷”
// findFirst——返回第一个元素
// findAny——返回当前流中的任意元素
  
// 练习1：计算1-10的自然数的和
// 练习2：计算公司所有员工工资的总和
  
// 练习1：查找工资大于6000的员工，结果返回为一个List或Set  
```



## 概述

> **Java 8中有两大最为重要的改变。一个是 Lambda 表达式，一个是 Stream API。**
>
> **Java 8是一个非常成功的版本，这个版本新增的Stream，配合同版本出现的 Lambda ，给我们操作集合（Collection）提供了极大的便利。**

`Stream`将**要处理的元素集合**看作一种**流**，在流的过程中，借助`Stream API`对流中的元素进行操作，比如：筛选、排序、聚合等。



Stream可以**由数组或集合创建**，对流的操作分为两种：

- 中间操作，每次返回一个新的流，可以有多个。
- 终端操作，每个流只能进行一次终端操作，终端操作结束后流无法再次使用。终端操作会产生一个新的集合或值。



**Stream有几个特性：**

- stream不存储数据，而是按照特定的规则对数据进行计算，一般会输出结果。
- stream不会改变数据源，通常情况下会产生一个新的集合或一个值。
- stream具有延迟执行特性，**只有调用终端操作时，中间操作才会执行。**

## 创建

### 通过集合

**通过` java.util.Collection.stream() `方法用集合创建流**

```java
@Test
public void test1(){
    List<Employee> employees = EmployeeData.getEmployees();

    // default Stream<E> stream() : 返回一个顺序流
    Stream<Employee> stream = employees.stream();

    // default Stream<E> parallelStream() : 返回一个并行流
    Stream<Employee> parallelStream = employees.parallelStream();

}


/**
 * 提供用于测试的数据
 */
public class EmployeeData {
	
	public static List<Employee> getEmployees(){
		List<Employee> list = new ArrayList<>();
		
		list.add(new Employee(1001, "马化腾", 34, 6000.38));
		list.add(new Employee(1002, "马云", 12, 9876.12));
		list.add(new Employee(1003, "刘强东", 33, 3000.82));
		list.add(new Employee(1004, "雷军", 26, 7657.37));
		list.add(new Employee(1005, "李彦宏", 65, 5555.32));
		list.add(new Employee(1006, "比尔盖茨", 42, 9500.43));
		list.add(new Employee(1007, "任正非", 26, 4333.32));
		list.add(new Employee(1008, "扎克伯格", 35, 2500.32));
		
		return list;
	}
	
}

public class Employee {

	private int id;
	private String name;
	private int age;
	private double salary;
	
    ......
}
```

### 通过数组

**使用`java.util.Arrays.stream(T[] array)`方法用数组创建流**

```java
@Test
public void test2(){
    int[] arr = new int[]{1,2,3,4,5,6};
    IntStream stream = Arrays.stream(arr);

    Employee e1 = new Employee(1001,"Tom");
    Employee e2 = new Employee(1002,"Jerry");
    Employee[] arr1 = new Employee[]{e1,e2};
    Stream<Employee> stream1 = Arrays.stream(arr1);

}
```

### 通过静态方法

**使用Stream的静态方法：of()、iterate()、generate()**

```java
@Test
public void test3(){

    Stream<Integer> stream = Stream.of(1, 2, 3, 4, 5, 6);

    Stream<Integer> stream2 = Stream.iterate(0, (x) -> x + 3).limit(4);
    stream2.forEach(System.out::println); // 0 2 4 6 8 10

    Stream<Double> stream3 = Stream.generate(Math::random).limit(3);
    stream3.forEach(System.out::println);

}
```



## 使用

### 中间操作

多个中间操作可以连接起来形成一个流水线，除非流水线上触发终止操作，否则中间操作不会执行任何的处理！而在终止操作时一次性全部处理，称为“惰性求值”。

#### 筛选与切片

<img src="https://gitee.com/code0002/blog-img/raw/master/img/image-20211030124237342.png" alt="image-20211030124237342" style="zoom:40%;" />

```java
    /**
     * 筛选与切片
     * <p>
     * filter(Predicate p)——接收 Lambda ， 从流中排除某些元素。
     * limit(n)——截断流，使其元素不超过给定数量。
     * skip(n) —— 跳过元素，返回一个扔掉了前 n 个元素的流。若流中元素不足 n 个，则返回一个空流。与 limit(n) 互补
     * distinct()——筛选，通过流所生成元素的 hashCode() 和 equals() 去除重复元素
     */
@Test
public void test1() {
  List<Employee> employees = EmployeeData.getEmployees();

  // 练习：查询员工表中薪资大于7000的员工信息
  employees.stream().filter(employee -> employee.getSalary() > 7000).forEach(System.out::println);
  System.out.println();

  // 练习：前3个员工的信息
  employees.stream().limit(3).forEach(System.out::println);
  System.out.println();

  // 练习：跳过前3个员工的信息
  employees.stream().skip(3).forEach(System.out::println);
  System.out.println();

  // 练习：去重
  employees.add(new Employee(1010,"刘强东",40,8000));
  employees.add(new Employee(1010,"刘强东",40,8000));
  employees.add(new Employee(1010,"刘强东",42,8000));
  employees.stream().distinct().forEach(System.out::println);

}
```

#### 映射

<img src="https://gitee.com/code0002/blog-img/raw/master/img/image-20211030124323540.png" alt="image-20211030124323540" style="zoom:40%;" />

```java
    /**
     * 映射
     * <p>
     * map(Function f)——接收一个函数作为参数，将元素转换成其他形式或提取信息，该函数会被应用到每个元素上，并将其映射成一个新的元素。
     * flatMap(Function f)——接收一个函数作为参数，将流中的每个值都换成另一个流，然后把所有流连接成一个流。
     */
@Test
public void test2() {
  List<Employee> list = EmployeeData.getEmployees();

  // 练习1：获取员工姓名长度大于3的员工的姓名。----先映射后过滤
  list.stream().map(employee -> employee.getName()).filter(e -> e.length() > 3).forEach(System.out::println);

  // 练习2：
  List<String> list2 = Arrays.asList("aa", "bb", "cc", "dd");
  Stream<Stream<Character>> streamStream = list2.stream().map(StreamAPITest1::fromStringToStream);
  streamStream.forEach(s -> {
    s.forEach(System.out::println);
  });
  System.out.println();

  //
  Stream<Character> characterStream = list2.stream().flatMap(StreamAPITest1::fromStringToStream);
  characterStream.forEach(System.out::println);

}

/**
     * 将字符串中的多个字符构成的集合转换为对应的Stream的实例
     * @param str
     * @return
     */
public static Stream<Character> fromStringToStream(String str){//aa
  ArrayList<Character> list = new ArrayList<>();
  for(Character c : str.toCharArray()){
    list.add(c);
  }
  return list.stream();

}
```

#### 排序

<img src="https://gitee.com/code0002/blog-img/raw/master/img/image-20211030124416039.png" alt="image-20211030124416039" style="zoom:40%;" />

```java
		/**
     * 排序
     * <p>
     * sorted()——自然排序
     * sorted(Comparator com)——定制排序
     */
@Test
public void test3() {
  List<Integer> list = Arrays.asList(12, 78, 150, 410, 60, 70, -1, 9);
  list.stream().sorted().forEach(System.out::println);

  // 抛异常，原因:Employee没有实现Comparable接口
  //List<Employee> employees = EmployeeData.getEmployees();
  //employees.stream().sorted().forEach(System.out::println);

  // 定制排序

  List<Employee> employeeList = EmployeeData.getEmployees();
  employeeList.stream().sorted((e1, e2) -> {
    int i = Integer.compare(e1.getAge(), e2.getAge());
    if (i != 0) {
      return i;
    } else {
      return Double.compare(e1.getSalary(), e2.getSalary());
    }
  }).forEach(System.out::println);

}


-1
9
12
60
70
78
150
410
Employee{id=1002, name='马云', age=12, salary=9876.12}
Employee{id=1007, name='任正非', age=26, salary=4333.32}
Employee{id=1004, name='雷军', age=26, salary=7657.37}
Employee{id=1003, name='刘强东', age=33, salary=3000.82}
Employee{id=1001, name='马化腾', age=34, salary=6000.38}
Employee{id=1008, name='扎克伯格', age=35, salary=2500.32}
Employee{id=1006, name='比尔盖茨', age=42, salary=9500.43}
Employee{id=1005, name='李彦宏', age=65, salary=5555.32}
```



### 终止操作

终端操作会从流的流水线生成结果。其结果可以是任何不是流的值，例如：List、Integer，甚至是 void 。 
流进行了终止操作后，不能再次使用。

#### 匹配与查找

<img src="https://gitee.com/code0002/blog-img/raw/master/img/image-20211030151011129.png" alt="image-20211030151011129" style="zoom:50%;" />

<img src="https://gitee.com/code0002/blog-img/raw/master/img/image-20211030151035839.png" alt="image-20211030151035839" style="zoom:50%;" />

```java
		/**
     * 匹配与查找
     * <p>
     * allMatch(Predicate p)——检查是否匹配所有元素。
     * anyMatch(Predicate p)——检查是否至少匹配一个元素。
     * noneMatch(Predicate p)——检查是否没有匹配的元素。
     * findFirst——返回第一个元素
     * findAny——返回当前流中的任意元素
     * <p>
     * count——返回流中元素的总个数
     * max(Comparator c)——返回流中最大值
     * min(Comparator c)——返回流中最小值
     * forEach(Consumer c)——内部迭代
     */
@Test
public void test1() {
  List<Employee> employees = EmployeeData.getEmployees();

  // 练习：是否所有的员工的年龄都大于18
  boolean allMatch = employees.stream().allMatch(employee -> employee.getAge() > 18);
  // 练习：是否存在员工的工资大于 10000
  boolean anyMatch = employees.stream().anyMatch(employee -> employee.getSalary() > 10000);
  // 练习：是否存在员工姓“雷”
  boolean noneMatch = employees.stream().noneMatch(e -> e.getName().startsWith("雷"));
  // findFirst——返回第一个元素
  Optional<Employee> employee = employees.stream().findFirst();
  // findAny——返回当前流中的任意元素
  Optional<Employee> any = employees.stream().findAny();

  // count——返回工资大于5000多员工总个数
  long count = employees.stream().filter(e -> e.getSalary() > 5000).count();
  // 练习：返回最高的工资
  Stream<Double> salaryStream = employees.stream().map(e -> e.getSalary());
  Optional<Double> maxSalary = salaryStream.max(Double::compareTo);
  // 练习：返回最低工资的员工
  Optional<Employee> minSalary = employees.stream().min((e1, e2) -> Double.compare(e1.getSalary(), e2.getSalary()));
  // forEach(Consumer c)——内部迭代（不做任何中间操作，直接遍历一下）
  employees.stream().forEach(System.out::println);
  // 也可以使用集合的遍历操作
  employees.forEach(System.out::println);

}
```

#### 规约

<img src="https://gitee.com/code0002/blog-img/raw/master/img/image-20211030151115708.png" alt="image-20211030151115708" style="zoom:50%;" />

```java
		/**
     * 规约
     * <p>
     * reduce(T identity, BinaryOperator)——可以将流中元素反复结合起来，得到一个值。返回 T
     * reduce(BinaryOperator) ——可以将流中元素反复结合起来，得到一个值。返回 Optional<T>
     */
@Test
public void test2() {

  // 练习1：计算1-10的自然数的和
  List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
  Integer sum = list.stream().reduce(0, Integer::sum);
  
  // 练习2：计算公司所有员工工资的总和
  List<Employee> employeeList = EmployeeData.getEmployees();
  Stream<Double> salaryStream = employeeList.stream().map(Employee::getSalary);
  //Optional<Double> sumMoney = salaryStream.reduce(Double::sum);
  Optional<Double> sumMoney = salaryStream.reduce((e1, e2) -> e1 + e2);
  System.out.println(sumMoney.get());

}
```

#### 收集

<img src="https://gitee.com/code0002/blog-img/raw/master/img/image-20211030151142221.png" alt="image-20211030151142221" style="zoom:50%;" />

```java
		/**
     * 收集
     * collect(Collector c)——将流转换为其他形式。接收一个 Collector接口的实现，用于给Stream中元素做汇总的方法
     */
@Test
public void test3() {

  // 练习1：查找工资大于6000的员工，结果返回为一个List或Set
  List<Employee> employees = EmployeeData.getEmployees();
  List<Employee> employeeList = employees.stream().filter(employee -> employee.getSalary() > 6000).collect(Collectors.toList());
  employeeList.forEach(System.out::println);

  Set<Employee> employeeSet = employees.stream().filter(employee -> employee.getSalary() > 6000).collect(Collectors.toSet());
  employeeSet.forEach(System.out::println);

}

```



## 工作中的应用

















