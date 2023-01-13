# 哈希表+回溯
## 方法一(哈希表+回溯)：
### 思路
1. 因为只有小写字母，那么我们可以用哈希表分别统计字符串$word1$与$word2$的字母数量
2. 然后遍历两个哈希表进行比对，具体如下：
   - 我们可以先用两个临时哈希拷贝要遍历的哈希,记为$temp\_hash1$，$temp\_hash2$
   - 然后遍历到的字母，对应的哈希减掉数量，相互增加相应字母数量，即:```--temp_hash1[c1]; ++temp_hash1[c2];--temp_hash2[c2];++temp_hash2[c1];```
   - 如果减掉后数量为$0$，从哈希表中删除
   - 之后判断两个哈希表数量是否相等，如果相等，说明再交换操作后两字符串不同字符数量相等。反之不相等，我们可以回溯刚才的操作，并往下迭代

### 复杂度
- 时间复杂度:
  > $O(m+n)$，$m$为字符串$word1$的大小，$n$为字符串$word2$的大小，遍历两个哈希最大为$O(k^2)$，这里$k$为$26$个小写字母，所以最终不会超过$O(m+n)$
- 空间复杂度:
  > $O(k)$，哈希表最大存储为$26$个小写字母

### Code
```C++ []
class Solution {
public:
    bool isItPossible(string word1, string word2) {
        unordered_map<char, int> um_w1, um_w2;
        for (auto&& c : word1) ++um_w1[c];
        for (auto&& c : word2) ++um_w2[c];
        if (abs((int)um_w1.size() - (int)um_w2.size()) > 2) return false;
        auto temp_hash1 = um_w1;
        auto temp_hash2 = um_w2;
        for (auto& [c1, cnt] : um_w1) {
            for (auto& [c2, cnt] : um_w2) {
                --temp_hash1[c1];
                ++temp_hash1[c2];
                if (temp_hash1[c1] == 0) temp_hash1.erase(c1);
                --temp_hash2[c2];
                ++temp_hash2[c1];
                if (temp_hash2[c2] == 0) temp_hash2.erase(c2);
                if (temp_hash1.size() == temp_hash2.size()) return true;

                //回溯
                ++temp_hash1[c1];
                --temp_hash1[c2];
                if (temp_hash1[c2] == 0) temp_hash1.erase(c2);
                ++temp_hash2[c2];
                --temp_hash2[c1];
                if (temp_hash2[c1] == 0) temp_hash2.erase(c1);
            }
        }
        return false;
    }
};
```