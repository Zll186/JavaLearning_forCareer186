<img width="558" height="432" alt="image" src="https://github.com/user-attachments/assets/11468c06-b452-4ca7-8cca-34c0e339e418" /># 数组part02
## 滑动窗口  
### 题目: 209.长度最小子数组
**位置:** https://leetcode.cn/problems/minimum-size-subarray-sum/  
思路：右指针扩，左指针缩
```java
// 滑动窗口
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int left = 0;//滑动窗口起始位置
        int sum = 0;// 滑动窗口数值之和
        int result = Integer.MAX_VALUE;//记录答案——最短长度，用一个很大的数当“尚未找到”的标记。之后每次找到满足条件的窗口就更新最短长度。
        for (int right = 0; right < nums.length; right++) {    //（滑动窗口终止位置）右指针向右扩张窗口
            sum += nums[right];
            // 注意这里使用while，每次更新 left，并不断比较子序列是否符合条件
            while (sum >= s) {  //一旦 sum 达到目标，说明当前窗口是“可行解”。但我们想要最短，所以要尽可能把左边往右缩
                //先更新result最小值。
                result = Math.min(result, right - left + 1);
                //然后缩小窗口，先把左端元素移出窗口，再左指针右移一格
                sum -= nums[left++];
            }
        }
        return result == Integer.MAX_VALUE ? 0 : result;
    }
} 
```
### 题目: 59.螺旋矩阵II  
**位置:** https://leetcode.cn/problems/spiral-matrix-ii/description/  
思路：**循环不变量**——左闭右开。从左至右，左闭右开；从上至下，左闭右开；从右至左，左闭右开；从下至上，左闭右开；
    大while套四个小for
