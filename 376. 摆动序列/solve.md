# 动态规划/贪心
## 思路
### 方法一（动态规划）：
1. 摆动序列的长度由差值正负的长度来决定。我们把最后一个元素呈上升趋势，称到这个元素之前的为最大上升序列，反之最大下降序列。如果差值与之前相等，我们称它为过渡元素。设最大上升序列为$up[i]$，最大下降序列为$down[i]$
2. 假设$nums[i]>nums[i-1]$：
- $up[i]$取决于$up[i-1]$与$down[i-1]+1$中的最大值。因为假如$nums[i-1]$在谷底，那么它代表down[i-1]是最大值，那么$up[i]=down[i-1]+1$；假如$nums[i-1]$是在谷底与谷峰之间，那么$up[i]=up[i-1]$。
- $down[i]$就由$down[i-1]$转移过来，因为没有下降趋势。
3. $nums[i]<nums[i-1]$同理可得
4. $nums[i]==nums[i-1]$，那么$up[i]$跟$down[i]$就由$up[i-1]$跟$down[i-1]$转移而得。
5. 注意边界条件：只含有一个元素的时候也视为摆动序列，那么$up[0]=down[0]=1$， 最长摆动序列为$max(up[n-1], down[n-1])$

### 复杂度
- 时间复杂度:
  > $O(n)$
- 空间复杂度:
  > $O(n)$

### Code
```C++ []
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        int n = nums.size();
        vector<int> up(n), down(n);
        up[0] = down[0] = 1;
        for (int i = 1;i < n;++i) {
            if (nums[i] < nums[i - 1]) {
                up[i] = up[i - 1];
                down[i] = max(down[i - 1], up[i - 1] + 1);
            }
            else if (nums[i] > nums[i - 1]) {
                up[i] = max(up[i - 1], down[i - 1] + 1);
                down[i] = down[i - 1];
            }
            else {
                up[i] = up[i - 1];
                down[i] = down[i - 1];
            }
        }
        return max(up[n - 1], down[n - 1]);
    }
};
```
### 方法二（滚动数组压缩动态规划）：
1. 由于当前状态只有上一个状态转移，那么用两个变量$up,down$进行状态转移
2. 我们可以发现，在转移过程中，峰到谷或谷到峰才会转移，转移状态不会超过$1$，所以$down=up+1, up=down+1$
### 复杂度
- 时间复杂度:
  > $O(n)$
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        int n = nums.size(), up = 1, down = 1;
        for (int i = 1;i < n;++i) {
            if (nums[i] < nums[i - 1]) down = up + 1;
            else if (nums[i] > nums[i - 1]) up = down + 1;
        }
        return max(up, down);
    }
};
```
### 方法三（贪心）：
1. 由方法二我们知道，过程只存在峰谷之间转换，那么我们只需要统计峰谷值就行
2. 对比过程中注意要出现上升或下降趋势才能答案$+1$
### 复杂度
- 时间复杂度:
  > $O(n)$
- 空间复杂度:
  > $O(1)$
### Code
```C++ []
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        int n = nums.size();
        if (n < 2) return n;
        int prediff = nums[1] - nums[0], ans = prediff != 0 ? 2 : 1;
        for (int i = 2;i < n;++i) {
            int diff = nums[i] - nums[i - 1];
            if ((diff > 0 && prediff <= 0) || (diff < 0 && prediff >= 0)) {
                ++ans;
                prediff = diff;
            }
        }
        return ans;
    }
};
```