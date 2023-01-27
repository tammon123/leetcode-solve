# 位移/Brian Kernighan 算法
## 方法一(位移)：
### 思路
1. 两个数按位与操作，某一位只要为$0$最终那位就是$0$。

2. 假设$left$与$right$对齐后二进制字符串中，前$i$位相等。因为是连续的数字，$i+1$后，总会有数字把相应的二进制位填为$0$，所以最终我们只需要找到公共前缀第$i$位，然后再后面填上$0$。

3. 那我们同时向右位移直到$left=right$，说明找到二进制的公共前缀了。我们在右移过程中记录下右移次数$shift$，最终返回$left<<shift$
### 复杂度
- 时间复杂度:
  > $O(log(right))$，因为$left<=right$，取决于$right$的二进制数
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int rangeBitwiseAnd(int left, int right) {
        int shift = 0;
        while (left != right) {
            left >>= 1;
            right >>= 1;
            ++shift;
        }
        return right << shift;
    }
};
```

## 方法二(Brian Kernighan 算法)：
### 思路
1. 计算公共前缀可以用$Brian Kernighan$算法。$right$每次**与**上$right-1$将右边的$1$置为$0$，直到$left>=right$为止，返回$right$

### 复杂度
- 时间复杂度:
  > $O(log(right))$，与位移一样同样取决于$right$的二进制数，但是因为每次是直接跳到下一个二进制$1$的位置，远小于位移时间复杂度
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int rangeBitwiseAnd(int left, int right) {
        while (left < right) {
            right &= (right - 1);
        }
        return right;
    }
};
```