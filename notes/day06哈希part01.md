# 哈希表part01  
## 哈希表  
哈希表就是通过hash函数来定位索引的数组  
## hash函数  
hash函数就是通过hashcode将由属性值转化为数值,然后映射到到某个索引处  
但是因为是由大范围映射到小范围(数组长度),所以会有hash冲突  
## hash冲突  
<img width="965" height="702" alt="image" src="https://github.com/user-attachments/assets/895a044d-3950-4c3e-b178-4512be93d9c7" />　　
解决hash冲突的方法有　　
1. 拉链法　　
索引后跟链表　　
2. 线性探测法　　
当前位置已经存在元素就向下移动直到找到空位置;
## 常见使用hash表的数据结构
map,set
**题目: 242.有效字母异位词**   
**位置:** https://leetcode.cn/problems/valid-anagram/description/  
```java
class Solution {
    public boolean isAnagram(String s, String t) {
        if(s.length()!=t.length()) return false;
        char[] c1 = s.toCharArray();
        char[] c2 = t.toCharArray();
        Arrays.sort(c1);
        Arrays.sort(c2);
        for(int i=0;i<c1.length;i++){
            if(c1[i]!=c2[i]) return false;
        }
        return true;
    }
}
```  
**题目: 349.两个数组的交集**   
**位置:** https://leetcode.cn/problems/intersection-of-two-arrays/description/
```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        Set<Integer> set = new HashSet<>();
        Set<Integer> ans = new HashSet<>();
        for(int i=0;i<nums1.length;i++){
            set.add(nums1[i]);

        }
        for(int i=0;i<nums2.length;i++){
            if(set.contains(nums2[i])){
                ans.add(nums2[i]);
            }
        }
        return ans.stream().mapToInt(Integer::intValue).toArray();
        /**
         * 将 Set<Integer> 转换为 int[] 数组：
         * 1. stream() : Collection 接口的方法，将集合转换为 Stream<Integer>
         * 2. mapToInt(Integer::intValue) : 
         *    - 中间操作，将 Stream<Integer> 转换为 IntStream
         *    - 使用方法引用 Integer::intValue，将 Integer 对象拆箱为 int 基本类型
         * 3. toArray() : 终端操作，将 IntStream 转换为 int[] 数组。
         */
    }
}
```  
**题目: 202.快乐数**   
**位置:** https://leetcode.cn/problems/happy-number/description/
```java
class Solution {
    public boolean isHappy(int n) {
        Set<Integer> record = new HashSet<>();
        while (n != 1 && !record.contains(n)) {
            record.add(n);
            n = getNextNumber(n);
        }
        return n == 1;
    }

    private int getNextNumber(int n) {
        int res = 0;
        while (n > 0) { // 关键点: 123 先%10 ,拿到3 然后/10 就变成了12
            int temp = n % 10;
            res += temp * temp;
            n = n / 10;
        }
        return res;
    }
}
```
**题目: 1.两数之和**   
**位置:** https://leetcode.cn/problems/two-sum/description/
```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer,Integer> hashmap = new HashMap<>();
        for(int i=0;i<nums.length;i++){
            if(hashmap.containsKey(target-nums[i])){
                return new int[]{hashmap.get(target-nums[i]),i};
            }
            hashmap.put(nums[i],i);
        }
        return null;
    }
}
```
