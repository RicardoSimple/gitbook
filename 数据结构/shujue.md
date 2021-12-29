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
阶乘：
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











