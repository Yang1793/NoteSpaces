# 线性表
## 1 概念
<p>线性表（linear list）是数据结构的一种，一个线性表是n个具有相同特性的数据元素的有限序列。数据元素是一个抽象的符号，其具体含义在不同的情况下一般不同。</p>

## 2 特点
1. 集合中必存在唯一的一个“第一元素”。
2. 集合中必存在唯一的一个 “最后元素” 。
3. 除最后一个元素之外，均有唯一的后继(后件)。
4. 除第一个元素之外，均有唯一的前驱(前件)。

## 3 分类
<p>线性表按照存储结构可以分为“顺序存储结构”和“链式存储结构”。</p>

## 4 顺序存储结构
### 4.1 顺序表
<p>顺序表是指用一组地址连续的存储单元依次存储数据元素的线性结构。</p>
<p>线性表采用顺序存储的方式存储就称之为顺序表，顺序表是将表中的结点依次存放在计算机内存中一组地址连续的存储单元中。线性表采用指针链接的方式存储就称之为链表。</p>

<img src="https://github.com/Yang1793/NoteSpaces/blob/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E5%92%8C%E7%AE%97%E6%B3%95/picture/%E9%A1%BA%E5%BA%8F%E8%A1%A8.png?raw=true" />

<p>a1是a2的前驱，ai+1 是ai的后继，a1没有前驱，an没有后继
n为线性表的长度 ，若n==0时，线性表为空表</p>

#### 4.1.1 优缺点
<p>**优点**：尾插效率高，支持随机访问。</br>
**缺点**：中间插入或者删除效率低。</br>
**应用**：数组、ArrayList</p>

#### 4.1.2 插入逻辑
<p>将需要插入的坐标对应的数据以及该数据之后的数据向后移动，再将数据插入空出的坐标。</p>

<img src="https://github.com/Yang1793/NoteSpaces/blob/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E5%92%8C%E7%AE%97%E6%B3%95/picture/%E9%A1%BA%E5%BA%8F%E8%A1%A8%E6%8F%92%E5%85%A5.png?raw=true" />

<p>当然 数组 或者 ArrayList 都是有初始容量的，比如ArrayList的默认容量为 10, 当容量不够用时，会先增长数组，ArrayList中是以1.5倍增长。（可能java版本不同，倍数不同）</p>


```
public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable
{
    ....

    private static final int DEFAULT_CAPACITY = 10;

    ....

    /**
     * 增加容量，以确保它至少可以容纳
     * 最小容量参数指定的元素数量。
     *
     * @param minCapacity the desired minimum capacity
     */
    private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        // minCapacity is usually close to size, so this is a win:
        elementData = Arrays.copyOf(elementData, newCapacity);
    }

    private static int hugeCapacity(int minCapacity) {
        if (minCapacity < 0) // overflow
            throw new OutOfMemoryError();
        return (minCapacity > MAX_ARRAY_SIZE) ?
            Integer.MAX_VALUE :
            MAX_ARRAY_SIZE;
    }
}
```

#### 4.1.3 删除逻辑

<p>将需要删除数据移除表，再将该数据后的数据向前移动位置。</p>

```
图片没有上传  先占位子
```

### 4.2 链表

<p>链表 分为 单链表，单链循环表，双链表，双链循环表。</p>
<p>单链表是一种链式存取的数据结构。</br>
单链表中的数据已节点来表示，每一个节点分为 **数据** 和 **指针**。指针会指向该节点的下一个数据。下面写了一段代码描述</p>

<p>单链循环表则是在单链表的基础上将尾节点的指针指向头节点。（目前没有遇到过用这个）</p>
<p>双链表也是已节点来表示，但是每一个节点分为 **数据** 和 **两个指针**，前指针指向当前节点的前一个节点，后指针指向当前节点的后一个节点。LinkedList就是典型的双链表</p>
<p>双链循环表则也是同理，将尾节点的后指针指向头节点，头节点的前指针指向尾节点。</p>

```
/**
 * Author:JR
 * E-mail:jianr.yang@sunyard.com
 * Time: 2018/10/29
 * Description: 单链表
 */
public class MyLinkedList<T> {

    private NODE<T> frist;

    private int size;

    public int getSize(){
        return size;
    }

    public void add(T t){
        NODE<T> node = new NODE<>(t, null);
        if (frist == null) {
            frist = node;
        } else {
            NODE<T> target;
            target = frist;
            for ( ; ; ) {
                if (target.next == null) {
                    break;
                }else{
                    target = target.next;
                }
            }
            target.next = node;
        }
        size++;
    }

    public void add(int position, T t){

        if (position > size) {
            return;
        }

        if (position == size) {
            add(t);
        }else if (position == 0) {
            NODE<T> node = new NODE<>(t, frist);
            frist = node;
            size++;
        } else {
            NODE<T> target;
            target = frist;
            for (int i = 0; i < position-1; i++) {
                target = target.next;
            }
            NODE<T> next = target.next;
            NODE<T> node = new NODE<>(t, next);
            target.next = node;
            size++;
        }

    }

    public T get(int position) {

        if (position<0 ||position >= size) {
            return null;
        }
        NODE<T> target;
        target = frist;
        for (int i = 0; i < position; i++) {
            target = target.next;
        }
        return target.getT();
    }

    public T remove(){
        if (size == 0) {
            return null;
        }
        NODE<T> target;
        target = frist;
        frist = target.next;
        target.next = null;
        size--;
        return target.getT();
    }

    public T remove(int position){
        if (position < 0 || position >= size) {
            return null;
        }
        if (size == 0) {
            return null;
        }
        if (position == 0) {
            return remove();
        }else{
            NODE<T> target;
            target = frist;
            for (int i = 0; i < position - 1; i++) {
                target = target.next;
            }
            NODE<T> next = target.next;
            target.next = next.next;
            next.next = null;
            size--;
            return next.getT();
        }
    }

    class NODE<T>{

        private T t;

        private NODE<T> next;

        public NODE(T t, NODE<T> next) {
            this.t = t;
            this.next = next;
        }

        public T getT() {
            return t;
        }

        public void setT(T t) {
            this.t = t;
        }

        public NODE<T> getNext() {
            return next;
        }

        public void setNext(NODE<T> next) {
            this.next = next;
        }
    }
}
```
