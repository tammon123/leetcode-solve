# 优先队列
## 方法一(优先队列)：
### 思路
1. 根据订单类型匹配：如果是$buy$订单就找价格最低的$sell$订单，如果是$sell$订单就找价格最高的$buy$订单，那么我么可以用两个堆，小顶堆存$sell$订单，大顶堆存$buy$订单，存入对值（价格，数量）
2. 最后根据剩余的两堆值计算结果
### 复杂度
- 时间复杂度:
  > $O(nlogn)$，n为orders数量，堆每个处理时间为O(logn)
- 空间复杂度:
  > $O(n)$，堆存储orders大小

### Code
```C++ []
class Solution {
public:
    using pII = pair<int, int>;
    static const int MOD = 1e9 + 7;

    int getNumberOfBacklogOrders(vector<vector<int>>& orders) {
        priority_queue<pII, vector<pII>, greater<>> pqsell;
        priority_queue<pII, vector<pII>> pqbuy;
        for (auto&& order : orders) {
            int price = order[0], amount = order[1], type = order[2];
            if (type == 0) {
                while (!pqsell.empty() && pqsell.top().first <= price && amount) {
                    auto [sell_price, sell_amount] = pqsell.top(); pqsell.pop();
                    if (amount > 0 && amount >= sell_amount) {
                        amount -= sell_amount;
                    }
                    else if (amount > 0) {
                        sell_amount -= amount;
                        pqsell.emplace(sell_price, sell_amount);
                        amount = 0;
                    }
                }

                if (amount) {
                    pqbuy.emplace(price, amount);
                }
            }
            else if (type == 1) {
                while (!pqbuy.empty() && pqbuy.top().first >= price && amount) {
                    auto [buy_price, buy_amount] = pqbuy.top(); pqbuy.pop();
                    if (amount > 0 && amount >= buy_amount) {
                        amount -= buy_amount;
                    }
                    else if (amount > 0) {
                        buy_amount -= amount;
                        pqbuy.emplace(buy_price, buy_amount);
                        amount = 0;
                    }
                }

                if (amount) {
                    pqsell.emplace(price, amount);
                }
            }
        }

        long long ans = 0LL;
        while (!pqbuy.empty()) {
            ans += pqbuy.top().second;
            ans %= MOD;
            pqbuy.pop();
        }
        while (!pqsell.empty()) {
            ans += pqsell.top().second;
            ans %= MOD;
            pqsell.pop();
        }
        return ans;
    }
};
```