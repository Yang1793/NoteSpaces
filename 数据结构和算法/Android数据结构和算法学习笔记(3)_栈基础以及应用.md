# 栈

## 栈的概念
<p>栈是限定仅在表尾进行插入和删除操作的线性表。</p>
<p>允许插入和删除的一端称为栈顶（top），另一端称为栈底（bottom）,不含任何数据元素的栈称为空栈。栈又称为后进先出的线性表</p>

<![](https://github.com/Yang1793/NoteSpaces/blob/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E5%92%8C%E7%AE%97%E6%B3%95/picture/%E6%A0%88.png?raw=true)

## 栈的实现
### 顺序实现
<p>类似数组的实现，插入元素则直接在栈顶（top）的后一位直接插入，如果栈顶（top）大于数组的大小（size），则也如同数组一样先增长数组，再增加。</p>
<p>Java中有一个类叫做 **Stack.java** ，它就是一个典型的顺序实现的栈。</p>

### 链式实现
<p>链式实现就和单链表相同。</p>
<p>插入元素</p>

```
    Node node = new Node();
    node.data = 插入的元素；
    node.next = stack.top;
    stack.top = s;
    stack.size++;
```

<p>移除元素</p>

```
    Node node = stack.top;
    stack.top = node.next;
    node.data = null;
    stack.size--;
```
