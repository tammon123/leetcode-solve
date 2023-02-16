# 数论-贝祖定理
## 方法一(数论-贝祖定理)
## 思路
1. 需要贝祖定理知识，[OI Wiki 贝祖定理](https://oi-wiki.org/math/number-theory/bezouts/)

2. 由定理知道，设$a,b$不全为$0$，则存在整数$x,y$，使得$ax+by=gcd(a,b)$。而题目要求有子集满足$ax+by+...=gcd(a,b,...)=1$。我们由$gcd$知道，$gcd(a,1)=1$，所以只要遍历数组求$gcd=1$，那么最终结果一定为$1$
## 复杂度
- 时间复杂度:
  > $O(n+logm)$，$n$为数组$nums$的大小，$m$为数组中最大值，所有值求$gcd$都不会超过$m$，所以最终为$O(logm)$
- 空间复杂度:
  > $O(1)$

## Code
```C++ []
class Solution {
public:
    bool isGoodArray(vector<int>& nums) {
        int l_gcd = nums[0];
        for (int i = 1;i < nums.size();++i) {
            l_gcd = gcd(l_gcd, nums[i]);
        }
        return l_gcd == 1;
    }
};
```