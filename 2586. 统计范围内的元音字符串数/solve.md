# 模拟
## 方法一(模拟)：
### 思路
1. 直接模拟

### 复杂度
- 时间复杂度:
  > $O(n)$，$n$为数组长度，$right-left+1$最多为$n$
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int vowelStrings(vector<string>& words, int left, int right) {
        int ans = 0;
        auto check = [&](char a)->bool {
            if (a == 'a' || a == 'o' || a == 'e' || a == 'i' || a == 'u') return true;
            return false;
        };
        for (int i = left;i <= right;++i) {
            string x = words[i];
            if (check(x[0]) && check(x[x.size() - 1])) ++ans;
        }
        return ans;
    }
};
```