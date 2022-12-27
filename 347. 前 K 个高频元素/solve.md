# 最小堆+哈希表
## 方法一（最小堆+哈希表）：
## 思路
1. 我们可以先遍历数组用哈希表把数字的频率统计出来
2. 然后构建一个最小堆，遍历哈希表
    - 如果堆大小小于$k$，把数据存入堆中
    - 堆大小大于$k$，跟堆顶元素对比，如果频率比堆顶元素大，弹出堆顶，入堆
3. 完成遍历，堆中元素就为频率最大的$k$个元素
## 复杂度
- 时间复杂度:
  > $O(n+nlogk)$，n为nums的大小，logk为堆排序
- 空间复杂度:
  > $O(n+k)$，哈希表存储数组大小n，堆存储k个元素

## Code
```C++ []
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> freq;
        for (auto&& num : nums) ++freq[num];

        auto cmp = [&](const pair<int, int>& a, const pair<int, int>& b) {
            return a.second > b.second;
        };
        priority_queue <pair<int, int>, vector<pair<int, int>>, decltype(cmp)> pq(cmp);
        for (auto&& [num, cnt] : freq) {
            if (pq.size() == k) {
                if (pq.top().second < cnt) {
                    pq.pop();
                    pq.emplace(num, cnt);
                }
            }
            else pq.emplace(num, cnt);
        }
        vector<int> ans;
        while (!pq.empty()) {
            ans.emplace_back(pq.top().first);
            pq.pop();
        }
        return ans;
    }
};
```
