# 竖式乘法
## 方法一(竖式乘法)：
### 思路
1. 类似[415题 字符串相加](https://leetcode.cn/problems/add-strings/)，采用竖式方式。如果$num1、num2$中任意一个为$"0"$，直接返回$"0"$。
2. 从右往左遍历，$num2$每一项都乘以$num1$的每一项，超$10$进位，最终进位不为零，把最后进位数加上，得到字符串，反转后为一项乘积后的结果。然后每一项都进行**字符串相加**操作，得到最终结果。
3. 注意在每一位相乘时，注意补$"0"$，
例如：
```
    123
x   456
---------
    738
   6150     "5"x"123", "5"位于十号位，要补1个"0"
  49200
---------
  56088 
```
### 复杂度
- 时间复杂度:
  > $O(mn+n^2)$，m为字符串num1长度，n为字符串num2的长度，字符串相加有n次操作，操作最长的字符串为m+n
- 空间复杂度:
  > $O(m+n)$，存储中间状态，最长字符串为m+n

### Code
```C++ []
class Solution {
public:
    string addStrings(string num1, string num2)
    {
        string ans;
        int n = num1.size() - 1, m = num2.size() - 1, add = 0;
        while (n >= 0 || m >= 0 || add != 0) {
            int x = n >= 0 ? num1[n--] - '0' : 0;
            int y = m >= 0 ? num2[m--] - '0' : 0;
            int r = x + y + add;
            ans += '0' + r % 10;
            add = r / 10;
        }
        reverse(ans.begin(), ans.end());
        return ans;
    }

    string multiply(string num1, string num2) {
        if (num1 == "0" || num2 == "0") return "0";
        int m = num1.size(), n = num2.size();
        string ans = "0";
        for (int i = n - 1;i >= 0;--i) {
            string str;
            for (int j = n - 1;j > i;--j) {
                str.push_back('0');
            }

            int x = num2[i] - '0', add = 0;
            for (int j = m - 1;j >= 0;--j) {
                int y = num1[j] - '0';
                int r = x * y + add;
                str.push_back('0' + r % 10);
                add = r / 10;
            }
            while (add != 0) {
                str.push_back('0' + add % 10);
                add /= 10;
            }
            reverse(str.begin(), str.end());
            ans = addStrings(ans, str);
        }
        return ans;
    }
};
```
## 方法二(乘法)：
### 思路
1. 从右往左相乘，用数组来存储每一位相乘的结果。
2. 设$m、n$分别为$num1、num2$的长度，那么数组最多存储$m+n$位。因为$num1$最大为$10^m-1$，$num2$最大为$10^n-1$，所以$num1*num2=(10^m-1)*(10^n-1)$，结果不会超过$10^{m+n}$，所以最大乘积长度为$m+n$
3. 对于$0<=i<m$与$0<=j<n$，$num1[i]*num2[j]$的值存入数组$[i+j+1]$中，进位存入数组$[i+j]$中

### 复杂度
- 时间复杂度:
  > $O(mn)$，m为字符串num1长度，n为字符串num2的长度
- 空间复杂度:
  > $O(m+n)$，创建长度m+n的数组

### Code
```C++ []
class Solution {
public:
    string multiply(string num1, string num2) {
        if (num1 == "0" || num2 == "0") return "0";
        int m = num1.size(), n = num2.size();
        vector<int> check(m + n);
        for (int i = m - 1;i >= 0;--i) {
            int x = num1[i] - '0';
            for (int j = n - 1;j >= 0;--j) {
                int y = num2[j] - '0';
                check[i + j + 1] += x * y;
            }
        }

        for (int i = m + n - 1;i > 0;--i) {
            check[i - 1] += check[i] / 10;
            check[i] %= 10;
        }
        int index = check[0] == 0 ? 1 : 0;
        string ans;
        while (index < (m + n)) {
            ans.push_back('0' + check[index]);
            ++index;
        }
        return ans;
    }
};
```