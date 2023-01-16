# 递归+回溯+哈希表
## 方法一(递归+回溯+哈希表)：
### 思路
1. 先用哈希表存储所有字母对应的字符可能，然后进行递归回溯操作
2. 回溯维护一个字符串，表示已经有的排列组合，每次从哈希表中取出对应数字的所有字母可能，递归到$n$存入答案中，然后回溯操作

### 复杂度
- 时间复杂度:
  > $O(3^m*4^n)$，$m$为输入数字对应字母个数为$3$的个数，$n$为输入数字对应字母个数为$4$的个数，$m+n$为输入总个数
- 空间复杂度:
  > $O(m+n)$，栈递归深度为$O(m+n)$

### Code
```C++ []
class Solution {
public:
    vector<string> letterCombinations(string digits) {
        if (digits.empty()) return {};
        unordered_map<char, string> um;
        um['2'] = "abc";
        um['3'] = "def";
        um['4'] = "ghi";
        um['5'] = "jkl";
        um['6'] = "mno";
        um['7'] = "pqrs";
        um['8'] = "tuv";
        um['9'] = "wxyz";
        int n = digits.size();
        vector<string> ans;
        string temp;
        auto dfs = [&](int cur, auto&& dfs)->void {
            if (cur == n) {
                ans.emplace_back(temp);
                return;
            }
            for (auto&& c : um[digits[cur]]) {
                temp.push_back(c);
                dfs(cur + 1, dfs);
                temp.pop_back();
            }
        };
        dfs(0, dfs);
        return ans;
    }
};
```