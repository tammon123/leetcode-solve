# 前缀和+哈希表/前缀和+单调栈
## 方法一(前缀和+哈希表)：
### 思路
1. 当前数如果大于$8$那么他对于最长时间段子串之一贡献$1$，小于$8$就贡献$-1$，我们计算出所有贡献值的前缀和。那么最终就是求最长的区间和大于$0$的子串长度。
   
2. 如果当前前缀和$s[i]>0$，那么表示长度为$i+1$需要统计答案中($i$从$0$开始)，如果$s[i]<0$，那么我们要找到$s[j...i]$区间和大于$0$，即:$s[i]-s[j]>0$，我们可用哈希表统计之前是否出现过$s[j]=s[i]-1$

### 复杂度
- 时间复杂度:
  > $O(n)$，$n$为数组$hours$的大小
- 空间复杂度:
  > $O(n)$，哈希表需要存储的数据大小

### Code
```C++ []
class Solution {
public:
    int longestWPI(vector<int>& hours) {
        int n = hours.size(), ans = 0, sum = 0;
        unordered_map<int, int> um;
        for (int i = 0;i < n;++i) {
            sum += (hours[i] > 8 ? 1 : -1);
            if (sum > 0) {
                ans = max(ans, i + 1);
            }
            else {
                if (um.count(sum - 1)) {
                    ans = max(ans, i - um[sum - 1]);
                }
            }

            if (!um.count(sum)) {
                um[sum] = i;
            }
        }

        return ans;
    }
};
```