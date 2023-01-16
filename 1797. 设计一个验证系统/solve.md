# 哈希表
## 方法一(哈希表)：
### 思路
1. 因为$tokenId$是唯一值，我们所有的操作都是$currentTime$与$timeToLive$的操作，我们可以把数据存入哈希表中，方便读取
   - ```AuthenticationManager(int timeToLive)```: 存入$timeToLive$的值
   - ```generate(string tokenId, int currentTime)```: 将$tokenId$与$currentTime+timeToLive$存入哈希表中，表示这个$tokenId$对应的过期时间
   - ```renew(string tokenId, int currentTime)```: 如果$tokenId$存在且过期时间大于$currentTime$，更新当前过期时间，反之不操作
   - ```countUnexpiredTokens(int currentTime)```:判断哈希表中是否有过期时间大于$currentTime$，有就答案加一

### 复杂度
- 时间复杂度:
  > $O(1)$，因为$generate$与$renew$操作都是$O(1)$，而$countUnexpiredTokens$操作取决于操作次数，总次数比较小，最终均摊为$O(1)$
- 空间复杂度:
  > $O(n)$，$n$为输入的验证码数

### Code
```C++ []
class AuthenticationManager {
public:
	AuthenticationManager(int timeToLive):m_timeToLive(timeToLive) {

	}

	void generate(string tokenId, int currentTime) {
		m_um[tokenId] = currentTime + m_timeToLive;
	}

	void renew(string tokenId, int currentTime) {
		if (m_um.count(tokenId) && m_um[tokenId] > currentTime)
			m_um[tokenId] = currentTime + m_timeToLive;
	}

	int countUnexpiredTokens(int currentTime) {
		int cnt = 0;
		for (auto& [_, time] : m_um) {
			if (time > currentTime) ++cnt;
		}
		return cnt;
	}
private:
	int m_timeToLive;
	unordered_map<string, int> m_um;
};
```