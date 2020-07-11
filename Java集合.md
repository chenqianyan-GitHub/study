# 1.ArrayList
ArrayList是一个有序可重复的集合，其底层是一个Object[] elementData数组。其数组的默认大小是10，每当该数组超出容量时，会自动扩大到原来的1.5倍。
ArrayList集合在执行add()方法时，需要先调用ensureCapacityInternal()方法检查add之后数组是否会超出容量，如果超出，就扩容到原来的1.5倍。对于需要插入的对象，还会调用System.arraycopy()方法
对整个数组进行移动。对于remove()方法，如果传入的是下标，会先检查下标是否越界，然后调用System.arraycopy()方法，如果传入的是对象，如果传入的是null，就会用==来遍历数组，并移除第一个null函数，
否则就调用equal()方法来遍历删除。

# 2.LinkedList
LinkedList是一个有序可重复的集合，其底层是一个双向链表。其中储存有链表的size，以及first节点和last节点，每个节点包含item值，以及对前后两个节点的指向。
add()时，创建一个Node，其前节点指向last，然后判断尾节点是否为空，为空则表示add的是第一个元素，将头节点赋值给Node，并更新last节点。

保证线程安全可以用copyOnWriteArrayList和copyOnWriteSet，其基本原理是当要向容器中添加元素时，会复制一个新容器，往新容器中添加然后将引用指向新容器。
这样可以保证其在读操作时不用加锁，在写操作时才加锁。但是它只能保证数据的最终一致性而无法保证实时一致性，并且可能导致内存占用过大。

# 3.HashSet
无序不可重复的集合，其底层原理参考HashMap。

# 4.LinkedHashSet
有序不可重复的集合，在HashSet的基础上，每个Node额外有了previous和next构成一个双向链表。

# 5.TreeSet
无序不可重复，继承自SortSet，元素按照重写的compare方法进行排序，底层是一个红黑树。
put()：先判断是否为空，然后调用比较器，用比较器的compare方法比较，找到插入的位置或更新节点的value。

# 6.HashMap
底层是一个链表数组，传入的键值对以Node类储存（Node类中有hash、key、value、Node.next），Node类继承了Map.Entry<key,value>。
储存一个键值对时，调用HashCode方法得到key的Hash值，插入数组相关位置，查找时先用HashCode查位置，再用equals方法比较链表中Entry对象中储存的key值。HashMap允许一个key为null，多个value为null。
哈希桶是使用一个顺序表来存放具有相同哈希值的key的链表的头节点，利用这个头节点可以找到其它的key。
Hash桶的数量默认是16个，当链表数组超过初始容量的0.75时，会将链表数组扩容两倍，原hash桶的索引要么不变，要么最高位置1，然后把原链表数组的搬移到新的数组中。
在JDK1.8中，当一个链表的长度超过8时，并且hash桶的大小大于64时（如果桶的数量小于64，说明桶内元素过于拥挤，HashMap会直接扩容），链表会转换成红黑树，
当红黑树的节点数小于6时，会重新转化为一个单链表。

HashMap在执行并发操作时，如果AB线程同时在同一位置执行put操作，两者同时得到该位置的头节点，然后分别更新，就会出现数据的覆盖。
HashMap在执行并发操作时还可能会引起死循环，是因为当一个线程执行put操作，链表的next还未指向时，另一个线程put操作导致数组扩容调用resize()方法，链表中Node顺序打乱，
此时一线程执行next可能导致HashMap的Entry链表形成环形数据结构。

# 7.ConcurrentHashMap
ConCurrentHashMap是HashMap在并发情况下的替代品。
在执行查询的操作时，JDK1.7中首先会通过两次Hash找到segment的位置，然后通过一次hash找到key的位置。
而JDK1.8中将分段锁Segment改为锁每一个链表的第一个Node，put操作时，若当前位置为空，则利用 CAS（比较和交换（Conmpare And Swap）是用于实现多线程同步的原子指令） 尝试写入，
失败则自旋保证成功。然后检查是否需要扩容，若不为空则利用 synchronized 锁写入数据，然后检查是否需要转化为红黑树。

