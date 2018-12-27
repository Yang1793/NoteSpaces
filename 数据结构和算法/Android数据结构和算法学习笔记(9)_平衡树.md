## AVL树

<p>AVL树,就是平衡二叉树，是一种二叉排序树，其中每一个节点的左子树和右子树的高度差至多等于1。</p>

## 平衡因子（balance facter）

<p>二叉树上左子树深度减去右子树深度的值称为平衡因子。</p>

<p>当插入一个因素，打破二叉树的平衡时，距离插入节点最近，且平衡因子的绝对值大于1的节点为根的子树，我们称为最小不平衡子树。</p>

<![最小不平衡子树]()

## 创建AVL树

### 左旋

<p>节点向左转动。</p>


```
/**
     * 左旋
     */
    private void leftRotate(Node<T> node){
        Node<T> rightChild = node.rightChild;
        Node<T> parent = node.parent;
        if (rightChild == null) {
            return;
        }
        if (rightChild.leftChild != null) {
            node.rightChild = rightChild.leftChild;
            rightChild.leftChild.parent = node;
        }else{
            node.rightChild = null;
        }
        rightChild.leftChild = node;
        node.parent = rightChild;

        rightChild.parent = parent;
        if (parent != null) {
            if (compareTo(parent.t, rightChild.t) == 1) {
                parent.leftChild = rightChild;
            } else {
                parent.rightChild = rightChild;
            }
        }else{
            root = rightChild;
        }
    }

```

### 右旋

<p>节点向右转动。</p>

```
/**
    * 右旋
    */
   private void rightRotate(Node<T> node){
       Node<T> leftChild = node.leftChild;
       Node<T> parent = node.parent;
       if (leftChild == null) {
           return;
       }
       if (leftChild.rightChild != null) {
           node.leftChild = leftChild.rightChild;
           leftChild.rightChild.parent = node;
       }else{
            node.leftChild = null;
        }
       leftChild.rightChild = node;
       node.parent = leftChild;

       leftChild.parent = parent;
       if (parent != null) {
           if (compareTo(parent.t, leftChild.t) == 1) {
               parent.leftChild = leftChild;
           } else {
               parent.rightChild = leftChild;
           }
       }else{
           root = leftChild;
       }
   }
```
### 左平衡操作

<p> 左平衡操作，即结点t的不平衡是因为左子树过深</p>

1. 如果新的结点插入到t的左孩子的左子树中，则直接进行右旋操作即可。

2. 如果新的结点插入到t的左孩子的右子树中，则需要进行分情况讨论

  + 情况a：当t的左孩子的右子树根节点的balance = RIGHT_HIGH
  - 情况b：当t的左孩子的右子树根节点的balance = LEFT_HIGH
  + 情况c：当t的左孩子的右子树根节点的balance = EQUAL_HIGH

```
private void leftBalance(Node<T> t){
        Node<T> tl = t.leftChild;
        switch (tl.balanceFacter) {
            case LH:
                rightRotate(t);
                t.balanceFacter = EH;
                tl.balanceFacter = EH;
                break;

            case RH:

                Node<T> tlr = tl.rightChild;
                switch (tlr.balanceFacter) {
                    case LH:
                        t.balanceFacter = RH;
                        tl.balanceFacter = EH;
                        tlr.balanceFacter = EH;
                        break;
                    case RH:
                        t.balanceFacter = EH;
                        tl.balanceFacter = LH;
                        tlr.balanceFacter = EH;
                        break;
                    case EH:
                        t.balanceFacter = EH;
                        tl.balanceFacter = EH;
                        tlr.balanceFacter = EH;
                        break;
                }
                leftRotate(t.leftChild);
                rightRotate(t);
                break;
        }
    }
```

### 右平衡操作

<p> 右平衡操作，即结点t的不平衡是因为右子树过深</p>

1. 如果新的结点插入到t的右孩子的右子树中，则直接进行左旋操作即可。

2. 如果新的结点插入到t的右孩子的左子树中，则需要进行分情况讨论

  + 情况a：当t的右孩子的左子树根节点的balance = LEFT_HIGH
  - 情况b：当t的右孩子的左子树根节点的balance = RIGHT_HIGH
  + 情况c：当t的右孩子的左子树根节点的balance = EQUAL_HIGH

```
private void rightBalance(Node<T> t){
        Node<T> tr = t.rightChild;
        switch (tr.balanceFacter) {
            case RH:
                leftRotate(t);
                t.balanceFacter = EH;
                tr.balanceFacter = EH;
                break;

            case LH:

                Node<T> trl = tr.leftChild;
                switch (trl.balanceFacter) {
                    case LH:
                        t.balanceFacter = EH;
                        tr.balanceFacter = RH;
                        trl.balanceFacter = EH;
                        break;
                    case RH:
                        t.balanceFacter = LH;
                        tr.balanceFacter = EH;
                        trl.balanceFacter = EH;
                        break;
                    case EH:
                        t.balanceFacter = EH;
                        tr.balanceFacter = EH;
                        trl.balanceFacter = EH;
                        break;
                }
                rightRotate(t.rightChild);
                leftRotate(t);
                break;
        }
    }

```

### AVL树 插入数据

<p>了解上述的8中情况，加上左右旋转，就可以插入数据了。</p>

```
public boolean addNode(T t){
       if (root == null) {
           root = new Node<>(t, null, null, null, 0);
           size = 1;
           return true;
       }else{
           //找到插入位置
           Node<T> parent = null;
           Node<T> node = root;
           while (node != null) {
               parent = node;
               if (compareTo(node.getT(), t) == 1) {
                   node = node.leftChild;
               } else if (compareTo(node.getT(), t) == -1) {
                   node = node.rightChild;
               } else {
                   return false;
               }
           }

           //插入数据
           if (compareTo(parent.getT(), t) == 1) {
               parent.leftChild = new Node<>(t, parent, null, null, 0);
           } else {
               parent.rightChild = new Node<>(t, parent, null, null, 0);
           }

           //检查平衡，回溯查找
           while (parent != null) {
               if (compareTo(parent.getT(), t) == 1) {
                   parent.balanceFacter++;
               }else{
                   parent.balanceFacter--;
               }

               if (parent.balanceFacter == 0) {
                   break;
               }

               if (parent.balanceFacter == 2 || parent.balanceFacter == -2) {
                   fixBalance(parent);
                   break;
               }else{
                   parent = parent.parent;
               }
           }
       }
       size++;
       return true;
   }

   protected void fixBalance(Node<T> parent){

       if (parent.balanceFacter == 2) {
           leftBalance(parent);
       }

       if (parent.balanceFacter == -2) {
           rightBalance(parent);
       }
   }
```
