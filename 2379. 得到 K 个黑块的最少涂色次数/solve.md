# 滑动窗口
## 方法一(滑动窗口)：
### 思路
1. 固定长度$k$中考虑翻转白色$W$变成全黑。固定长度就可以考虑滑动窗口，先统计$k$长度中需要替换的$W$数量，那么往后右滑进一个字符，左滑出一个字符，如果为$W$,右滑加一，左滑减一，随后对比此次滑动的大小。最终找出最小的$W$数量就是要求的最少$k$个连续黑块的最少涂色次数。
   
### 复杂度
- 时间复杂度:
  > $O(n)$，$n$为字符串$blocks$的长度
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int minimumRecolors(string blocks, int k) {
        int ans = INT_MAX, cnt = 0, n = blocks.size();
        for (int i = 0;i < k;++i) {
            if (blocks[i] == 'W') ++cnt;
        }
        ans = min(ans, cnt);

        for (int i = k;i < n;++i) {
            if (blocks[i] == 'W') ++cnt;
            if (blocks[i - k] == 'W') --cnt;
            ans = min(ans, cnt);
        }
        return ans;
    }
};
```