# 模拟
## 方法一(模拟)：
### 思路
1. 空格裁剪出来后判断是否为数字，如果是数字跟前一个数字比较，严格递增继续遍历，反之返回$false$
### 复杂度
- 时间复杂度:
  > $O(n)$，n为字符串s的长度
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    bool areNumbersAscending(string s) {
        int n = s.size(), pre_num = 0;
        string t;
        for (int i = 0;i < n;++i) {
            if (s[i] == ' ') {
                if (isdigit(t[0])) {
                    int now = stoi(t);
                    if (now > pre_num) pre_num = now;
                    else return false;
                }
                t.clear();
            }
            else t += s[i];
        }
        if (!t.empty() && isdigit(t[0])) return stoi(t) > pre_num;
        return true;
    }
};
```