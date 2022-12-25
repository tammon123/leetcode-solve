# 一次遍历
## 思路
1. 我们只需要考虑$1$跟$-1$之间的距离，求出最大距离就行
    - 可以用两个变量存储上一个数$pre$为$1$或$-1$，位置$pre\_index$
    - 从左到右遍历，如果遍历到当前的$forts[i]=1$，判断$pre=-1$，我们可以求出距离$ans=max(ans, i-pre\_index-1）$注意距离减一才是$0$的数量，然后更新$pre$与$pre\_index$
    - 我们开始设$ans=INT\_MIN$，假如数组中只有$1$或者只有$-1$，结束后$ans$应该为$0$
## 复杂度
- 时间复杂度:
  > $O(n)$，n为数组长度
- 空间复杂度:
  > $O(1)$

## Code
```C++ []
class Solution {
public:
    int captureForts(vector<int>& forts) {
        int pre = forts[0], pre_index = 0, ans = INT_MIN;
        for (int i = 1;i < forts.size();++i) {
            if (forts[i] == 1) {
                if (pre == -1) ans = max(ans, i - pre_index - 1);
                pre = 1; pre_index = i;
            }
            else if (forts[i] == -1) {
                if (pre == 1) ans = max(ans, i - pre_index - 1);
                pre = -1; pre_index = i;
            }
        }
        return ans == INT_MIN ? 0 : ans;
    }
};
```