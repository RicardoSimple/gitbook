# 数据结构和算法
二分法————————猜数游戏
### 复杂度
#### 大O记法
时间复杂度

需要一个不用具体的数据和环境就可以粗略估计算法执行效率的方法，称为**大O记法**

使用步数作为时间复杂度的计算

比如数组的每次索引值的读取就算一步，称为unit_time
对数据操作一次算作一步

计算机可以一步跳到任意一个位置上进行数据读取，所以随着数组的长度增加，时间复杂度并不会提升

这个如果用大O记法记为  **常数时间——O(1)**

而猜数字的时间复杂度会随之增加，记作  **对数时间——O(log(N))**

只需关注for循环的部分及嵌套

二分法代码(数组有序)
```
public static int find(int[] array, int aim) {
    // 初始化left = 最左侧, right = 最右侧
    int left = 0;
    int right = array.length - 1;

    // 当left > right则表示遍历完成
    while (left <= right) {
        // 获取中间的值
        int middle = (left + right) / 2;
        int middleValue = array[middle];
        if (middleValue == aim) {
            // 已经找到目标
            return middle;
        } else if (middleValue < aim) {
            // 目标在middle的右侧，重置left
            left = middle + 1;
        } else {
            // 目标在middle的左侧，重置right
            right = middle - 1;
        }
    }
    return -1;
}
```

重复问题

遍历下标加1，代码：
```
public static ArrayList<Integer> repeat(int[] array) {
    ArrayList<Integer> result = new ArrayList<>();
    int[] exists = new int[11];
    for (int i = 0; i < array.length; i++) {
        int value = array[i];
        // 只有当前位置已经为1，标示重复，并且输出，>1情况则不输出了
        if (exists[value] == 1) {
            result.add(value);
        }
        // 用exists标示记录
        exists[value]++;
    }
    return result;
}
```

### 数组
和c语言差不多

数组第N个元素的地址计算：
```
// 第N个元素地址
start_address + item_size * (N - 1)
```

所以数组访问的时间复杂度是O(1)

而且为了方便内存地址的计算，(start_address + item_size * 0)第一个元素的地址

所以起始下标为0

java数据结构ArrayList就是用数组作为底层实现的

#### 插入和删除
+ 尾部插入
直接加  array[size]
+ 中部插入

+ 尾部删除
+ 中部删除

### 排序
+ 冒泡排序
代码：
```
// 冒泡排序
public static void bubbleSort(int[] array) {
    // 1. 每次循环，都能冒泡出剩余元素中最大的元素，因此需要循环 array.length 次
    for (int i = 0; i < array.length; i++) {
        // 2. 每次遍历，只需要遍历 0 到 array.length - i - 1中元素，因此之后的元素都已经是最大的了
        for (int j = 0; j < array.length - i - 1; j++) {
            //3. 交换元素
            if (array[j] > array[j + 1]) {
                int temp = array[j + 1];
                array[j + 1] = array[j];
                array[j] = temp;
            }
        }
    }
}
```
时间复杂度O(n²)
+ 选择排序
重点在选择，每次在剩余数组中选择最大或最小的数，存在数组的一端

核心规则
```
1. 利用两个变量，一个存储当前的最大值，一个存储当前最大值的索引
2. 依次比较后面的元素，如果发现比当前最大值大，则更新最大值，并且更新最大值所在的索引
3. 直到遍历结束，将最大值放在数组的最右边，也就是交换最大值和最右端的元素
4. 重复上述步骤
```
时间复杂度O(n²)

虽然冒泡排序和选择排序的时间复杂度一样，但是实际应用中选择排序的速度比冒泡排序快，因为冒泡排序需要频繁交换数据，但是选择排序一次遍历只需要交换一次

所以真实速度选择排序要快一倍

代码：
```
 public static void selectSort(int[] array) {
    int index=0;
    int max = array[0];
    for (int i=0;i< array.length;i++)
    {
      for (int j=0;j< array.length-i;j++)
      { if (max<array[j]){
          index = j;
          max = array[index];
        }}
    //不判断就会出错
      if(index!= array.length-i-1){
        max = array[array.length-i-1];
        array[array.length-i-1]=array[index];
        array[index]=max;
      }
      else
      {
        index = 0;
        max = array[0];
      }

    }
  }
```

