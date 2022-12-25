# 排序+哈希表
## 思路
1. 我们要根据$positive_feedback$与$negative_feedback$给每个学生计分，可以用哈希表预处理词汇，方便后面统计可以$O(1)$时间读取。
2. 然后存储学生分数$score$跟$id$，进行排序，排序规则：
   - 得分高的排前，分数一样$id$小的排前
3. 最后取前$k$个学生的$id$值
## 复杂度
- 时间复杂度:
  > $O(ij+nm+nlogn)$，i为positive_feedback词汇的数量，j为negative_feedback词汇的数量，n为学生数量，m为每个report字符串的长度，这里最大为100，nlogn为排序时间复杂度
- 空间复杂度:
  > $O(i+j+n+logn)$，logn为排序所用空间复杂度

## Code
```C++ []
class Solution {
public:
    vector<int> topStudents(vector<string>& positive_feedback, vector<string>& negative_feedback, vector<string>& report, vector<int>& student_id, int k) {
        unordered_set<string> us_pos, us_neg;
        for (auto&& s : positive_feedback) us_pos.emplace(s);
        for (auto&& s : negative_feedback) us_neg.emplace(s);

        int cnt = 0, n = student_id.size();
        vector<pair<int, int>> score(n);

        for (auto&& re : report) {
            score[cnt].second = student_id[cnt];

            int len = re.size();
            string t = "";
            for (int i = 0;i < len;++i) {
                if (re[i] == ' ') {
                    if (us_pos.count(t)) score[cnt].first += 3;
                    else if (us_neg.count(t)) score[cnt].first -= 1;
                    t = "";
                }
                else t += re[i];
            }
            if (t != "") {
                if (us_pos.count(t)) score[cnt].first += 3;
                else if (us_neg.count(t)) score[cnt].first -= 1;
            }
            ++cnt;
        }

        sort(score.begin(), score.end(), [&](const pair<int, int>& p1, const pair<int, int>& p2) {
            return p1.first > p2.first || (p1.first == p2.first && p1.second < p2.second);
        });

        vector<int> ans(k);
        for (int i = 0;i < k;++i) {
            ans[i] = score[i].second;
        }
        return ans;
    }
};
```