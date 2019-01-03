## 最小生成树

一个有 n 个结点的连通图的生成树是原图的极小连通子图，且包含原图中的所有 n 个结点，并且有保持图连通的最少的边。 [1]  最小生成树可以用kruskal（克鲁斯卡尔）算法或prim（普里姆）算法求出。

### kruskal（克鲁斯卡尔）算法实现

**大致思路如下**
1. 构建图；
2. 找出并保存所有的边以及边对应的权重；
3. 将找出的边根据权重排序；
4. 定义一个数组，用于标识构建树过程中顶点的连通关系；
5. 将排序好的边数组循环，通过连通数组判断边对应的顶点是否连通，如果未连通，则连通这两个顶点。

关键代码如下：

```
private void kruskalAlgorithims(){

        //获取所有的边
        Edge[] edge = getEdge();

        //按照权重排序边
        sortEdge(edge);

        //定义一个数组 存放连通的顶点，表示连通的关系
        //为了优化空间 下标表示起点，值表示终点
        int[] connectedArray = new int[verticeSize];

        //初始化结果数组
        Edge[] resultEdge = new Edge[edgeSize];

        //循环排序好的边
        //根据取出的边的两端的顶点 根据连通关系数组判断 这两个点是否已经是连通关系
        //如果未连通 则连通这两个顶点
        int index = 0;
        for (int i = 0; i < edgeSize; i++) {
            Edge arg = edge[i];
            int start = arg.start;
            int end = arg.end;
            int n = getEnd(connectedArray, start);
            int m = getEnd(connectedArray, end);
            if (n != m) {
                resultEdge[index++] = arg;
                if (n > m) {
                    connectedArray[m] = n;
                }else{
                    connectedArray[n] = m;
                }
            }
        }

        priintData(resultEdge, index);
    }
```

实现结构：

<img src='https://github.com/Yang1793/NoteSpaces/blob/graph_dp/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E5%92%8C%E7%AE%97%E6%B3%95/picture/dp3_1.png?raw=true' width='' height='160' align=center/>

全部代码：

```
/**
 * Author:JR
 * Time: 2019/1/3 9:34:11
 * Description: kruskal（克鲁斯卡尔）算法实现最小生成树
 */
public class Kruskal {

    @Test
    public void test(){
        createEdge();
        kruskalAlgorithims();
    }

    public static final int MAX_WEIGHT = 0xFFF8;
    private int verticeSize = 7;
    public int[] vertices = new int[verticeSize]; // 顶点数组
    public int[][] matrix = new int[verticeSize][verticeSize]; // 图
    private int edgeSize = -1;

    private class Edge{
        private int start;
        private int end;
        private int weight;

        public Edge(int start, int end, int weight) {
            this.start = start;
            this.end = end;
            this.weight = weight;
        }

        public int getStart() {
            return start;
        }

        public void setStart(int start) {
            this.start = start;
        }

        public int getEnd() {
            return end;
        }

        public void setEnd(int end) {
            this.end = end;
        }

        public int getWeight() {
            return weight;
        }

        public void setWeight(int weight) {
            this.weight = weight;
        }
    }

    private void createEdge() {
        int[] v0 = new int[] {0, 50, 60, MAX_WEIGHT, MAX_WEIGHT,MAX_WEIGHT,MAX_WEIGHT};
        int[] v1 = new int[] {50, 0, MAX_WEIGHT, 65, 40,MAX_WEIGHT,MAX_WEIGHT};
        int[] v2 = new int[] {60, MAX_WEIGHT, 0, 52, MAX_WEIGHT,MAX_WEIGHT,45};
        int[] v3 = new int[] {MAX_WEIGHT, 65, 52, 0, 50,30,42};
        int[] v4 = new int[] {MAX_WEIGHT, 40, MAX_WEIGHT, 50, 0,70,MAX_WEIGHT};
        int[] v5 = new int[] {MAX_WEIGHT, MAX_WEIGHT, MAX_WEIGHT, 30,70,0,MAX_WEIGHT};
        int[] v6 = new int[] {MAX_WEIGHT, MAX_WEIGHT, 45, 42,MAX_WEIGHT,MAX_WEIGHT,0};
        matrix[0] = v0;
        matrix[1] = v1;
        matrix[2] = v2;
        matrix[3] = v3;
        matrix[4] = v4;
        matrix[5] = v5;
        matrix[6] = v6;
    }

    private void kruskalAlgorithims(){

        //获取所有的边
        Edge[] edge = getEdge();

        //按照权重排序边
        sortEdge(edge);

        //定义一个数组 存放连通的顶点，表示连通的关系
        //为了优化空间 下标表示起点，值表示终点
        int[] connectedArray = new int[verticeSize];

        //初始化结果数组
        Edge[] resultEdge = new Edge[edgeSize];

        //循环排序好的边
        //根据取出的边的两端的顶点 根据连通关系数组判断 这两个点是否已经是连通关系
        //如果未连通 则连通这两个顶点
        int index = 0;
        for (int i = 0; i < edgeSize; i++) {
            Edge arg = edge[i];
            int start = arg.start;
            int end = arg.end;
            int n = getEnd(connectedArray, start);
            int m = getEnd(connectedArray, end);
            if (n != m) {
                resultEdge[index++] = arg;
                if (n > m) {
                    connectedArray[m] = n;
                }else{
                    connectedArray[n] = m;
                }
            }
        }

        priintData(resultEdge, index);
    }

    private void priintData(Edge[] resultEdge,int index) {

        int lengh = 0;
        for(int i = 0; i < index; i++) {
            lengh+= resultEdge[i].weight;
        }
        System.out.println("最小生成树的权重之和" + lengh);
        char[] chars = new char[verticeSize];
        chars[0] = 'A';
        chars[1] = 'B';
        chars[2] = 'C';
        chars[3] = 'D';
        chars[4] = 'E';
        chars[5] = 'F';
        chars[6] = 'G';

        for (int i = 0; i < index; i++) {
            System.out.printf("(%s, %s)---> %d \n",chars[resultEdge[i].start], chars[resultEdge[i].end], matrix[resultEdge[i].start][resultEdge[i].end]);
        }
    }

    private int getEnd(int[] connectedArray, int start) {
        int i = start;
        while (connectedArray[i] != 0){
            i = connectedArray[i];
        }
        return i;
    }

    private void sortEdge(Edge[] edge) {
        for (int i = 0; i < edgeSize; i++) {
            for (int j = i+1; j < edgeSize; j++) {
                if (edge[i].weight > edge[j].weight) {
                    Edge temp = edge[i];
                    edge[i] = edge[j];
                    edge[j] = temp;
                }
            }
        }
    }

    private Edge[] getEdge() {
        Edge[] edge = new Edge[verticeSize * verticeSize];
        int index = 0;
        for (int i = 0; i < verticeSize; i++) {
            for (int j = 0; j < verticeSize; j++) {
                if (matrix[i][j] != 0 && matrix[i][j] != MAX_WEIGHT) {
                    edge[index++] = new Edge(i, j, matrix[i][j]);
                }
            }
        }
        edgeSize = index;
        return edge;
    }
}
```
