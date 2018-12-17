## 哈夫曼树
<p>简单说：带权最小路劲二叉树就是哈夫曼树</p>

<![](https://github.com/Yang1793/NoteSpaces/blob/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E5%92%8C%E7%AE%97%E6%B3%95/picture/huffman_tree_3.png?raw=true)

## 生成哈夫曼树

<![](https://github.com/Yang1793/NoteSpaces/blob/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E5%92%8C%E7%AE%97%E6%B3%95/picture/huffman_tree_4.png?raw=true)

<![](https://github.com/Yang1793/NoteSpaces/blob/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E5%92%8C%E7%AE%97%E6%B3%95/picture/huffman_tree_5.png?raw=true)

## 哈夫曼树的应用

压缩 使用
<![](https://github.com/Yang1793/NoteSpaces/blob/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E5%92%8C%E7%AE%97%E6%B3%95/picture/huffman_tree_6.png?raw=true)

<![](https://github.com/Yang1793/NoteSpaces/blob/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E5%92%8C%E7%AE%97%E6%B3%95/picture/huffman_tree_7.png?raw=true)

## 简单生产练习
```
public class HuffmanTree {
    TreeNode root;
    /**
     * 生成哈夫曼树
     */
    public TreeNode createHuffManTree(ArrayList<TreeNode> list) {
        while (list.size() > 1) {
            Collections.sort(list);
            TreeNode left = list.get(list.size() - 1);
            TreeNode right = list.get(list.size() - 2);
            TreeNode parent = new TreeNode("p", left.weight + right.weight);
            parent.leftChild = left;
            left.parent = parent;
            parent.rightChild = right;
            right.parent = parent;
            list.remove(left);
            list.remove(right);
            list.add(parent);
        }
        root = list.get(0);
        return list.get(0);
    }

    /**
     * 横向打印
     */
    public void showHuffman(TreeNode root) {
        LinkedList<TreeNode> list = new LinkedList<>();
        list.offer(root);//入队
        while (!list.isEmpty()) {
            TreeNode node = list.pop();
            System.out.println(node.data);
            if (node.leftChild != null) {
                list.offer(node.leftChild);
            }
            if (node.rightChild != null) {
                list.offer(node.rightChild);
            }
        }
    }

    /**
     * 测试
     *
     */
    @Test
    public void test() {
        ArrayList<TreeNode> list = new ArrayList<>();
        TreeNode<String> node = new TreeNode("good", 50);
        list.add(node);
        list.add(new TreeNode("morning", 10));
        TreeNode<String> node2 =new TreeNode("afternoon", 20);
        list.add(node2);
        list.add(new TreeNode("hell", 110));
        list.add(new TreeNode("hi", 200));
        HuffmanTree tree = new HuffmanTree();
        tree.createHuffManTree(list);
        tree.showHuffman(tree.root);
        getCode(node2);
    }

    /**
     * 编码
     */
    public void getCode(TreeNode node) {
        TreeNode tNode = node;
        Stack<String> stack = new Stack<>();
        while (tNode != null && tNode.parent != null) {
            //left 0  right 1
            if (tNode.parent.leftChild == tNode) {
                stack.push("0");
            } else if (tNode.parent.rightChild == tNode) {
                stack.push("1");
            }
            tNode = tNode.parent;
        }
        System.out.println();
        while (!stack.isEmpty()) {
            System.out.print(stack.pop());
        }
    }

    /**
     * 结点
     *
     * @param <T>
     */
    public static class TreeNode<T> implements Comparable<TreeNode<T>> {
        T data;
        int weight;
        TreeNode leftChild;
        TreeNode rightChild;
        TreeNode parent;

        public TreeNode(T data, int weight) {
            this.data = data;
            this.weight = weight;
            leftChild = null;
            rightChild = null;
            parent = null;
        }

        @Override
        public int compareTo(@NonNull TreeNode<T> o) {
            if (this.weight > o.weight) {
                return -1;
            } else if (this.weight < o.weight) {
                return 1;
            }
            return 0;
        }
    }
}
```
