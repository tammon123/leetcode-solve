# 双指针
## 思路
1. 因为前缀后缀要全部相同的才会去掉，那么可以用双指针分别指向开头$(left)$跟结尾$(right)$，如果$left<right\&\&s[left]=s[right]$迭代靠近，反之退出遍历。
2. 在循环内部，我么可以先取$equal_c=s[left]$用来对比，满足$left<=right\&\&s[left]=equal_c$，$left$指针往右移。$right$指针同样迭代往左移，最终返回$right-left+1$
3. 为啥内部循环需要满足$left<=right$，假设最终剩余形如：$"aa"$，$s[left]=s[right]=a$，如果只是满足$left<right$，那么最终$left$只会到$right$位置，这与$"aca"$结果是一样的，结果都为$right-left+1=1$，为了统一条件，我们使得内部循环条件变为$left<=right$，最终$"aa"$得到$left$只会超过$right$一格，那么结果为$0$，$"aca"$结果还是$1$符合预期。
## 复杂度
- 时间复杂度:
  > $O(n)$，n为字符串长度
- 空间复杂度:
  > $O(1)$

## Code
```C++ []
class Solution {
public:
    int minimumLength(string s) {
        int n = s.size(), left = 0, right = n - 1;
        while (left < right && s[left] == s[right]) {
            char equal_c = s[left];
            while (left <= right && s[left] == equal_c) ++left;
            while (left <= right && s[right] == equal_c) --right;
        }
        return right - left + 1;
    }
};
```