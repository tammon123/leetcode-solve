# 排序+字符串匹配/字典树
## 方法一(排序+字符串匹配)：
### 思路
1. 首先我们可以将数组排序，排序后作为父文件夹一定在子文件夹前，例如：```"/a","/a/b"```。那么我们遍历排序后的数组，假如当前遍历的文件夹与之前答案存入的最后一个文件夹进行字符串匹配能匹配，那么说明当前文件夹是子文件夹，跳过继续遍历下一个文件夹。

2. 匹配规则为：父文件夹位于子文件的开始位置，且子文件夹从$0$加上父文件夹长度后是字符$'/'$。为啥这样匹配，假如：父文件夹为$"/a"$，当前文件夹为$"/ab"$，那他们不能匹配。

### 复杂度
- 时间复杂度:
  > $O(nl*logn)$，$n$为数组$folder$的长度，$l$为文件夹平均长度，排序需要$O(nl*logn)$时常，后面构造答案需要$O(nl)$，最终趋于$O(nl*logn)$
- 空间复杂度:
  > $O(logn)$，排序需要栈空间为$O(logn)$

### Code
```C++ []
class Solution {
public:
    vector<string> removeSubfolders(vector<string>& folder) {
        sort(folder.begin(), folder.end());
        int n = folder.size();
        vector<string> ans;
        ans.emplace_back(folder[0]);
        for (int i = 1;i < n;++i) {
            int size = folder[i].size();
            auto j = folder[i].find(ans.back());
            int match_to = j + ans.back().size();

            if (j == 0 && (match_to < size && folder[i][match_to] == '/')) continue;
            else ans.emplace_back(folder[i]);
        }
        return ans;
    }
};
```

## 方法二(字典树)：
### 思路
1. 可以用字典树方法，每个节点相当于一个文件夹。对于字典树的每个节点，我们都可以存储一个变量$end$作为结束标志，那么$end>=0$表示有该节点对应$folder[end]$，否则$end=-1$表示该节点作为中间节点。

2. 我们可以对字符$'/'$进行分割字符串后构建字典树。

3. 然后用深度优先搜索遍历字典树。如果当前节点的$end>=0$我们存入答案，然后直接回溯返回，因为后续的节点都作为当前节点的子文件夹。

### 复杂度
- 时间复杂度:
  > $O(nl)$，$n$为数组$folder$的长度，$l$为文件夹平均长度，构建字典树需要的时间为$O(nl)$
- 空间复杂度:
  > $O(nl)$，构建字典树需要的空间大小

### Code
```C++ []
class Trie {
public:
    Trie():end(-1) {}

    unordered_map<string, Trie*> children;
    int end;
};

class Solution {
public:
    vector<string> removeSubfolders(vector<string>& folder) {
        auto split = [&](string& s)->vector<string> {
            vector<string> res;
            string t;
            for (int i = 1;i < s.size();++i) {
                if (s[i] == '/') {
                    res.emplace_back(t);
                    t.clear();
                }
                else t.push_back(s[i]);
            }
            res.emplace_back(t);
            return res;
        };

        Trie* root = new Trie();
        for (int i = 0;i < folder.size();++i) {
            vector<string> vsplit = split(folder[i]);
            Trie* cur = root;
            for (auto&& s : vsplit) {
                if (!cur->children.count(s)) {
                    cur->children[s] = new Trie;
                }
                cur = cur->children[s];
            }
            cur->end = i;
        }

        vector<string> ans;
        auto dfs = [&](Trie* node, auto&& dfs)->void {
            if (node->end != -1) {
                ans.emplace_back(folder[node->end]);
                return;
            }
            for (auto&& [_, child] : node->children) {
                dfs(child, dfs);
            }
        };
        dfs(root, dfs);
        return ans;
    }
};
```