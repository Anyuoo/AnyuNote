# 1.0 Java

## 1.1 java SE

1. java 9 不推荐使用`newInstance() `推荐使用`clazz.getDeclaredConstructor().newInstance()`
2. 重写`equals`方法，必须重写`hashcode`方法，equals 先判断值相同在判断地址是否相同；hashcode相同，对象相等，对象相等hashcode不一定相同。
3. jdk1.8及以前String使用的是`char`数组，jdk1.9及以后使用的是`byte`数组，被final修饰。
4. 方法的签名包含方法名、方法的参数，不包括方法返回值；所以重载不能已返回值作为判定条件。
5. object对象中的方法
   - clone 创建并返回此对象的一个副本
   - hashCode()返回该对象的哈希码值
   - finalize()当垃圾回收器确定不存在对该对象的更多引用时，由对象的垃圾回收器调用此方法
   - getClass()返回一个对象的运行时类
   - notify()唤醒在此对象监视器上等待的单个线程。
   -  notifyAll()唤醒在此对象监视器上等待的所有线程。
   - wait(long timeout)导致当前的线程等待，直到其他线程调用此对象的 notify() 方法或 notifyAll() 方法，或者超过指定的时间量。

# 2.0 数据结构

## 2.1 

1. 布隆过滤器

   >布隆过滤器是一种数据结构，比较巧妙的概率型数据结构（probabilistic data structure），特点是高效地插入和查询，可以用来告诉你 “某样东西一定不存在或者可能存在”。相比于传统的 List、Set、Map 等数据结构，它更高效、占用空间更少，但是缺点是其返回的结果是概率性的，而不是确切的。

   **核心思想**就是利用多个不同的Hash函数来解决“冲突”。
   Hash存在一个冲突（碰撞）的问题，用同一个Hash得到的两个URL的值有可能相同。为了减少冲突，我们可以多引入几个Hash，如果通过其中的一个Hash值我们得出某元素不在集合中，那么该元素肯定不在集合中。只有在所有的Hash函数告诉我们该元素在集合中时，才能确定该元素存在于集合中。这便是Bloom-Filter的基本思想。
   Bloom-Filter一般用于在大数据量的集合中判定某元素是否存在。