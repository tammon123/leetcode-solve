# 哈希表/哈希表+位运算
## 方法一（哈希表）：
### 思路
1. 只要数字存在任意两个数组中就行，我们可以先遍历三个数组分别存入三个不同哈希表跟一个全部数字的哈希表
2. 然后遍历全部数字的哈希表，判断再任意两个哈希表中就存入答案中
### 复杂度
- 时间复杂度:
  > $O(n)$，n为遍历三个数组的时间复杂度，后有遍历全部数字的，但不会超过三个数组之和
- 空间复杂度:
  > $O(n)$，n为三个数组不同数字数量之和

### Code
```C++ []
class Solution {
public:
    vector<int> twoOutOfThree(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3) {
        unordered_set<int> us1, us2, us3, temp;
        for (auto&& x : nums1) us1.emplace(x), temp.emplace(x);
        for (auto&& x : nums2) us2.emplace(x), temp.emplace(x);
        for (auto&& x : nums3) us3.emplace(x), temp.emplace(x);

        vector<int> ans;
        for (auto&& x : temp) {
            if ((us1.count(x) && us2.count(x)) || (us1.count(x) && us3.count(x)) ||
            (us2.count(x) && us3.count(x)))
                ans.emplace_back(x);
        }

        return ans;
    }
};
```
## 方法二（哈希表+位运算）：
### 思路
1. 既然只有三个数组，那么我们键值存数字，值存$mask$用二进制表示，例如：数字$1$再三个数组中都存在那么二进制表现为$111$，即$hash[1]=7$
2. 之后再遍历哈希表，运用$x\&(x-1)$，消除末位$1$的办法，如果值大于$0$，说明数字再两个数组内以上，不然只存在一个数组当中。
### 复杂度
- 时间复杂度:
  > $O(n)$，n为遍历三个数组的时间复杂度，后有遍历全部数字的，但不会超过三个数组之和
- 空间复杂度:
  > $O(n)$，n为三个数组不同数字数量之和

### Code
```C++ []
class Solution {
public:
    vector<int> twoOutOfThree(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3) {
        unordered_map<int, int> um;
        for (auto&& x : nums1) um[x] = 1;
        for (auto&& x : nums2) um[x] |= 2;
        for (auto&& x : nums3) um[x] |= 4;

        vector<int> ans;
        for (auto&& [x, mask] : um) {
            if (mask & (mask - 1)) ans.emplace_back(x);
        }
        return ans;
    }
};
```
