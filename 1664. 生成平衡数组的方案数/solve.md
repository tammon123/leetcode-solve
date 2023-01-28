# 奇偶交换
## 方法一(奇偶交换)：
### 思路
1. 每次删除一个数然后查看新的奇偶总和是否相等。假设当前删除的点为数组第$i$个数，$i$前的奇偶总和是不变的，我们只需要考虑$i$后的奇偶性变化，因为删除这点后，后面的所有数都会往前移动一格，原先在偶数位置的就会变成奇数位置，奇数位的会变成偶数位，那$i-1$后的奇偶总和其实是互换了。

2. 那么我们可以先遍历一遍数组，把总的奇偶和计算出来$ori\_even,ori\_odd$。假设当前遍历到$i$点，我们用两个变量记录$i$之前的奇偶总和，初始值为$0$，有：$left\_even=left\_odd=0$。
    - 那么新的奇数总和为:$new\_even=ori\_odd-left\_odd-nums[i]+left\_even$，其中$i$为偶数位(从$0$开始，$i$实际为奇数)，否则$nums[i]=0$
    - 同理新的偶数总和为：$new\_odd=ori\_even-left\_even-nums[i]+left\_odd$，其中$i$为奇数位(从$0$开始，$i$实际为偶数)，否则$nums[i]=0$
    - 最终判断$new\_even=new\_odd$，答案加一

3. 例如：
```
[2,1,6,4]
ori_even=2+6=8, ori_odd=1+4=5, left_odd=left_even=0
i=0
- new_even=ori_odd-left_odd-当前奇数位+left_even=5-0-0+0=5
- new_odd=ori_even-left_even-当前奇数位+left_odd=8-0-2+0=6
- 5!=6, left_even=2,left_odd=0
i=1
- new_even=5-0-1+2=6, new_odd=8-2-0+0=6, 6==6 ans=1 left_even=2, left_odd=1
...
以此类推
```

### 复杂度
- 时间复杂度:
  > $O(n)$，$n$表示数组$nums$的大小
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int waysToMakeFair(vector<int>& nums) {
        long long ori_even = 0, ori_odd = 0, left_even = 0, left_odd = 0;
        int n = nums.size(), ans = 0;
        for (int i = 0;i < n;++i) {
            if (i & 1) ori_odd += nums[i];
            else ori_even += nums[i];
        }
        for (int i = 0;i < n;++i) {
            long long new_even = ori_odd - left_odd - (i & 1 ? nums[i] : 0) + left_even;
            long long new_odd = ori_even - left_even - (i & 1 ? 0 : nums[i]) + left_odd;
            if (new_even == new_odd) ++ans;
            i & 1 ? left_odd += nums[i] : left_even += nums[i];
        }
        return ans;
    }
};
```