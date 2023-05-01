#### HashMap put方法步骤

put方法是通过调用putVal方法进行增加一个元素的。

```java
/**
Associates the specified value with the specified key in this map. If the map previously contained a mapping for the key, the  old value is replaced.
Params:
key – key with which the specified value is to be associated
value – value to be associated with the specified key
Returns:
the previous value associated with key, or null if there was no mapping for key. (A null return can also indicate that the map previously associated null with key.)
*/
public V put(K key, V value) {
    return putVal(hash(key), key, value, false, true);
}
```

putVal 源码

```java
/**
 * Implements Map.put and related methods.
 *
 * @param hash hash for key
 * @param key the key
 * @param value the value to put
 * @param onlyIfAbsent if true, don't change existing value
 * @param evict if false, the table is in creation mode.
 * @return previous value, or null if none
 */
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
               boolean evict) {
    Node<K,V>[] tab; Node<K,V> p; int n, i;       //p是桶的第一个元素
    if ((tab = table) == null || (n = tab.length) == 0)
        n = (tab = resize()).length;
    if ((p = tab[i = (n - 1) & hash]) == null)
        tab[i] = newNode(hash, key, value, null);
    else {
        Node<K,V> e; K k;
        if (p.hash == hash &&
            ((k = p.key) == key || (key != null && key.equals(k))))  //如果p和待插入的元素key相等
            e = p;
        else if (p instanceof TreeNode)   //判断是否是红黑树
            e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
        else {    //是链表，进行插入或者遇到Key值相等的返回
            for (int binCount = 0; ; ++binCount) {
                if ((e = p.next) == null) {
                    p.next = newNode(hash, key, value, null);
                    if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                        treeifyBin(tab, hash);    //如果达到界限则调整为红黑树
                    break;   //成功插入，跳出循环
                }
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    break;   //遇到key值相等也跳出循环
                p = e;
            }
        }
        if (e != null) { // existing mapping for key  //存在已有key值则更新value返回旧值
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

- 如果哈希表为空，则初始化一个哈希表，将元素插入进去
- 如果哈希表不空，根据hash值计算出元素在哈希表中将要存放的桶的位置(实际为一个整数索引)，如果桶为空，则将该元素放入该桶中
- 当桶不空时，如果桶的第一个元素和要放入的元素key相等，更新Value，返回旧值。
- 如果第一个元素和要放入的元素key值不等，则根据第一个元素的类型判断这个桶是不是红黑树，是则执行红黑树的添加方法
- 不是红黑树(即是链表)，则遍历该链表，在链表末尾插入元素，如果遍历发现有元素key值等于待插入元素，则不插入仅更新value。
- 链表形式成功添加后，如果桶中元素超过界限，则该桶转化为红黑树。
- 添加后如果整个哈希表元素个数超出界限，则重哈希。

以上便是hashmap put方法的流程了。

细看源码可以发现以下事实：

**由hash值得到哈希表桶的位置关系式为 index i = (n-1) & hash  ,n = tab.length**

从位级表示来看n-1 是全1。因此（n-1）& hash 等价于将hash值超过w位（2^w =n）高位部分直接丢弃。剩下的低于w位表示的整数就是索引值。

