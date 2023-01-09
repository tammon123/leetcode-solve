# 模拟
## 方法一(模拟)：
### 思路
1. 直接模拟
### 复杂度
- 时间复杂度:
  > $O(1)$
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    string categorizeBox(int length, int width, int height, int mass) {
        bool is_bulky = false, is_heavy = false;
        if (length >= 10000 || width >= 10000 || height >= 10000 || (long)length * width * height >= 1000000000) is_bulky = true;
        if (mass >= 100) is_heavy = true;
        if (is_bulky && is_heavy) return "Both";
        else if (is_bulky) return "Bulky";
        else if (is_heavy) return "Heavy";
        else return "Neither";
    }
};
```