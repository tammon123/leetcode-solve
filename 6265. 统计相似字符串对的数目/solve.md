# 位运算匹配
## 思路
1. 相似就是包含的字母相同，我们可以通过位运算，把$words$进行转化，然后再对比

## 复杂度
- 时间复杂度:
  > $O(nk+n^2)$，n为words的长度，k单个字符串长度
- 空间复杂度:
  > $O(n)$

## Code
```C++ []
class Solution {
public:
    int similarPairs(vector<string>& words) {
        int n = words.size();
        vector<int> masks(n);
        for (int i = 0;i < n;++i) {
            int mask = 0;
            for (auto&& c : words[i]) {
                mask |= (1 << c - 'a');
            }
            masks[i] = mask;
        }
        int ans = 0;
        for (int i = 0;i < n;++i) {
            for (int j = i + 1;j < n;++j) {
                if (masks[i] == masks[j]) ++ans;
            }
        }
        return ans;
    }
};
```