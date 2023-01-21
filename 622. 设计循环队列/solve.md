# 链表
## 方法一(链表)：
### 思路
1. 用链表可以实现队列，$capacity$表示存入容量，$size$表示链表节点数，$head$头节点，$tail$尾节点，在首尾插入节点都是$O(1)$操作完成

### 复杂度
- 时间复杂度:
  > $O(1)$，各项都是$O(1)$操作
- 空间复杂度:
  > $O(n)$，输入的元素个数

### Code
```C++ []
class MyCircularQueue {
public:
	MyCircularQueue(int k):m_capacity(k), m_size(0) {
		m_head = m_tail = nullptr;
	}

	bool enQueue(int value) {
		if (isFull()) return false;
		ListNode* node = new ListNode(value);
		if (!m_head) {
			m_head = m_tail = node;
		}
		else {
			m_tail->next = node;
			m_tail = node;
		}
		++m_size;
		return true;
	}

	bool deQueue() {
		if (isEmpty()) return false;
		ListNode* node = m_head;
		m_head = m_head->next;
		delete node;
		--m_size;
		return true;
	}

	int Front() {
		if (isEmpty()) return -1;
		return m_head->val;
	}

	int Rear() {
		if (isEmpty()) return -1;
		return m_tail->val;
	}

	bool isEmpty() {
		return m_size == 0;
	}

	bool isFull() {
		return m_size == m_capacity;
	}
private:
	ListNode* m_head;
	ListNode* m_tail;
	int m_size;
	int m_capacity;
};
```