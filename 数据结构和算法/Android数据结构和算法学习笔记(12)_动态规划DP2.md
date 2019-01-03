## 动态规划DP 2
图论基础算法：Floyd算法（罗伯特·弗洛伊德命名）和 Dijkstra（迪杰斯特拉）算法。

### Floyd算法（罗伯特·弗洛伊德命名）

这是一个多源最短路径的算法。
1. 先生成图所对应的邻接矩阵与路径矩阵
2. 用d[i][j]表示i点到j点的最短距离，但i到j可能会经过其他的顶点才能找到最短路径，则用d[i][j] = d[i][k] + d[k][j]表示，其中k这样的顶点有多个。
3. 遍历全部顶点，如果出现d[i][j] > d[i][k] + d[k][j],我们就用短的路径替换长的路径，如图中的v1->v2直接走需要权重为4，用v1->v0->v2权重为3。
4. 为了找到全部节点对之间的最短路径，可以在路径计算过程中，取数据p[i][j] = p[i][k]用于记录每次替换的过程。

<img src="https://raw.githubusercontent.com/Yang1793/NoteSpaces/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E5%92%8C%E7%AE%97%E6%B3%95/picture/dp2_1.png" width='600' height='170' align=center/>

<img src="https://raw.githubusercontent.com/Yang1793/NoteSpaces/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E5%92%8C%E7%AE%97%E6%B3%95/picture/dp2_2.png" width='600' height='1000' align=center/>

```
/**
 * Author:JR
 * Time: 2019/1/2
 * Description: Floyd算法（罗伯特·弗洛伊德命名）
 */
public class Floye {

    @Test
    public void test(){
        floyeAlgorithms();
        for (int[] anInt : graphDemo) {
            for (int i : anInt) {
                System.out.print(i + "  ");
            }
            System.out.println();
        }
        System.out.println("======================");
        for (int[] anInt : path) {
            for (int i : anInt) {
                System.out.print(i + "  ");
            }
            System.out.println();
        }
    }

    private final int MAX = 100;

    // 图对应的邻接矩阵
    private int[][] graphDemo = {
            {0, 2, 1, 5},
            {2, 0, 4, MAX},
            {1, 4, 0, 3},
            {5, MAX, 3, 0}
    };

    private int[][] path = {
            {0, 1, 2, 3},
            {0, 1, 2, 3},
            {0, 1, 2, 3},
            {0, 1, 2, 3}
    };

    private void floyeAlgorithms(){

        //循环 每一个顶点
        for (int i = 0; i < graphDemo.length; i++) {
            //每个顶点中 需要计算
            for (int j = 0; j < graphDemo.length; j++) {
                for (int k = 0; k < graphDemo.length; k++) {
                    if (j == i || k == j) {
                        continue;
                    }
                    if (graphDemo[j][k] > graphDemo[j][i] + graphDemo[i][k]) {
                        graphDemo[j][k] = graphDemo[j][i] + graphDemo[i][k];
                        path[j][k] = path[j][i];
                    }
                }
            }
        }
    }
}

```


### Dijkstra（迪杰斯特拉）算法

相对于暴力简单的Floyd算法，Dijkstra算法更为有用且复杂度较为合理--O(N^2)。</br>
Dijkstra算法使用了广度优先搜索解决赋权有向图或者无向图的单源最短路径问题，算法最终得到一个最短路径树。该算法常用于路由算法或者作为其他图算法的一个子模块。

1. 先生成图所对应权重的邻接矩阵
2. 初始化信息。（定义路径数组，定义一个数组保存用于标识顶点是否已经经过等等）
3. 寻找距离当前顶点权重最小的顶点。
4. 根据新找到的顶点，修正权重。

<img src='https://github.com/Yang1793/NoteSpaces/blob/graph_dp/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E5%92%8C%E7%AE%97%E6%B3%95/picture/dp2_3.png?raw=true' height='150' aling=center/>
```
private static final int I = 100;
private int[][] array=new int[][]{
            {0,1,5,I,I,I,I,I,I},
            {1,0,3,7,5,I,I,I,I},
            {5,3,0,I,1,7,I,I,I},
            {I,7,I,0,2,I,3,I,I},
            {I,5,1,2,0,3,6,I,I},
            {I,I,7,I,3,0,I,5,I},
            {I,I,I,3,6,I,0,2,7},
            {I,I,I,I,9,5,2,0,4},
            {I,I,I,I,I,I,7,4,0}
    };
```

```
private void dijkstraAlgorithms(){
        //表示当前顶点
        int k = 0;
        //路径数组
        int[] path = new int[array.length];
        int[] weight = array[k];
        //定义数组 保存顶点是否经过 下标代表顶点， --->  值代表是否经过这个顶点，0没有经过，1已经经过
        int[] flag = new int[array.length];
        flag[k] = 1;

        for (int i = 0; i < array.length; i++) {
            int min = I;

            //找到距离最近的顶点
            for (int j = 0; j < weight.length; j++) {
                if (flag[j] == 0 && min > weight[j]) {
                    min = weight[j];
                    k = j;
                }
            }
            flag[k] = 1;

            //修正权重
            for (int j = 0; j < weight.length; j++) {
                if (flag[j] == 0 && min + array[k][j] < weight[j]) {
                    weight[j] = min + array[k][j];
                    path[j] = k;
                }
            }
        }


        for(int i=0;i<path.length;i++){
            System.out.print(path[i]+" ");
        }
        System.out.println();
        for(int i=0;i<weight.length;i++){
            System.out.print(weight[i]+" ");
        }

        //打印结果
        int v=8;
        while(v!=0){
            System.out.print(v + "--->" + path[v] + "  ");
            v=path[v];
        }

    }
```

### 
