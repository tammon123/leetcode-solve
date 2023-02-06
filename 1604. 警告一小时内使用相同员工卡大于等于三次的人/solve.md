# 排序+哈希表
## 方法一(排序+哈希表)：
### 思路
1. 可以用哈希映射，键值存储人名，值存储人对应的打卡时间集合。我们可将时间转化为分钟数存储，方便计算。

2. 一个小时内打卡超过三次就警告，表示每个人的集合中有三个时间戳在一小时内，那么我们可以把时间集合从小到大排序后，遍历时间戳。如果有$time[i]-time[i-2]<=60$，即连续的三个时间戳，第三个减去第一个的时间大于等于$60$，那么表示这个人需要警告，存入答案中。

3. 最终将答案数组排序

### 复杂度
- 时间复杂度:
  > $O(nlogn)$，$n$数组$keyName,keyTime$的大小，存入哈希值需要$O(n)$，遍历哈希与排序需要$O(nlogn)$，最终需要$O(nlogn)$
- 空间复杂度:
  > $O(n)$，哈希表所需存储空间，排序栈空间小于哈希表存储空间

### Code
```C++ []
class Solution {
public:
    vector<string> alertNames(vector<string>& keyName, vector<string>& keyTime) {
        int n = keyName.size();
        unordered_map<string, vector<int>> um;

        for (int i = 0;i < n;++i) {
            string name = keyName[i];
            string time = keyTime[i];
            int hour = (time[0] - '0') * 10 + (time[1] - '0');
            int mins = (time[3] - '0') * 10 + (time[4] - '0');
            um[name].emplace_back(hour * 60 + mins);
        }

        vector<string> ans;
        for (auto&& [name, times] : um) {
            sort(times.begin(), times.end());
            for (int i = 2;i < times.size();++i) {
                int first = times[i - 2], third = times[i];
                if (third - first <= 60) {
                    ans.emplace_back(name);
                    break;
                }
            }
        }
        sort(ans.begin(), ans.end());
        return ans;
    }
};
```