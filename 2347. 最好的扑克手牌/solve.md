# 哈希表
## 方法一(哈希表)：
### 思路
1. 花色与手牌数字相互独立，所以我们用两个哈希表计数。一个按照手牌数字计数，一个按照花色计数。

2. 如果花色哈希大小为$1$，说明是同花，返回`"Flush"`。手牌哈希大小为$5$，为高牌，返回`"High Card"`。手牌哈希值的数量大于等于$3$，为三条，返回`"Three of a Kind"`，否则为对子，返回`"Pair"`。

### 复杂度
- 时间复杂度:
  > $O(n)$，$n$为数组$ranks,suits$的大小
- 空间复杂度:
  > $O(n)$，哈希表需要的存储大小

### Code
```C++ []
class Solution {
public:
    string bestHand(vector<int>& ranks, vector<char>& suits) {
        unordered_map<int, int> um, suitcnt;
        int n = ranks.size();
        for (int i = 0;i < n;++i) {
            ++um[ranks[i]];
            ++suitcnt[suits[i] - 'a'];
        }
        if (suitcnt.size() == 1) return "Flush";
        if (um.size() == 5) return "High Card";
        for (auto& [_, cnt] : um) {
            if (cnt >= 3) return "Three of a Kind";
        }
        return "Pair";
    }
};
```
