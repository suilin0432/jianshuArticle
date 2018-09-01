Predicate 文档解释:Determines if the input object matches some criteria.
predicate方法中有
```
boolean test(T t);//用来判断是否符合实现后的predicate的函数
default Predicate<T> and(Predicate<? super T> other);//返回  (t)->test(t) && other.test(t) 即两个Predicate的test是否都满足
default Predicate<T> negate();//否定调用，返回(t)->!test(t);
default Predicate<T> or(Predicate<? super T> other);//与and类似
static <T> Predicate<T> isEqual(Object targetRef);//返回 (null == targetRef) ? Objects::isNull : object -> targetRef.equals(object); 就是返回
```
Consumer 文档解释:An operation which accepts a single input argument and returns no result. Unlike most other functional interfaces, Consumer is expected to operate via side-effects. 接受单个参数并且不反悔结果的操作，与大多数其他功能接口不同，消费者要通过副作用进行操作
```
void accept(T t); //接受参数
default Consumer<T> andThen(Consumer<? super T> after); //处理后继续处理
```

---

总结一下
Consumer 会对传入的对象进行一定的操作，然后利用对其操作产生的副作用进行对象的更改，其本质就是一个可以andthen的匿名方法

Predicate 则主要是用来判断一个条件是否满足条件


---

JAVA8 方法引用
构造器引用:
```
@FunctionalInterface
public interface Supplier<T>{
    T get();
}

class Car{
  //Supplier是JAVA 8的街扩，和lambda同时使用
//PS:下面的方法等同于
//Supplier<Test> supplier = Test:new;
//return test.get();
  public static Car create(final Supplier<Car> supplier){
    return supplier.get();
}
  public static void collide(final Car car){
    System.out.println("Collided "+car.toString());
}
  public void follow(final Car another){
    System.out.println("Following the "+another.toString());
}
  public void repair(){
    System.out.println("Repaired" + this.toString());
}
}

//构造器引用: 语法: class::new 或者Class<T>:: new (PS:不要有参数...)
//eg:
final Car car = car.create(Car::new);
final List<Car> cars = Arrays.asList(car);

//静态方法引用: 语法: class::static_method
//eg:
cars.forEach(Car::collide);//将Car作为参数传入
//特定类的任意兑现的方法的引用:语法: Class:method
//eg:
cars.forEach(Car::repair);
//特定对象的方法引用: 语法: instance::method
//eg:
final Car police = Car.create(Car::new);
cars.forEach(police::follow);
```
PS: 使用Supplier 之后可以使用Consumer进行初始化或者更改什么的。
