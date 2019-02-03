#LinkedHashMap

## 概述
LinkedHashMap继承于HashMap，存取过程与HashMap类似，但在实现细节上稍有不同。这是由LinkedHashMap自身特性决定的，因为它额外维护了一个双向链表用于保持迭代顺序。准确的说，LinkedHashMap是一个将所有所有Entry节点存入一个双向链表的HashMap。

双向链表保持的迭代顺序，可以是插入顺序，也可以是访问顺序。根据链表中元素的迭代顺序可以分为保持插入顺序的LinkedHashMap和保持访问顺序的LinkedHashMap。LinkedHashMap的默认实现是按插入顺序排序的。

## HashMap与LinkedHashMap的结构示意图

![](https://i.imgur.com/qIkkwSP.jpg)

## 类结构定义

	public class LinkedHashMap<K,V> extends HashMap<K,V> implements Map<K,V>{

	...
	}