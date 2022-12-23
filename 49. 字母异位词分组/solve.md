# 排序+哈希表/计数+哈希表
## 方法一(排序+哈希表)：
### 思路
1. 每个单词中对应字母频率一样就认为是字母异位词，那只需要把每个字母排序，用哈希表的$key$存储排序后的字母，对应排序前的字母存入哈希表的$value$中就分好类了。

### 复杂度
- 时间复杂度:
  > $O(nklogk)$，n为strs数量，k为strs中字符串的最大长度，klogk为排序时间复杂度
- 空间复杂度:
  > $O(nk)$，哈希表存储全部字符串

### Code
```C++ []
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> um;
        for (auto&& s : strs) {
            string str = s;
            sort(str.begin(), str.end());
            um[str].emplace_back(s);
        }

        vector<vector<string>> ans;
        for (auto&& [_, v] : um) {
            ans.emplace_back(v);
        }
        return ans;
    }
};
```
## 方法二(计数+哈希表)：
### 思路
1. 因为只有小写字母，我们可以用数组将字母的频率统计出来作为哈希表的键值，然后重写编写哈希映射函数

### 复杂度
- 时间复杂度:
  > $O(n(k+|Σ|))$，n为strs数量，k为strs中字符串的最大长度，|Σ|这里为26
- 空间复杂度:
  > $O(n(k+|Σ|))$，哈希表存储全部字符串

### Code
```C++ []
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        auto myhash = [&](const array<int, 26>& arr) -> size_t {
            return accumulate(arr.begin(), arr.end(), 0u, [&](int a, int num) {
                return (a << 1) ^ hash<int>()(num);
            });
        };

        unordered_map<array<int, 26>, vector<string>, decltype(myhash)> um(0, myhash);
        for (auto&& s : strs) {
            array<int, 26> cnt{};
            for (auto&& c : s) ++cnt[c - 'a'];
            um[cnt].emplace_back(s);
        }

        vector<vector<string>> ans;
        for (auto&& [_, v] : um) {
            ans.emplace_back(v);
        }
        return ans;
    }
};
```