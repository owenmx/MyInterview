<img src="../../\LeetCode刷题\images\PRZETcraZV7dbK4E5zhKziFhtoZd3qmdYoXVn5w2BtuNlmBthGqQ-t6bkslUFwUqup2ey0f6tZiifCXaN5OMFr2sSyUoLTqTuRSTIug735po3buZnsa8XPmSlRw" alt="Overview of Java ArrayList, HashTable, HashMap, Hashet,LinkedList" style="zoom:150%;" />

## Collections

#### 数组的特点与缺点

+   一旦数组被初始化后，长度就确定了，无法再进行修改。
+   数组一旦定义好，元素类型就确定了。
+   数组存储的确定：有序，可重复；对于其他数据类型不支持。

#### collections架构

+   List接口，有序、可重复的集合
+   Set接口，无序、不可重复的集合

#### collection元素遍历

+   迭代器遍历

```java
Iterator it = coll.iterator();
while(it.hasNext()) {
    System.out.println(it.next());
}
```

+   for-each循环遍历

```java
for(Object obj:coll) {
    System.out.println(obj);
}
```

#### ArrayList 常用接口操作

|                             方法                             |                     描述                      |
| :----------------------------------------------------------: | :-------------------------------------------: |
| [add()](https://www.runoob.com/java/java-arraylist-add.html) |      将元素插入到指定位置的 arraylist 中      |
| [addAll()](https://www.runoob.com/java/java-arraylist-addall.html) |      添加集合中的所有元素到 arraylist 中      |
| [clear()](https://www.runoob.com/java/java-arraylist-clear.html) |          删除 arraylist 中的所有元素          |
| [clone()](https://www.runoob.com/java/java-arraylist-clone.html) |              复制一份 arraylist               |
| [contains()](https://www.runoob.com/java/java-arraylist-contains.html) |           判断元素是否在 arraylist            |
| [get()](https://www.runoob.com/java/java-arraylist-get.html) |       通过索引值获取 arraylist 中的元素       |
| [indexOf()](https://www.runoob.com/java/java-arraylist-indexof.html) |         返回 arraylist 中元素的索引值         |
| [removeAll()](https://www.runoob.com/java/java-arraylist-removeall.html) | 删除存在于指定集合中的 arraylist 里的所有元素 |
| [remove()](https://www.runoob.com/java/java-arraylist-remove.html) |          删除 arraylist 里的单个元素          |
| [size()](https://www.runoob.com/java/java-arraylist-size.html) |           返回 arraylist 里元素数量           |
| [isEmpty()](https://www.runoob.com/java/java-arraylist-isempty.html) |            判断 arraylist 是否为空            |
| [subList()](https://www.runoob.com/java/java-arraylist-sublist.html) |           截取部分 arraylist 的元素           |
| [set()](https://www.runoob.com/java/java-arraylist-set.html) |        替换 arraylist 中指定索引的元素        |
| [sort()](https://www.runoob.com/java/java-arraylist-sort.html) |           对 arraylist 元素进行排序           |
| [toArray()](https://www.runoob.com/java/java-arraylist-toarray.html) |            将 arraylist 转换为数组            |
| [toString()](https://www.runoob.com/java/java-arraylist-tostring.html) |           将 arraylist 转换为字符串           |
| [ensureCapacity](https://www.runoob.com/java/java-arraylist-surecapacity.html)() |         设置指定容量大小的 arraylist          |
| [lastIndexOf()](https://www.runoob.com/java/java-arraylist-lastindexof.html) | 返回指定元素在 arraylist 中最后一次出现的位置 |
| [retainAll()](https://www.runoob.com/java/java-arraylist-retainall.html) | 保留 arraylist 中在指定集合中也存在的那些元素 |
| [containsAll()](https://www.runoob.com/java/java-arraylist-containsall.html) |  查看 arraylist 是否包含指定集合中的所有元素  |
| [trimToSize()](https://www.runoob.com/java/java-arraylist-trimtosize.html) |  将 arraylist 中的容量调整为数组中的元素个数  |
| [removeRange()](https://www.runoob.com/java/java-arraylist-removerange.html) |    删除 arraylist 中指定索引之间存在的元素    |
| [replaceAll()](https://www.runoob.com/java/java-arraylist-replaceall.html) |    将给定的操作内容替换掉数组中每一个元素     |
| [removeIf()](https://www.runoob.com/java/java-arraylist-removeif.html) |     删除所有满足特定条件的 arraylist 元素     |
| [forEach()](https://www.runoob.com/java/java-arraylist-foreach.html) |   遍历 arraylist 中每一个元素并执行特定操作   |

## List 接口

list接口通常作为数组的替换。

#### ArrayList, LinkedList,Vector三者的异同

+   同：三者都实现了List接口，存储数据的特点相同。
+   不同：
    +   ArrayList 是作为List接口的主要实现类，线程不安全的，效率高。底层使用Object[]存储。
    +   LinkedList 底层利用双向链表实现，插入和删快，查找慢。
    +   Vector是作为List接口的古老实现，线程安全的，效率低



#### ArrayList

+   源码分析：扩容机制
    +   JDK 7.0中，在无参构造的过程中就会初始化为10
    +   JDK 8.0中，先初始化为`{}`，在进行add的时候才会初始化。



#### LinkedList

+   源码分析，双向链表实现



#### 常用操作

+   增 add(Object obj)

+   删 remove(int index) / remove(Object obj)，在ArrayList和LinkedList都是删除索引，可以new Integer(2)来实现删除元素。

+   改 set(int index, Object obj)

+   查 get(int index)

+   插 add(int index,Object obj)

+   长度 size()

+   遍历

    ```java
    // 迭代器
    Iterator it = li.iterator();
    while (it.hasnext()) {
        it.next();
    }
    
    // foreach
    for(Object val: li) {
        System.out.printlin(val);
    }
    
    // 普通迭代
    for(int i = 0; i < li.size(); i++) {
        System.out.printlin(li.get(i));
    }
    ```

## Set 接口

#### HashSet，LinkedHashSet，TreeSet

+   HashSet是Set集合的主要实现类，线程不安全
    +   LinkedHashSet 作为hashset的子类，遍历其内部数据时候，可以按照添加的顺序遍历
+   TreeSet 采用红黑树存储，可以进行排序，判断是否相同的标准利用CompareTo比较

#### 理解无序性和不可重复性

**在底层按照数组存储。**    

+   无序性：不等于随机性。存储的数据在底层数组中并非按照数组索引的顺序进行添加，而是根据数据的hash值决定
+   不可重复性：保证添加的元素按照equals判断时，不能返回true。也就是相同的元素不能添加进来。

## List 和 Set的比较

#### 重写equals方法

在List中，自定义对象需要重写equals方法，因为我们需要判断两个对象是否相同，来实现remove。

在Set中，自定义对象需要重写equals方法和hashCode方法，因为我们需要确保元素的不可重复性（TreeSet例外，通过CompareTo比较）。

## Map接口

#### 关键定义数据

+   DEFAULT_INITIAL_CAPACITY：默认容量
+   DEFAULT_LOAD_FACTOR：默认填充因子
+   threshold：扩容的临界值=容量*填充因子
+   TREEIFY_THRESHOLD：jdk8中链表长度大于该默认值，转化为红黑树
+   MIN_TREEIFY_CAPACITY：jdk8中数组容量长度大于该默认值，转化为红黑树

#### Map常用方法

+   增 map.put(key,val)

+   删 map.remove(key)

+   改 map.put(key,val)

+   查 map.containsKey(key) / map.containsValue(val)

+   长度 map.size()

+   遍历：

    ```java
    // 1. 通过key来访问map数据
    for (Object key: data.keySet()) {
        System.out.println(data.get(key));
    }
    
    // 2. 通过Entry访问数据
    for (Map.Entry ent:data.entrySet()) {
        System.out.println(ent);
    }
    
    // 3. 直接访问values数据
    for (Object val:data.values()) {
        System.out.println(val);
    }
    ```

#### HashMap

+   可以存放null值，更加稳健

+   底层：
    +   JDK 7及之前，数组+链表
    +   JDK 8之后，数组+链表+红黑树

+   在JDK 7中，在实例化之后，底层创建了长度是16的一维数组Entry[] table数组。
    +   map.put(key1,value1)
        +   首先，调用key1所在类的hashCode计算key1哈希值，此哈希值经过某种算法计算以后，得到Entry数组中的存放位置。
            +   如果此位置上的数据为空，此时key1-value1添加成功
            +   如果此位置上的数据不为空（此位置上已经存在一个或者多个数据），然后依次与链表上的hash值进行比较
                +   如果hash值不同，则添加成功
                +   如果hash值相同，调用equals
                    +   如果返回false，添加成功
                    +   如果返回true，使用value1替换相同key的value值。
    +   在不断添加过程中，会涉及到扩容问题，默认的扩容方式，扩容为原来容量的2倍
+   JDK 8 的不同
    +   new HashMap的时候底层不会存放一个长度为16的数组
    +   首次调用put方法的时候，底层创建长度为16的数组
    +   jdk7只有数组+链表，jdk中底层结构：数组+链表+红黑树（当某一个索引位置上的元素以链表形式存在的数据个数>8，且当前数组的长度>64时候，索引位置上所有的数据改为使用红黑树存储）

+   面试题
    +   HashMap的底层实现原理

+   JDK 8

    +   创建一个 `transient Node<K,V>[] table;`
    +   putVal

    ```java
    final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                       boolean evict) {
            Node<K,V>[] tab; // 新建一个数组
            Node<K,V> p; int n, i;
            if ((tab = table) == null ||  // 判断table是否为空，如果为空，则表明第一次put
                (n = tab.length) == 0) 
                n = (tab = resize()).length; // 调用resize创建默认大小为16的table
            /* (n - 1) & hash 找到当前存放的位置 */
            if ((p = tab[i = (n - 1) & hash]) == null) // 为null，直接添加
                tab[i] = newNode(hash, key, value, null); 
            else { // 不为null
                Node<K,V> e; K k;
                if (p.hash == hash &&
                    ((k = p.key) == key || (key != null && key.equals(k)))) // 第一个hash值相等
                    e = p; // 获取当前p值
                else if (p instanceof TreeNode)
                    e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
                else {
                    for (int binCount = 0; ; ++binCount) {
                        if ((e = p.next) == null) { // 说明只有一个元素
                            p.next = newNode(hash, key, value, null); // 链接元素
                            // 下面是判断是否变成红黑树，TREEIFY_THRESHOLD=8
                            if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                                treeifyBin(tab, hash);
                            break;
                        }
                        if (e.hash == hash &&
                            ((k = e.key) == key || (key != null && key.equals(k))))
                            break;
                        p = e;
                    }
                }
                if (e != null) { // existing mapping for key
                    V oldValue = e.value;
                    if (!onlyIfAbsent || oldValue == null)
                        e.value = value;
                    afterNodeAccess(e);
                    return oldValue;
                }
            }
            ++modCount;
            if (++size > threshold)
                resize();
            afterNodeInsertion(evict);
            return null;
        }
    ```

    

#### LinkedHashMap

![这里写图片描述](../../LeetCode刷题/images/20170512160734275)

+   底层代码

    ```java
        static class Entry<K,V> extends HashMap.Node<K,V> {
            Entry<K,V> before, after; // 定义了两个指针，用来指向前一个元素和后一个元素
            Entry(int hash, K key, V value, Node<K,V> next) { 
                super(hash, key, value, next);
            }
        }
    
        Node<K,V> newNode(int hash, K key, V value, Node<K,V> e) {
            LinkedHashMap.Entry<K,V> p =
                new LinkedHashMap.Entry<K,V>(hash, key, value, e);
            linkNodeLast(p);
            return p;
        }
    ```

#### TreeMap

底层使用红黑树，按照key进行排序，要求key必须是由同一个类创建的对象。



## Collections 工具类

#### 常用方法

+   reverse(List) 反转List中元素的顺序

+   shuffle(List) 对List集合元素进行随机排序

+   sort(List) 根据元素的自然顺序对list集合按照升序排序

+   sort(List,Comparator)根据指定list集合中的i处元素和j处元素进行交换

+   swap将list集合中i处元素和j处元素进行交换

+   Comparable和Comparator的区别：Comparable是一个接口，在自定义类的时候实现；Comparator通常用于定义在sort方法参数中，用于实现精确排序。

    +   1、在当前类中 实现 Comparable 接口，重写其中的 compareTo() 方法，在方法里面指定比较方式。
    +   2、在调用 sort() 方法的时候，在第二个参数的位置，new Comparor 对象，然后重写 compare 方法。

+   Copy 在进行拷贝时的操作

    ```java
    // 
    List dest = Arrays.asList(new Integer[list.size()]);
    Collections.copy(dest,data);
    
    // 或者这样实现拷贝
    System.out.println(new ArrayList(Arrays.asList(data.toArray())));
    ```

    

## 解决hash冲突的方法

一共包含四种方法



#### 1. 开放地址法

就是如果当前发生冲突，就去寻找下一个，再冲突，再寻找下一个。

+ 线性探测
+ 平方探测



#### 2. 再哈希法

准备多个哈希函数，如果发生哈希冲突，就换一个哈希函数。



#### 3. 链地址法

如果发生哈希冲突，用链表将他们串起来



#### 4.建立一个公共溢出区

假设哈希函数的值域为[0,m-1],则设向量HashTable[0..m-1]为基本表，另外设立存储空间向量OverTable[0..v]用以存储发生冲突的记录。



## HahMap的扩容机制

如果不是第一次扩容，则`新容量=旧容量x2`，`新阈值=新容量x负载因子`

