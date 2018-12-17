# 二叉树的增删查遍历

## 二叉树概念

或者是一颗空树，或者是一颗具有如下性质的树：
1. 若左子树不为空，那么左子树上面的所有节点的关键字值都比根节点的关键字值小
2. 若右子树不为空，那么右子树上面的所有节点的关键字值都比根节点的关键字值大
3. 左右子树都为二叉树
4. 没有重复值（这一点在实际中可以忽略）

## 增加

通过比较需要加入节点的关键值 和 当前节点的关键值，小于则查看当前节点是否有右子坐节点，有则继续比较，无则增加节点到当前节点的右子坐节点；大于则查看当前节点是否有左子坐节点，有则继续比较，无则增加节点到当前节点的左子坐节点。
```
/**
     * Time: 2018/12/12 14:20
     * Description: 增加
     * @return <>true</> 成功
     *         <>false</> 失败
     */
    public boolean add(T t){

        if (root == null) {
            root = new Node<T>(t);
            return true;
        }

        Node<T> current = root;
        while (current != null) {
            if (Comparator(t, current.data) == -1) {
                if (current.leftChild == null) {
                    Node<T> newNode = new Node<T>(null, null, current, t);
                    current.leftChild = newNode;
                    return true;
                }else{
                    current = current.leftChild;
                }
            }else if (Comparator(t, current.data) == 1){
                if (current.rightChild == null) {
                    Node<T> newNode = new Node<T>(null, null, current, t);
                    current.rightChild = newNode;
                    return true;
                }else{
                    current = current.rightChild;
                }
            }else {
                current = null;
            }
        }
        return false;
    }

```

## 查询
通过关键值比较找到需求的值。
```
/**
    * Time: 2018/12/12 14:20
    * Description: 查找
    * @return <>true</> 存在
    *         <>false</> 不存在
    */
   public boolean query(T t){
       if (root == null) {
           return false;
       }
       Node<T> current = root;
       while (current != null) {
           if (Comparator(t, current.getData()) == 0) {
               return true;
           }else if (Comparator(t, current.getData()) == -1) {
               current = current.leftChild;
           }else{
               current = current.rightChild;
           }
       }
       return false;
   }

```
```
   /**
    * Time: 2018/12/12 14:20
    * Description: 查找2
    * @return 节点
    */
   public Node<T> query2(T t){
       if (root == null) {
           return null;
       }

       Node<T> current = root;
       while (current != null) {
           if (Comparator(t, current.getData()) == 0) {
               return current;
           }else if (Comparator(t, current.getData()) == -1) {
               current = current.leftChild;
           }else{
               current = current.rightChild;
           }
       }
       return null;
   }
```
## 删除
删除则相对比较复杂，分为四种情况。
1. 无子节点（叶子节点）；
2. 有左子节点无右子节点；
3. 无左子节点有右子节点；
4. 有左子节点有右子节点。

#### 无子节点（叶子节点）

如果是根节点，就直接赋null；如果不是就断开被删除节点和其父节点的连接。
```
//当前节点为叶子节点
           if (deleteNode == root){
               root = null;
           }else{

               if (Comparator(deleteNode.getData(), parentNode.getData()) == -1) {
                   parentNode.leftChild = null;
               }else{
                   parentNode.rightChild = null;
               }
               deleteNode.parent = null;
           }
```

#### 有左子节点无右子节点

<p>如果是根节点，则将根节点赋值为被删除节点的左子节点；被删除节点的左子节点与被删除节点断开连接；</p>

<p>如果是不是根节点，则将被删除节点的父节点赋值为被删除节点的左子节点,被删除节点的左子节点与被删除节点断开连接；</p>

```

            if (deleteNode == root) {
                root = deleteNode.leftChild;
                deleteNode.leftChild = null;
                root.parent = null;
            }else{
                Node<T> leftChild = deleteNode.leftChild;
                if (Comparator(deleteNode.getData(), parentNode.getData()) == -1) {
                    parentNode.leftChild = leftChild;
                }else{
                    parentNode.rightChild = leftChild;

                }
                leftChild.parent = parentNode;
                deleteNode.parent = null;
                deleteNode.leftChild = null;
            }
```

