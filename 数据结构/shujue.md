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