```java
class Solution {
    public int[][] generateMatrix(int n) {
        int[][] nums = new int[n][n];
        int startX = 0, startY = 0;  // 每一圈的起始行、列
        int i, j; // 二维数组行、列下标
        int offset = 1;//用来控制“右边界/下边界”
        int count = 1;  // 矩阵中需要填写的数字
        int loop = 1; // 记录当前的圈数

        //while循环：只要圈数没做完就继续。
        //为什么是 n/2——偶数 n：正好做完所有圈——奇数 n：做完外面几圈后，剩中心 1 个格子，后面单独填
        while (loop <= n / 2) {
            // 顶部  // 左闭右开，不填右上角那个点，所以判断循环结束时——j 不能等于 n - offset
            for (j = startY; j < n - offset; j++) {
                //i不变，j变
                nums[startX][j] = count++;
            }

            // 右列  // 左闭右开，不填右下角，所以判断循环结束时—— i 不能等于 n - offset
            for (i = startX; i < n - offset; i++) {
                //i变，j不变
                nums[i][j] = count++;
            }

            // 底部
            // 左闭右开，所以判断循环结束时， j != startY
            for (; j > startY; j--) {
                //i不变，j变
                nums[i][j] = count++;
            }

            // 左列
            // 左闭右开，所以判断循环结束时， i != startX
            for (; i > startX; i--) {
                //i变，j不变
                nums[i][j] = count++;
            }
            startX++;
            startY++;
            offset++;
            loop++;
        }
        if (n % 2 == 1) { // n 为奇数时，单独处理矩阵中心的值
            nums[startX][startY] = count;
        }
        return nums;
    }
}

```
### 题目: 54.螺旋矩阵 
**位置:** https://leetcode.cn/problems/spiral-matrix/description/?envType=study-plan-v2&envId=top-100-liked
```java
// 与 59.螺旋矩阵II解法完全一致
class Solution {
    private final int[][] DIRS = {{0,1},{1,0},{0,-1},{-1,0}};
    public List<Integer> spiralOrder(int[][] matrix) {
        int m= matrix.length;
        int n = matrix[0].length;
        int size =m*n;
        List<Integer> ans =new ArrayList<>(size);
        int i=0;
        int j=-1;
        for(int dis =0;ans.size()<size;dis=(dis+1)%4){
            for(int k=0;k<n;k++){
                i+=DIRS[dis][0];
                j+=DIRS[dis][1];
                ans.add(matrix[i][j]);
            }
            int tmp =n;
            n=m-1;
            m = tmp; 
        }
        return ans;
    }
}
```
## 前缀和  
**题目: 58.区间和**  
**位置:** https://kamacoder.com/problempage.php?pid=1070  
思路：高中数学的前n项和
```java
//有显式数组
import java.util.Scanner;//引入 Scanner，用于从标准输入读数据。

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);//scanner用来读入数字

        int n = scanner.nextInt();//读数组长度 n
        int[] vec = new int[n];//vec：存原数组
        int[] p = new int[n];//p：存前缀和数组，p[i] 表示从 0 到 i 的累加和。

        int presum = 0;//滚动累加变量
        for (int i = 0; i < n; i++) {
            vec[i] = scanner.nextInt();//读入第 i 个元素
            presum += vec[i];//把它加进累计和
            p[i] = presum;//把“到 i 为止的和”存进 p[i]
        }

        while (scanner.hasNextInt()) {//不断读取区间并输出答案
            int a = scanner.nextInt();
            int b = scanner.nextInt();//读区间左端点 a 和右端点 b

            int sum;
            if (a == 0) {
                sum = p[b];
            } else {
                sum = p[b] - p[a - 1];
            }
            System.out.println(sum);//输出这个区间和
        }

        scanner.close();//关闭输入流（习惯写法）
    }
}
//无显式数组
import java.util.Scanner;
class Main{
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int[] pre = new int[n+1];//pre[i] 表示：前 i 个元素的和，这样写不需要判定a=0的情况
        for(int i=1;i<=n;i++){
            pre[i]=pre[i-1]+sc.nextInt();
        }
        while(sc.hasNextInt()){
            int a=sc.nextInt();
            int b = sc.nextInt();

            int sum=pre[b+1]-pre[a];
            System.out.println(sum);
        }
        sc.close();
    }
}
```
**题目: 44.开发商购买土地**  
**位置:** https://kamacoder.com/problempage.php?pid=1044  
思路：枚举所有“横切一刀”或“竖切一刀”的分法，算两边总和差的最小值。
```java
import java.util.Scanner;
public class Main{
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int m = sc.nextInt();
        int[][] matrix = new int[n][m];
        int sum=0; 
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                matrix[i][j]=sc.nextInt();//matrix读入每个格子，保存每个区块价值
                sum+=matrix[i][j];//保存全区域总价值
            }
        }
        int ans = Integer.MAX_VALUE;
        // 行分割——每扫完一整行，就把“以当前行 为分界”的切法算一次。
        int count=0;
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                count+=matrix[i][j];//按行从上往下累加，把已经扫过的“上半部分”加进 count。
                if(j==m-1){//// 遍历到行末尾时候开始统计
                    ans=Math.min(ans,Math.abs(sum-count-count)); //总和是 sum，切一刀后其中一边的和是 count，另一边就是 sum - count，两块之差为|sum-2*count|
                }
            }
        }

        // 列分割
        count=0;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                count+=matrix[j][i];
                if(j==n-1){
                    ans=Math.min(ans,Math.abs(sum-count-count));
                }
            }
        }
        System.out.println(ans);
    }
}
```
## 数组总结
### 题型: 二分法,双指针,滑动窗口,前缀和,二维数组  
#### 二分法
两种写法,1.左闭右开 2.左开右开
#### 双指针  
**1. 快慢指针**   
**1.1** 快指针在for内循环,慢指针根据条件移动  
**1.2** 链表中常用到 slow=slow.next fast=fast.next.next这种移动模式  
**2.碰撞指针**   
while循环内left右移,right左移  
#### 滑动窗口  
**主要用于单调的序列**  
**1.定长窗口**  
一般可以不定义left,直接用right-len+1等表示    
可以根据这个right-len+1是否小于零来判断此时整个窗口都进入数组了没有  
**2.变长窗口**  
得定义left,根据条件手动调整left    
#### 前缀和  
前缀和的作用主要就在于可以将计算**连续序列的和**转换为计算**任意位置的差**  
#### 二维数组  
二维数组一般都需要定义DIRS这个方向二维数组,多数情况下都能够简化代码   
还得注意外层是循环遍历方向  


## little tips
### Integer.MAX_VALUE
- int result = Integer.MAX_VALUE;————用一个很大的数当“尚未找到”的标记。之后每次找到满足条件的窗口就更新最短长度。
### 什么时候用 new？
- 1）创建数组
  - int[] a = new int[10];
  - int[][] b = new int[3][4];
  - String[] s = new String[5];
- 2）创建对象
  - Person p = new Person();
  - ArrayList<Integer> list = new ArrayList<>();
  - HashMap<String,Integer> map = new HashMap<>();
- 3）创建对象数组
  - Person[] ps = new Person[3]; // 创建了长度为3的数组，但里面每个元素默认是 null
    ps[0] = new Person();        // 这里才真正创建第0个 Person 对
### Math.abs()
- 求绝对值
### ACM写法的一般开头和结尾
```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        System.out.println(result);
        scanner.close();
    }
}
```
### ACM写法注意
- if判断是==
- Math.min Math.max Math.abs，都要带前缀