#### 无左子节点有右子节点
这种情况与上一种类似

```
//当前节点 只有 右子节点
            if (deleteNode == root) {
                root = deleteNode.rightChild;
                deleteNode.rightChild = null;
                root.parent = null;
            }else{
                Node<T> rightChild = deleteNode.rightChild;
                if (Comparator(deleteNode.getData(), parentNode.getData()) == -1) {
                    parentNode.leftChild = rightChild;
                }else{
                    parentNode.rightChild = rightChild;
                }
                rightChild.parent = parentNode;
                deleteNode.parent = null;
                deleteNode.rightChild = null;
            }
```

#### 有左子节点有右子节点。

这种情况 分为两种小情况：
1. 被删除节点的右子节点的左子节点为空；
2. 被删除节点的右子节点的左子节点不为空；

##### 被删除节点的右子节点的左子节点为空

<p>因为二叉树的特点，节点的右边的关键值均大于左边的关键值，所以可以将被删除节点的左节点连接到被删除节点的右子节点的左子节点上。</p>

```

            Node<T> leftChild = deleteNode.leftChild;
            Node<T> rightChild = deleteNode.rightChild;
            if (rightChild.leftChild == null) {
                //删除节点的 右子节点 的 左子节点 为空
                if (deleteNode == root){
                    root = rightChild;
                    rightChild.parent = null;

                }else{
                    rightChild.parent = parentNode;
                    if (Comparator(deleteNode.getData(), parentNode.getData()) == -1) {
                        parentNode.leftChild = rightChild;
                    }else{
                        parentNode.rightChild = rightChild;
                    }
                }
                rightChild.leftChild = leftChild;
                leftChild.parent = rightChild;

            }else{
              .......
            }
            deleteNode.leftChild = null;
            deleteNode.rightChild = null;
            deleteNode.parent = null;
```

##### 被删除节点的右子节点的左子节点不为空

被删除节点的右子节点的左子节点不为空时，需要做三步：

1. 找到被删除节点的右边最小节点（目标节点）；
2. 将目标节点的右子节点连接到目标节点的父节点的左子节点上，目标节点与其右子节点断开连接，与其父节点断开连接；
3. 使用目标节点替代被删除节点。

```
            Node<T> leftChild = deleteNode.leftChild;
            Node<T> rightChild = deleteNode.rightChild;
            if (rightChild.leftChild == null) {
                ......
            }else{
                //删除节点的 右子节点 的 左子节点 不为空
                //1 找到 需要 删除节点 的 右侧 最小节点
                Node<T> minNode = findMinNode(deleteNode.rightChild);

                //如果 目标节点 有右子节点，将 右子节点 连接到 目标节点的父节点的左节点
                Node<T> minParent = minNode.parent;
                Node<T> minRight = minNode.rightChild;
                if (minRight == null) {
                    minParent.leftChild = null;
                }else{
                    minParent.leftChild = minRight;
                    minRight.parent = minParent;
                }

                //将 被删除节点的左右父子节点 连接 到 目标节点的左右父字节点
                minNode.leftChild = leftChild;
                minNode.rightChild = rightChild;
                leftChild.parent = minNode;
                rightChild.parent = minNode;
                minNode.parent = parentNode;
                if (deleteNode == root) {
                    root = minNode;
                }else{
                    if (Comparator(deleteNode.getData(), parentNode.getData()) == -1) {
                        parentNode.leftChild = minNode;
                    }else{
                        parentNode.rightChild = minNode;
                    }
                }
            }
            deleteNode.leftChild = null;
            deleteNode.rightChild = null;
            deleteNode.parent = null;

```

## 二叉树所有代码

