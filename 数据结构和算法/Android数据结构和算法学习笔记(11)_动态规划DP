## 动态规划DP
<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

### 概念

动态规划(dynamic programming)是运筹学的一个分支，是求解决策过程(decision process)最优化的数学方法。
20世纪50年代初美国数学家R.E.Bellman等人在研究多阶段决策过程(multistep decision process)的优化问题时，提出了著名的最优化原理(principle of optimality)，把多阶段过程转化为一系列单阶段问题，利用各阶段之间的关系，逐个求解，创立了解决这类过程优化问题的新方法——动态规划。

### 特点

特点：分析是从大到小，写代码是从小到大
计算过程中会把结果都记录下，最终结果在记录中找到。

### 最长公共子序列(Longest Common Subsequence,lcs)
##### 定义
  + 一个序列A任意删除若干个字符得到新序列B，则A叫做B的子序列。
  - 两个序列X和Y的公共子序列中,长度最长的那个,定义为X和Y的最长公共子序列。
  + 例如X={A,B,C,B,D,A,B}，Y={B,D,C,A,B,A}则它们的lcs是{B,C,B,A}和{B,D,A,B}。求出一个即可。

##### LCS应用
1. 相似度的比较:计算生物学DNA对比(亲子验证),,,百度云盘找非法数据(岛国片)。
2. 图形相似处理,媒体流的相似比较,百度知道,百度百科,,WEB页面中找非法言论。

##### 实现方式
1. 穷举法（现实实现中不可取）
2. 动态规划DP

##### 算法分析
条件：
1. 字符串X，长度m，从1开始计数
2. 字符串Y，长度n，从1开始计数
3. LCS(X,Y)表示字符串X和Y最长公共子序列，其中的一个解为Z={z1,z2,...zk}

若 Xm = Yn (最后一个字符相同)，则字符串X和Y的最长公共子序列Zk的最后一个字符必定为Xm(=Yn).</br>
即Xm = Yn,LCS(X,Y) = LCS(Xm-1,Yn-1) + Xm。</br>
比如 X5 = Y4 = 'B',LCS(BDCAB,ABCB) = LCS(BDCA,ABC) + 'B'。

若 Xm != Yn,则字符串X和Y的最长公共子序列必定在 字符串X与字符串Y减少最后一位的最长公共子序列 和  字符串X减少最后一位与字符串Y的最长公共子序列 中产生。</br>
即 Xm != Yn,LCS(X,Y) = Max(LCS(Xm,Yn-1), LCS(Xm-1,Yn))。 </br>
比如 X5 != Y4 ,LCS(BDCAC,ABCB) = Max( LCS(BDCAC,ABC) + LCS(BDCA,ABCB))。

$$LCS(X,Y) = \begin{cases}
LCS(Xm-1,Yn-1) + Xm & Xm = Yn \\
\max\(LCS(Xm,Yn-1), LCS(Xm-1,Yn)) & X5 \neq Y4
\end{cases}$$

##### 代码分析与编码
求长度为m的字符X 和 长度为n的字符串Y最长公共子序列。</br>
首先创建一个二维数组，temp[m+1][n+1]。</br>
循环数组，坐标都从1开始，取值方式：相同的取左上加1,不同取上和左中的最大值。

```
    private String getLcs(String a, String b) {
        int[][] matrix = new int[a.length()+1][b.length()+1];
        char[] aChars = a.toCharArray();
        char[] bChars = b.toCharArray();

        for (int i = 1; i <= aChars.length; i++) {
            for (int j = 1; j <= bChars.length; j++) {
                matrix[i][j] = getData(matrix, aChars, bChars, i, j);
            }
        }

       // .......
    }

    private int getData(int[][] matrix, char[] aChars, char[] bChars, int i, int j){

        if (aChars[i-1] == bChars[j-1]) {
            return matrix[i-1][j-1] + 1;
        }else{
            return matrix[i-1][j] > matrix[i][j-1] ? matrix[i-1][j] : matrix[i][j-1];
        }
    }

```

求出最大公共子序列之后，倒叙找出这个字符串。

```

    private String getLcs(String a, String b) {
        // do something。。。。。。

        int i = a.length();
        int j = b.length();
        StringBuilder sb = new StringBuilder();
        while (i > 0 && j > 0) {
            if (aChars[i - 1] == bChars[j - 1]) {
                sb.insert(0, aChars[i-1]);
                i--;j--;
            }else{
                if (matrix[i-1][j] > matrix[i][j-1]){
                    i--;
                }else{
                    j--;
                }
            }
        }

        return sb.toString();
    }


```

----

### KMP算法（改进的字符串匹配算法）

KMP算法是一种改进的字符串匹配算法，由D.E.Knuth，J.H.Morris和V.R.Pratt同时发现，因此人们称它为克努特——莫里斯——普拉特操作（简称KMP算法）。

KMP算法的关键是利用匹配失败后的信息，尽量减少模式串与主串的匹配次数以达到快速匹配的目的

时间复杂度 O(m+n)

##### 思路

找出模式串匹配失败后退回的位置。

定义一个数组(下面叫这个数据为next[]),数组长度与模式串的长度相同，

用这个数组记录模式串中对应字符(从第二位开始到最后一位)与模式串开头字符相同的位数。

比如模式串为：'ababcaba'，那它对应的数组next[]={0,0,1,2,0,1,2,3}

得到这样子的数组后，匹配目标串和模式串时：
1. 如果匹配的字符相同时，就继续匹配下一位目标串和下一位模式串；
2. 如果匹配到的字符不同，先找到匹配不上的这个字符在next中的值position，再将目标串中匹配不上的值与模式串position中的值匹配。

如果模式串里面的字符都被匹配到了，则目标串中包含模式串；</br>
如果目标串匹配完了，模式串还是没有完全匹配，则目标串中不包含模式串。

```
/**
     * 模式字符串遍历，数组表示到某一个字符为止与字符串开头匹配的个数
     *
     * 就比如 ‘ababcaba’得到的重复数组为 ‘00120123’
     * @param arg 模式字符串
     * @return 模式字符串对应的重复数组
     */
    private int[] getNextArray(String arg) {

        int[] result = new int[arg.length()];

        for (int i = 1, j = 0; i < arg.length(); i++) {
            //匹配字符串 和  模式字符串头部
            while (j > 1 && arg.charAt(i) != arg.charAt(j)) {
                j = result[j - 1];
            }

            if (arg.charAt(i) == arg.charAt(j)) {
                j++;
            }

            result[i] = j;
        }
        return result;
    }

```

```
/**
     * 找到模式串在目标串中的位置
     *
     * @param source 目标字符串
     * @param target 模式字符串
     * @param kmpArray 模式字符串对应的数组
     * @return 模式字符串对应的重复数组
     */
private int kmp(String source, String target, int[] kmpArray){

        for (int i = 0, j = 0; i < source.length(); i++) {
            while (j > 0 && source.charAt(i) != target.charAt(j)) {
                j = kmpArray[j-1];
            }

            if (source.charAt(i) == target.charAt(j)) {
                j++;
            }

            if (j == target.length()) {
                return i-j+1;
            }
        }

        return -1;
    }

```
