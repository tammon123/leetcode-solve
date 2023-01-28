# 滑动窗口+哈希表
## 方法一(滑动窗口+哈希表)：
### 思路
1. 题目条件```i<j&&nums[i]==nums[j]```表示好子数组。在一个子数组中，相等的数会有$C_{cnt}^2$个好子数组，例如:$[1,1,1,1,1]$有$C_5^2=10$个好子数组。

2. 那么我们可以用一个哈希表来记录数字出现的频率新增的好子组数$good[nums[i]]$。并用一个哈希表$freq$记录数字频率来作为滑动窗口记录数据的出入。具体操作如下：
   - 我们从左到右遍历数组，$freq$记录数字频率，如果当数字频率等于$2$时候，$good[nums[i]]=1$，大于$2$时候$good[nums[i]]+1$，小于$2$继续滑窗。如上面例子当$i=1$时候，$1$出现$2$次，$good[1]=1$。当$i=2$时，$1$出现$3$次，那么$good[1]=1+1=2$。
   - 我们用一个变量$cnt$表示总好子数组数量，那么$cnt+=good[nums[i]]$。如果$cnt>=k$时候，说明当前滑动的窗口满足要求的$k$个好数组了，右边加任何数组都满足，那么答案应加上$n-i$。然后收敛左边界，将左窗口$j$往右滑动，每次减少一个数，我们看滑动窗口是否还满足要求，满足要求继续加上$n-i$，否则停止收敛左边界，继续右边界滑动。

### 复杂度
- 时间复杂度:
  > $O(n)$，$n$为数组$nums$大小
- 空间复杂度:
  > $O(n)$，哈希表$freq$最多存储$n$个数，而哈希表$good$小于$ferq$，所以最终为$O(n)$

### Code
```C++ []
class Solution {
public:
    long long countGood(vector<int>& nums, int k) {
        unordered_map<int, int> good, freq;
        int n = nums.size(), i = 0, j = 0;
        long long cnt = 0, ans = 0;
        for (;i < n;++i) {
            ++freq[nums[i]];
            if (freq[nums[i]] >= 2) {
                good[nums[i]] = freq[nums[i]] == 2 ? 1 : good[nums[i]] + 1;
                cnt += good[nums[i]];
                if (cnt < k) continue;
                ans += n - i;
                while (j < i) {
                    if (freq[nums[j]] < 2) {
                        --freq[nums[j++]];
                        if (cnt >= k) ans += n - i;
                        continue;
                    }
                    --freq[nums[j]];
                    good[nums[j]] <= 1 ? --cnt : cnt -= good[nums[j]];
                    --good[nums[j]];
                    ++j;
                    if (cnt >= k) ans += n - i;
                    else break;
                }
            }
        }

        return ans;
    }
};
```
