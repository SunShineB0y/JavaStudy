## LinkedHashMap

### 概述
LinkedHashMap继承于HashMap，存取过程与HashMap类似，但在实现细节上稍有不同。这是由LinkedHashMap自身特性决定的，因为它额外维护了一个双向链表用于保持迭代顺序。准确的说，LinkedHashMap是一个将所有所有Entry节点存入一个双向链表的HashMap。

双向链表保持的迭代顺序，可以是插入顺序，也可以是访问顺序。根据链表中元素的迭代顺序可以分为保持插入顺序的LinkedHashMap和保持访问顺序的LinkedHashMap。LinkedHashMap的默认实现是按插入顺序排序的。

### HashMap与LinkedHashMap的结构示意图

![](https://i.imgur.com/qIkkwSP.jpg)

LinkedHashMap重新定义了Entry,即增加了before和after两个指针，用于维护双向链表，而next指针仍然用于维护map中同一位置的Entry所连接成的链表。


在LinkedHashMapMap中，所有put进来的Entry都保存在HashMap中，但由于它又额外定义了一个以head为头结点的空的双向链表，因此对于每次put进来Entry还会将其插入到双向链表的尾部。

### 类结构定义

	public class LinkedHashMap<K,V> extends HashMap<K,V> implements Map<K,V>{

	...
	}


### 成员变量定义
LinkedHashMap中新增了两个成员变量用于保证迭代顺序，分别是双向链表头结点header和标志位accessOrder。（accessOrder值为True时，表示按访问顺序迭代，为False时，表示按照插入顺序迭代）

	/**
	 * The head of the doubly linked list.
 	 */
	private transient Entry<K,V> header; // 双向链表的表头元素

	/**
 	* The iteration ordering method for this linked hash map: <tt>true</tt>
 	* for access-order, <tt>false</tt> for insertion-order.
 	*
 	* @serial
 	*/
	private final boolean accessOrder;//true表示按照访问顺序迭代，false时表示按照插入顺序

### LinkedHashMap 的扩容操作 : resize()
LinkedHashMap和HashMap一样，当存储的Entry达到扩容临界值的时候会进行扩容，因为LinkedHashMap本来就是一个HashMap，只是它还将所有Entry节点链入到了一个双向链表中。LinkedHashMap的扩容比HashMap来的方便，因为HashMapp需要将原来的每个链表的元素分别在新数组进行反向插入链化，而LinkedHashMap的元素都连在一个链表上，可以直接迭代然后插入。
