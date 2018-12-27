# 分治法

## 查找技术

### 顺序查找
<p>如果线性表为无序表，即表中元素的排列是无序的，则不管线性表采用顺序存储还是链式存储，都必须使用顺序查找。
  如果线性表有序，但采用链式存储结构，则也必须使用顺序查找。</p>

### 二分查找
<p>数据有序</p>

<p>注意：设计成左闭右开--是一种区间无重复的思想
  random(０，１)等大量的数学函数
  for(int i=0;i<array.length;i++）</p>

```
private int binarySearch(int[] data, int aim) {
        if (data == null || data.length == 0) {
            return -1;
        }
        int low = 0;
        int height = data.length;
        int mid = -1;
        while (low < height) {
            mid = low + ((height - low) >> 1);
            if (aim < data[mid]) {
                height = --mid;
            } else if (aim > data[mid]) {
                low = ++mid;
            } else {
                return mid;
            }
        }
        return -(mid + 1);
    }
```

## 快速排序（树的前序遍历）

<p>应用场景
  数据量大并且是线性结构</p>
  <p>短处
  有大量重复数据的时候，性能不好
  单向链式结构处理性能不好（一般来说，链式都不使用）</p>

```
/**
     * Time: 2018/12/9 20:16
     * Description: 快速排序
     */
    private void fastSort(int[] data, int low, int height) {
        if (data == null || data.length == 0) {
            return;
        }
        int length = data.length;
        if (low < 0 || low > length || height < 0 || height > length) {
            return;
        }

        if (low >= height) {
            return;
        }

        int aim = data[low];
        boolean tag = true;
        int left = low;
        int right = height;

        L1:
        while (left < right) {
            if (tag) {
                for (int i = right; i > left; i--) {
                    if (data[i] < aim) {
                        data[left++] = data[i];
                        right = i;
                        tag = !tag;
                        continue L1;
                    }
                }
                right = left;
            } else {

                for (int i = left; i < right; i++) {
                    if (data[i] > aim) {
                        data[right--] = data[i];
                        left = i;
                        tag = !tag;
                        continue L1;
                    }
                }
                left = right;
            }
        }

        data[left] = aim;
        fastSort(data, low, left-1);
        fastSort(data, left+1, height);

    }
```


## 归并排序（树的后序遍历）

<p>  
  应用场景
    数据量大并且有很多重复数据，链式结构
  短处
    需要空间大</p>

```
private void mergeSort(int[] data, int low, int height){
       if (data == null) {
           return;
       }
       int length = data.length;
       if (low < 0 || height < 0 || low >= length || height >= length
               || height - low <=0) {

           return;
       }
       int mid = low + ((height - low) >> 1);
       mergeSort(data, low, mid);
       mergeSort(data, mid + 1, height);
       mergeData(data, low, mid, height);
   }

   private void mergeData(int[] data, int low, int mid, int height) {
       int[] leftArray = new int[mid - low + 1];
       int[] rightArray = new int[height - mid];//包含mid

       for (int i = 0; i < mid - low + 1; i++) {
           leftArray[i] = data[low + i];
       }

       for (int i = 0; i < height - mid; i++) {
           rightArray[i] = data[mid + 1 + i];
       }

       int leftIndex = 0;
       int rightIndex = 0;
       int index = low;
       while (leftIndex < leftArray.length && rightIndex < rightArray.length) {
           if (leftArray[leftIndex] < rightArray[rightIndex]) {
               data[index] = leftArray[leftIndex++];
           }else{
               data[index] = rightArray[rightIndex++];
           }
           index++;
       }

       for (int i = leftIndex; i < leftArray.length; i++) {
           data[index++] = leftArray[i];
       }
       for (int i = rightIndex; i < rightArray.length; i++) {
           data[index++] = rightArray[i];
       }
   }
```
