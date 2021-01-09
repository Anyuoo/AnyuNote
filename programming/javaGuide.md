java
> 引用：强软弱虚
```java
SoftReference<byte[]> sf = new SoftReference(new Byte[1024*1024*10])
    //强引用:sf - new SF 软引用： sf-> new byte 
    //构造器new 对象
```

> 软引用堆内存不够就会回收，适合缓存

> 弱引用 每次GC都会被回收

```java
//弱引用
WeakReference<M> m = new WeakReference(new M())
```

> PhantomReference 虚引用

```java
	ThreadLocal tl = new ThraedLocal();
    public void set(T value) {
        Thread t = Thread.currentThread();
        //当前线程对象作为key，获取ThreadLocalMap
        ThreadLocalMap map = getMap(t);
        if (map != null) {
            map.set(this, value);
        } else {
            createMap(t, value);
        }
    }
  ThreadLocalMap getMap(Thread t) {
        return t.threadLocals;
    }
 void createMap(Thread t, T firstValue) {
     //当前threadLocal对象作为ThreadLocalMap的key
        t.threadLocals = new ThreadLocalMap(this, firstValue);
    }

//ThreadLocalMap 存储值table数组是Entry节点
   ThreadLocalMap(ThreadLocal<?> firstKey, Object firstValue) {
            table = new Entry[INITIAL_CAPACITY];
            int i = firstKey.threadLocalHashCode & (INITIAL_CAPACITY - 1);
            table[i] = new Entry(firstKey, firstValue);
            size = 1;
            setThreshold(INITIAL_CAPACITY);
        }

//继承WeakReference对象
static class Entry extends WeakReference<ThreadLocal<?>> {
    /** The value associated with this ThreadLocal. */
    Object value;
//当前ThreadLocal对象的弱引用作为entry的key
    Entry(ThreadLocal<?> k, Object v) {
        super(k);
        value = v;
    }
}
```