```

/**
 * Author:JR
 * E-mail:jianr.yang@sunyard.com
 * Time: 2018/12/12
 * Description:
 */
public abstract class BinaryTree<T> {

    private Node<T> root;


    /**
     * Time: 2018/12/12 14:20
     * Description: 增加
     * @return <>true</> 成功
     *         <>false</> 失败
     */
    public boolean add(T t){

        if (root == null) {
            root = new Node<T>(t);
            return true;
        }

        Node<T> current = root;
        while (current != null) {
            if (Comparator(t, current.data) == -1) {
                if (current.leftChild == null) {
                    Node<T> newNode = new Node<T>(null, null, current, t);
                    current.leftChild = newNode;
                    return true;
                }else{
                    current = current.leftChild;
                }
            }else if (Comparator(t, current.data) == 1){
                if (current.rightChild == null) {
                    Node<T> newNode = new Node<T>(null, null, current, t);
                    current.rightChild = newNode;
                    return true;
                }else{
                    current = current.rightChild;
                }
            }else {
                current = null;
            }
        }
        return false;
    }

    /**
     * Time: 2018/12/12 14:20
     * Description: 查找
     * @return <>true</> 存在
     *         <>false</> 不存在
     */
    public boolean query(T t){
        if (root == null) {
            return false;
        }
        Node<T> current = root;
        while (current != null) {
            if (Comparator(t, current.getData()) == 0) {
                return true;
            }else if (Comparator(t, current.getData()) == -1) {
                current = current.leftChild;
            }else{
                current = current.rightChild;
            }
        }
        return false;
    }

    /**
     * Time: 2018/12/12 14:20
     * Description: 查找2
     * @return 节点
     */
    public Node<T> query2(T t){
        if (root == null) {
            return null;
        }

        Node<T> current = root;
        while (current != null) {
            if (Comparator(t, current.getData()) == 0) {
                return current;
            }else if (Comparator(t, current.getData()) == -1) {
                current = current.leftChild;
            }else{
                current = current.rightChild;
            }
        }
        return null;
    }

    public boolean delete(T t){
        Node<T> deleteNode = query2(t);
        if (deleteNode == null) {
            throw new NoSuchElementException();
        }

        Node<T> parentNode = deleteNode.parent;
        if (deleteNode.leftChild == null && deleteNode.rightChild == null) {
            //当前节点为叶子节点
            if (deleteNode == root){
                root = null;
            }else{

                if (Comparator(deleteNode.getData(), parentNode.getData()) == -1) {
                    parentNode.leftChild = null;
                }else{
                    parentNode.rightChild = null;
                }
                deleteNode.parent = null;
            }
        } else if (deleteNode.leftChild != null && deleteNode.rightChild == null) {
            //当前节点 只有 左子节点
            if (deleteNode == root) {
                root = deleteNode.leftChild;
                deleteNode.leftChild = null;
                root.parent = null;
            }else{
                Node<T> leftChild = deleteNode.leftChild;
                if (Comparator(deleteNode.getData(), parentNode.getData()) == -1) {
                    parentNode.leftChild = leftChild;
                }else{
                    parentNode.rightChild = leftChild;

                }
                leftChild.parent = parentNode;
                deleteNode.parent = null;
                deleteNode.leftChild = null;
            }
        } else if (deleteNode.leftChild == null) {
            //当前节点 只有 右子节点
            if (deleteNode == root) {
                root = deleteNode.rightChild;
                deleteNode.rightChild = null;
                root.parent = null;
            }else{
                Node<T> rightChild = deleteNode.rightChild;
                if (Comparator(deleteNode.getData(), parentNode.getData()) == -1) {
                    parentNode.leftChild = rightChild;
                }else{
                    parentNode.rightChild = rightChild;

                }
                rightChild.parent = parentNode;
                deleteNode.parent = null;
                deleteNode.rightChild = null;
            }
        } else {
            //当前节点 既有 左子节点 又有 右子节点

            Node<T> leftChild = deleteNode.leftChild;
            Node<T> rightChild = deleteNode.rightChild;
            if (rightChild.leftChild == null) {
                //删除节点的 右子节点 的 左子节点 为空
                if (deleteNode == root){
                    root = rightChild;
                    rightChild.parent = null;

                }else{
                    rightChild.parent = parentNode;
                    if (Comparator(deleteNode.getData(), parentNode.getData()) == -1) {
                        parentNode.leftChild = rightChild;
                    }else{
                        parentNode.rightChild = rightChild;
                    }
                }
                rightChild.leftChild = leftChild;
                leftChild.parent = rightChild;

            }else{
                //删除节点的 右子节点 的 左子节点 不为空
                //1 找到 需要 删除节点 的 右侧 最小节点
                Node<T> minNode = findMinNode(deleteNode.rightChild);

                //如果 目标节点 有右子节点，将 右子节点 连接到 目标节点的父节点的左节点
                Node<T> minParent = minNode.parent;
                Node<T> minRight = minNode.rightChild;
                if (minRight == null) {
                    minParent.leftChild = null;
                }else{
                    minParent.leftChild = minRight;
                    minRight.parent = minParent;
                }

                //将 被删除节点的左右父子节点 连接 到 目标节点的左右父字节点
                minNode.leftChild = leftChild;
                minNode.rightChild = rightChild;
                leftChild.parent = minNode;
                rightChild.parent = minNode;
                minNode.parent = parentNode;
                if (deleteNode == root) {
                    root = minNode;
                }else{
                    if (Comparator(deleteNode.getData(), parentNode.getData()) == -1) {
                        parentNode.leftChild = minNode;
                    }else{
                        parentNode.rightChild = minNode;
                    }
                }
            }
            deleteNode.leftChild = null;
            deleteNode.rightChild = null;
            deleteNode.parent = null;
        }

        return true;
    }

    /**
     * Time: 2018/12/12 15:13
     * Description: 找到最小节点
     */
    private Node<T> findMinNode(Node<T> node){
        if (node == null) {
            return null;
        }
        while (node.leftChild != null) {
            node = node.leftChild;
        }
        return node;
    }

    /**
     * Time: 2018/12/12 14:20
     * Description: 打印数据 用于展示
     */
    public void midOrderTraverse(){
        midOrderTraverse(root);
    }

    /**
     * 中序遍历
     * @param root
     */
    public void midOrderTraverse(Node<T> root){
        if (root == null) {
            return;
        }
        midOrderTraverse(root.leftChild);
        System.out.print("  " + root.getData().toString());
        midOrderTraverse(root.rightChild);
    }


    /**
     * Time: 2018/12/12 13:53
     * Description: 如果 t1 > t2 , 返回 1
     *              如果 t1 = t2 ，返回 0
     *              如果 t1 < t2 ，返回 -1
     */
    public abstract int Comparator(T t1, T t2);

    class Node<T> {

        private Node<T> leftChild;

        private Node<T> rightChild;

        private Node<T> parent;

        private T data;

        public Node(T data) {
            this.leftChild = null;
            this.rightChild = null;
            this.parent = null;
            this.data = data;
        }

        public Node(Node<T> leftChild, Node<T> rightChild, Node<T> parent, T data) {
            this.leftChild = leftChild;
            this.rightChild = rightChild;
            this.parent = parent;
            this.data = data;
        }

        public Node<T> getLeftChild() {
            return leftChild;
        }

        public void setLeftChild(Node<T> leftChild) {
            this.leftChild = leftChild;
        }

        public Node<T> getRightChild() {
            return rightChild;
        }

        public void setRightChild(Node<T> rightChild) {
            this.rightChild = rightChild;
        }

        public Node<T> getParent() {
            return parent;
        }

        public void setParent(Node<T> parent) {
            this.parent = parent;
        }

        public T getData() {
            return data;
        }

        public void setData(T data) {
            this.data = data;
        }
    }
}
```