+ 插入排序
重点在插入，每次抽离一个元素当作临时元素，依次比较和移动之后的其他元素，最终将这个临时元素插入都对应的位置

核心规则：
```
1. 在第一轮，抽离数组末尾倒数第二个元素，作为临时元素
2. 用临时元素和数组后面的元素进行对比：如果后面的元素小于临时元素，则后面的元素左移
3. 如果后面的元素大于临时元素，或者已经移动到数组末尾，则将临时元素插入到空隙
4. 重复上述步骤，完成排序
```

时间复杂度
最好的情况：每次比较都不用移动 O(N)
最坏的情况：每次比较都要移动O(N²)

所以排序的选择，如果数组内本身有很多元素是已经有顺序的那么选择插入排序就很方便

```
// 插入排序
  public static void insertSort(int[] array) {
    int temp,j;
    for(int i=0;i< array.length-1;i++)
    {
      temp = array[array.length-2-i];
      for (j= array.length-1-i;j< array.length;j++) {
        if (temp > array[j]) {
          array[j-1]=array[j];
          if (j== array.length-1)
          {
            array[j]=temp;
          }
        } else {
          array[j-1]=temp;
          break;
        }
      }
    }
  }
```

+ 插入排序进阶--二分插入排序

![理解](https://style.youkeda.com/img/course/a1/4/4-1.svg)

寻找插入的位置可用二分查找法
查找插入下标代码：
```
// 查找应该插入的索引位置
public static int searchIndex(int[] array, int left, int right, int aim) {
    // 循环查找节点位置
    while (left < right) {
        int middle = (left + right) / 2;
        int value = array[middle];
        if (value < aim) {
            left = middle + 1;
        } else {
            right = middle - 1;
        }
    }
    // #1. 如果最终元素仍然大于目标元素，则将索引位置往左边移动一个
    if(array[left] > aim){
        return left -1;
    }
    // 否则就是当前位置
    return left;
}
```

所以排序优化：
```
// 插入排序
public static void insertSort(int[] array) {
    // 从倒数第二位开始，遍历到底0位，遍历 N-1 次
    for (int i = array.length - 2; i >= 0; i--) {
        // 存储当前抽离的元素
        int temp = array[i];
        int index = searchIndex(array, i + 1, array.length - 1, temp);

        // #1. 根据插入的索引位置，进行数组的移动和插入
        int j = i + 1;
        while (j <= index) {
            array[j - 1] = array[j];
            j++;
        }
        array[j - 1] = temp;
    }
}
```

### 递归
+ 阶乘：
可简单理解为
f(1) = 1
f(n) = f(n-1) * n
```
public static int factorial(int n) {
    //#1. 当 n = 1 时，递归结束
    if (n == 1) {
        return 1;
    }
    //#2. 把 factorial(n - 1) 的结果和 n 相乘，剩下的交给 factorial(n - 1) 来解决。
    return n * factorial(n - 1);
}
```
就是求f(n)时，假定f(n-1)已知

+ 汉诺塔
#### 如何写递归函数
+ 基准条件
也叫结束条件
+ 递归公式
就是自己和下一个递归函数的直接关系

写递归函数的基本步骤
```
1. 找出基准条件
2. 思考在基准条件下会出现什么情况
3. 思考基准条件的前一步的情况，或函数的执行情况
4. 配合递归公式，继续往前推进
5. 最后实现完善代码
```
+ 斐波那契数列
![fib](https://qgt-document.oss-cn-beijing.aliyuncs.com/PY1/10/%E6%96%90%E6%B3%A2%E9%82%A3%E5%A5%91%E6%95%B0%E5%88%97.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

代码：
```
public static int fibonacci(int n) {
    if(n<=1){
      return n;
    }
    else {
      return fibonacci(n-1)+fibonacci(n-2);
    }
```

将上次的二分插入查找的searchindex部分改为递归函数
```
  public static int searchIndex(int[] array, int left, int right, int aim) {
      int middle = (left + right) / 2;
      int value = array[middle];
      if (left>=right){
        if (array[left]>aim)
        {
          return left-1;
        }
        else return left;
      }
      else{
        if (value<aim)
        {
          return searchIndex(array,middle+1,right,aim);
        }
        else
        {
          return searchIndex(array,left,middle-1,aim);
        }
      }


  }
```

##### 归并排序，分治思想
![分治](https://style.youkeda.com/img/course/a1/5/4-2.svg)
步骤：
```
1. 分：二分法拆分数组，直到每个数组里面只剩一个元素
2. 治：将拆分的数组合并并且在合并的时候保证数组是有序的
分的代码实现：
如何递归进行数组拆分，如何用原始数组创建两个子数组
```


用两个函数表示：
```
// 归并排序，返回排好序的数组
public static int[] mergeSort(int[] array) {
}

// 拷贝原数组的部分内容，从 left 到 right
public static int[] subArray(int[] source, int left, int right) {
}
```

先完成第二个函数：拷贝原数组
```
// 拷贝原数组的部分内容，从 left 到 right
public static int[] subArray(int[] source, int left, int right) {
    // 创建一个新数组
    int[] result = new int[right - left];
    // 依次赋值进去
    for (int i = left; i < right; i++) {
        result[i - left] = source[i];
    }
    return result;
}
```

对mergesort，先找基准条件：数组只有一个就直接返回array；数组元素不止一个就调用拆分

```
// 归并排序，返回排好序的数组
public static int[] mergeSort(int[] array) {
    // 为了方便查看结果，我们将每个数组进行打印
    System.out.println(Arrays.toString(array));
    if (array.length == 1) {
        return array;
    }

    int middle = array.length / 2;
    // #1. 处理 0 到 middle 左侧数组部分
    int[] left = mergeSort(subArray(array, 0, middle));
    // #2. 处理 middle 到 array.length 右侧数组部分
    int[] right = mergeSort(subArray(array, middle, array.length));

    // TODO处理合并问题
    return array;
}
```
这只是拆分的逻辑


![chaifen](https://style.youkeda.com/img/course/a1/5/4-5.svg)

治的代码实现：对于两个已经排好序的数组进行排序
小的相比较然后把较小的放入数组，然后索引移动(被放入的数所在的数组)，再进行比较
第一步：
![diyibu](https://style.youkeda.com/img/course/a1/5/4-3.svg)

第二步：
![dierbu](https://style.youkeda.com/img/course/a1/5/4-4.svg)


时间复杂度和二分法类似，O(Nlog(N)),而且速度稳定

代码实现：
```
public static int[] mergeSort(int[] array) {
    // 为了方便查看结果，我们将每个数组进行打印
    if (array.length == 1) {
      return array;
    }

    int middle = array.length / 2;
    // 处理 0 到 middle 左侧数组部分
    int[] left = mergeSort(subArray(array, 0, middle));
    // 处理 middle 到 array.length 右侧数组部分
    int[] right = mergeSort(subArray(array, middle, array.length));

    // TODO处理合并问题
    int leftindex = 0,rightindex=0,index=0;
    while(index< array.length)
    {
      if (leftindex==left.length||rightindex==right.length)
      {
       if (leftindex==left.length)array[index]=right[rightindex++];
       else array[index]=left[leftindex++];
        index++;
        continue;
      }

      if (left[leftindex]<right[rightindex])
      {
        array[index]=left[leftindex];
        leftindex++;
      }else{
        array[index]=right[rightindex];
        rightindex++;
      }
      index++;
    }
    return array;
  }
```

答案代码：
```

package com.youkeda;

import java.util.Arrays;

public class Sort {

  // 归并排序
  public static int[] mergeSort(int[] array) {
    // 为了方便查看结果，我们将每个数组进行打印
    if (array.length == 1) {
      return array;
    }

    int middle = array.length / 2;
    // 处理 0 到 middle 左侧数组部分
    int[] left = mergeSort(subArray(array, 0, middle));
    // 处理 middle 到 array.length 右侧数组部分
    int[] right = mergeSort(subArray(array, middle, array.length));

    // TODO处理合并问题
    int l = 0;
    int r = 0;
    int index = 0;
    // 依次比较左右两个数组
    while (l < left.length && r < right.length) {
      array[index] = Math.min(left[l], right[r]);
      index++;
      if (left[l] < right[r]) {
        l++;
      } else {
        r++;
      }
    }

    // 右侧数组已经遍历完成，左侧有剩余
    if (l < left.length) {
      for(int i = l; i < left.length; i++){
        array[index] = left[i];
        index++;
      }
    }

    // 左侧数组已经遍历完成，右侧有剩余
    if(r < right.length){
      for(int i = r; i < right.length; i++){
        array[index] = right[i];
        index++;
      }
    }

    return array;
  }

  // 拷贝原数组的部分内容，从 left 到 right
  public static int[] subArray(int[] source, int left, int right) {
    // 创建一个新数组
    int[] result = new int[right - left];
    // 依次赋值进去
    for (int i = left; i < right; i++) {
      result[i - left] = source[i];
    }
    return result;
  }

  public static void main(String[] args) {
    int[] array = {9, 2, 4, 7, 5, 3};
    // Arrays.toString 可以方便打印数组内容
    System.out.println("raw: " + Arrays.toString(array));
    int[] result = mergeSort(array);
    System.out.println("result: " + Arrays.toString(result));
  }
}

```

### 快速排序

+实现原理：
```
分区：快速排序中和归并排序差不多的分解思路，首先随机选择一个数作为轴，目标是将原始数组按照轴进行分区分拆，比轴小的在左边，比轴大的在右边

public static int partition(int[] array, int left, int right) {
      int indexleft = left,indexright = right -1,index;
      int aim = array[right ];
      while(indexleft<indexright){
          if (array[indexleft]>aim&&array[indexright]<aim)
          {
              int temp = array[indexleft];
              array[indexleft]=array[indexright];
              array[indexright]=temp;
              indexleft++;
              indexright--;
          }
          if (indexleft>=indexright){
              break;
          }
          if (array[indexright]>aim)
          {
              indexright--;
          }
          if (array[indexleft]<aim){
              indexleft++;
          }
          if (indexleft>=indexright){
              break;
          }
      }
      if (indexright==right-1){
          index =  right;
      }
      else
          index = indexleft;
      int temp = array[index];
      array[index] = array[right];
      array[right]=temp;

      return index;

  }
```

答案代码：
```
package com.youkeda;

import java.util.Arrays;

public class QuickSort {

  // 快速排序
  public static void quickSort(int[] array) {
    // 调用快速排序的核心，传入left，right
    quickSortCore(array, 0, array.length - 1);
  }

  // 快速排序的核心，同样也是递归函数
  public static void quickSortCore(int[] array, int left, int right) {
    // 递归基准条件，left >= right 即表示数组只有1个或者0个元素。
    if (left >= right) {
      return;
    }
    // 根据轴分区
    int pivotIndex = partition(array, left, right);

    // 递归调用左侧和右侧数组分区
    quickSortCore(array, left, pivotIndex - 1);
    quickSortCore(array, pivotIndex + 1, right);
  }

  // 对数组进行分区，并返回当前轴所在的位置
  public static int partition(int[] array, int left, int right) {
    int pivot = array[right];

    int leftIndex = left;
    int rightIndex = right - 1;
    while (true) {
      // 左指针移动
      while (array[leftIndex] <= pivot && leftIndex < right) {
        leftIndex++;
      }
      // 右指针移动
      while (array[rightIndex] >= pivot && rightIndex > 0) {
        rightIndex--;
      }

      if (leftIndex >= rightIndex) {
        break;
      } else {
        swap(array, leftIndex, rightIndex);
      }
    }

    swap(array, leftIndex, right);
    return leftIndex;
  }

  public static void swap(int[] array, int index1, int index2) {
    int temp = array[index1];
    array[index1] = array[index2];
    array[index2] = temp;
  }

  public static void main(String[] args) {
    int[] array = {9, 2, 4, 7, 5, 3};
    // Arrays.toString 可以方便打印数组内容
    System.out.println("raw: " + Arrays.toString(array));
    quickSort(array);
    System.out.println("result: " + Arrays.toString(array));
  }
}
```


### 快速选择
问题：
找到10个元素的数组中，第4大的元素
![tujie](https://style.youkeda.com/img/course/a1/5/5-7.svg)










