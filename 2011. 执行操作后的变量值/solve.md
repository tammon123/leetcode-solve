# 模拟
## 思路
1. 因为只会存在$X++、++X、X--、--X$，所以我们只需要第二个字符就能判断是$+1$还是$-1$

### 复杂度
- 时间复杂度:
  > $O(n)$，n为数组大小
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int finalValueAfterOperations(vector<string>& operations) {
        int ans = 0;
        for (auto&& v : operations) {
            ans += v[1] == '+' ? 1 : -1;
        }
        return ans;
    }
};
```