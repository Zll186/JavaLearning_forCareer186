# 数组part01
## 数组理论知识
数组是存放在连续内存空间上的相同类型数据的集合  
二维数组? c++二维数组内存连续,，地址为16进制；java内存地址不对外暴露,寻址操作完全交给虚拟机。  
## 704.二分查找： https://leetcode.cn/problems/binary-search/description/  
**描述**：这道题目的前提是数组为升序数组，同时题目还强调数组中无重复元素

**目标**: 掌握二分的两种写法,1.左闭右开2.左闭右闭  
```java
//左闭右闭
class Solution {
    public int search(int[] nums, int target) {
        // 避免当 target 小于数组最小值，大于数组最大值时多次循环运算
        if (target < nums[0] || target > nums[nums.length - 1]) {
            return -1;
        }
        int left = 0, right = nums.length - 1;   
        while (left <= right) {
//>> 是Java的右移位运算符，意思是：把(right-left) 的二进制整体向右移动1位。
//直观理解：>> 1 等于 “除以 2”且向下取整
            int mid = left + ((right - left) >> 1);
            if (nums[mid] == target) {
                return mid;
            }
            else if (nums[mid] < target) {
                left = mid + 1;
            }
            else { // nums[mid] > target
                right = mid - 1;
            }
        }
        // 未找到目标值
        return -1;
    }
}
 //左闭右开
class Solution {
    public int search(int[] nums, int target) {
        int left = 0, right = nums.length;   // 关键点1. 右开区间,时刻保证取不到这个值
        while (left < right) {     // 关键点2. 左闭右开,left等于right就说明结束了 例如[1,1) 取不到1
            int mid = left + ((right - left) >> 1);
            if (nums[mid] == target) {  
                return mid;
            }
            else if (nums[mid] < target) {
                left = mid + 1;
            }
            else { // nums[mid] > target  // 关键点3. right取不到mid,答案区间在[left,mid),如果mid-1,那么就会变成[left,mid-1)导致缺少mid-1时的值
                right = mid;
            }
        }
        // 未找到目标值
        return -1;
    }
}

```



## 双指针  
**目标** 学会使用快慢指针和对撞指针  

### 题目: 27.移除元素
**位置:** https://leetcode.cn/problems/remove-element/
```java
// 快慢指针
class Solution {
    public int removeElement(int[] nums, int val) {
        int slowIndex = 0;
        for (int fastIndex = 0; fastIndex < nums.length; fastIndex++) {//慢指针静止,快指针for循环
            if (nums[fastIndex] != val) {
                nums[slowIndex] = nums[fastIndex];
                slowIndex++;
            }
        }
        return slowIndex;
    }
}
//相向双指针法
class Solution {
    public int removeElement(int[] nums, int val) {
        int left = 0;
        int right = nums.length - 1;
        while(right >= 0 && nums[right] == val) right--; //将right移到从右数第一个值不为val的位置
        while(left <= right) {
            if(nums[left] == val) { //left位置的元素需要移除
                //将right位置的元素移到left（覆盖），right位置移除
                nums[left] = nums[right];
                right--;
            }
            left++;
            while(right >= 0 && nums[right] == val) right--;
        }
        return left;
    }
}
```
### 题目:977.有序数组的平方
**位置:** https://leetcode.cn/problems/squares-of-a-sorted-array/
```java
//暴力排序法
class Solution {
    public int[] sortedSquares(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            nums[i] = nums[i] * nums[i];
        }
        Arrays.sort(nums);
        return nums;
    }
}
class Solution {
    public int[] sortedSquares(int[] nums) {
        int right = nums.length - 1;
        int left = 0;
        int[] result = new int[nums.length];
        int index = result.length - 1;
        while (left <= right) {
            if (nums[left] * nums[left] > nums[right] * nums[right]) {
                // 正数的相对位置是不变的， 需要调整的是负数平方后的相对位置
                result[index--] = nums[left] * nums[left];
                ++left;
            } else {
                result[index--] = nums[right] * nums[right];
                --right;
            }
        }
        return result;
    }
}
```


















## little tips

### 时间复杂度：输入规模为n时，算法执行步骤数大概怎么增长。通常用大O表示法写成 O(...)。

- O(1)：常数时间——例：访问数组第 i 个元素 a[i]
- O(log n)：对数时间——例：二分查找
- O(n)：线性时间——例：遍历一遍数组
- O(n log n)：常见高效排序——例：归并排序、堆排序（平均情况）
- O(n^2)：双重循环——例：两层遍历比较（冒泡/选择排序等）
- O(2^n)、O(n!)：指数/阶乘级（非常慢）——例：暴力枚举子集、全排列

怎么估算时间复杂度：
- 忽略常数：O(3n) 记为 O(n)
- 只看最高阶：O(n^2 + n) 记为 O(n^2)
- 循环看次数：一层循环 n → O(n)；两层各 n → O(n^2)
- 递归看分支/深度：比如每次减半 → O(log n)

### 空间复杂度算法运行过程中，额外需要多少内存随 n 增长（不算输入本身占的空间）
- O(1)：只用少量固定变量——例：用几个临时变量交换
- O(n)：额外开一个数组/哈希表/队列等——例：用哈希表统计频次
- O(n)（递归栈）：递归深度是 n 就可能占 O(n) 栈空间——例：链表递归遍历

### >>写法的两个目的：
- **介绍**：>> 是Java的带符号右移位运算符，意思是：把(right-left) 的二进制整体向右移动1位。直观理解：>> 1 等于 “除以 2”且向下取整
- **防止溢出**：不用 left + right，避免两个大数相加超过 int 上限
- 算中间位置**更安全**：标准二分写法

### 后缀自减
result[index--] = nums[left] * nums[left]; 的含义是：
- 先用当前的 index 作为下标，完成赋值：result[index] = nums[left] * nums[left];
- 再把 index 减 1（向左移动一格），为下一次填充做准备。

