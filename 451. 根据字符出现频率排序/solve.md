# 最大堆+哈希表/桶排序+哈希表
## 方法一（最大堆+哈希表）：
### 思路
1. 可用哈希表记录字母出现频率，然后建立最大堆对字母频率排序，最后根据最大堆构建新字符串
### 复杂度
- 时间复杂度:
  > $O(n+klogk)$，n为字符串长度，k为不同字符数，logk为堆排序
- 空间复杂度:
  > $O(n+k)$，n为哈希表，k为堆大小

### Code
```C++ []
class Solution {
public:
    string frequencySort(string s) {
        unordered_map<char, int> freq;
        for (auto&& c : s) ++freq[c];

        auto cmp = [&](const pair<char, int>& a, const pair<char, int>& b) {return a.second < b.second;};
        priority_queue<pair<char, int>, vector<pair<char, int>>, decltype(cmp)> pq(cmp);
        for (auto& [c, cnt] : freq) pq.emplace(c, cnt);

        string ans;
        while (!pq.empty()) {
            auto [c, cnt] = pq.top(); pq.pop();
            ans += string(cnt, c);
        }
        return ans;
    }
};
```
## 方法二（桶排序+哈希表）：
### 思路
1. 可用哈希表记录字母出现频率，并记录最大频率$maxFreq$
2. 然后可以用桶排序思想，按照频率把对应频率的字符存入相应频率桶中
3. 最后从大到小的对桶中的字符进行构建新字符串
### 复杂度
- 时间复杂度:
  > $O(n+k)$，n为字符串长度，k为不同字符数
- 空间复杂度:
  > $O(n+k)$，n为哈希表，k为桶的大小

### Code
```C++ []
class Solution {
public:
    string frequencySort(string s) {
        unordered_map<char, int> freq;
        int maxFreq = 0;
        for (auto&& c : s) maxFreq = max(maxFreq, ++freq[c]);

        vector<string> buckets(maxFreq + 1);
        for (auto& [c, cnt] : freq) buckets[cnt].push_back(c);

        string ans;
        for (int i = maxFreq;i > 0;--i) {
            string& str = buckets[i];
            for (auto& ch : str) {
                ans += string(i, ch);
            }
        }
        return ans;
    }
};
```