# 哈希表+贪心
## 思路
1. 同一个字母只能出现在一个片段中，尽可能的多分片段。我们可以用贪心的思想，从左往右遍历，先用哈希表记录每个字母最远的位置。
2. 然后再从左往右遍历，遍历到的字母得到最远距离，如果往后的所有字母的最远距离都在这个距离内，那么就可以分段。往后遍历如果对应字母最远距离大于记录的最远距离，就更新最远距离。
3. 记录一个开始位置$pre$，一个最远距离$mostPos$，能分段，就把长度$mostPost-pre+1$存入答案中，并让$pre=i+1$

## 复杂度
- 时间复杂度:
  > $O(n)$，n为字符串s的长度
- 空间复杂度:
  > $O(n)$

## Code
```C++ []
class Solution {
public:
    vector<int> partitionLabels(string s) {
        int n = s.size(), pre = 0, mostPos = 0;
        unordered_map<char, int> um;
        for (int i = 0;i < n;++i) um[s[i]] = i;

        vector<int> ans;
        for (int i = 0;i < n;++i) {
            mostPos = max(mostPos, um[s[i]]);
            if (i == mostPos) {
                ans.emplace_back(mostPos - pre + 1);
                pre = i + 1;
            }
        }
        return ans;
    }
};
```