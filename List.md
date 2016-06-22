## 目录
* [单链表的节点](#单链表的节点)

* [单链表的数据结构](#单链表的数据结构)

* [单链表代码](#代码)

* [链表相关的面试题](#链表相关的面试题)
	- [x] [1.从尾到头打印单链表](#从尾到头打印单链表)

<br>
<br>
<br>

### 单链表的节点

	分为头节点和普通节点。
	
![image](http://hbimg.b0.upaiyun.com/0106f3fb369a499d60df752dffa74b06a12dda5663bd-786ixp_fw658)

<br>

### 单链表的数据结构

	单链表一般分为带头节点的链表和不带头节点的链表。带头节点的链表，它的数据域里面可以是随机值，
	
	也可以拿来存储链表的有效节点个数等信息。
	
	操作：	* PushBack()     * PopBack()
			* PushFront()    * PopFront()
			* Erase()		 * Find()
			* Remove()       * Insert()
	
	
![image](http://hbimg.b0.upaiyun.com/b399451e1f3eaa006b84e379d6f4d3eeb9b9378e7599-nysGD4_fw658)


![image](http://hbimg.b0.upaiyun.com/631bf7d136adc82fef8fdb2751edab68c6d14aa4794c-6ajoZ2_fw658)


<br>

### 代码

```cpp

	cpp code:
	
	
		# include <iostream>
		using namespace std;
		
		template<class T>
		struct ListNode
		{
			T _data;                    //数据域
			ListNode* _next;            //指向下个节点的指针域
		
			ListNode(const T& x = 0)
			{
				_data = x;
				_next = NULL;
			}
		};
		
		template<class T>
		class List
		{
			typedef ListNode<T> Node;
		public:
			List()
				:_head(NULL)
			{}
			~List()                                   //确保析构函数的正确性，防止内存泄露
			{
				while (_head)
				{
					Node* NextNode = _head->_next;
		
					delete _head;
					_head = NextNode;
				}
			}
		
		public:
			void PushFront(const T& x)
			{
				if (NULL == _head)                   //链表为空
					_head = new Node(x);
				else
				{
					Node* tmp = new Node(x);
					
					tmp->_next = _head;
					_head = tmp;
				}
			}
			void PopFront()
			{
				if (NULL == _head)
					return;
				if (NULL == _head->_next)
				{
					delete _head;
					_head = NULL;
				}
				else
				{
					Node* NextNode = _head->_next;
		
					delete _head;
					_head = NextNode;
				}
			}
			void PushBack(const T& x)
			{
				if (NULL == _head)                   //链表为空
					_head = new Node(x);
				else if (NULL == _head->_next)       //链表中只有一个节点
					_head->_next = new Node(x);
				else                                 
				{
					Node* cur = _head;
					Node* tmp = new Node(x);
		
					while (cur && cur->_next)
					{
						cur = cur->_next;
					}
					cur->_next = tmp;
				}
			}
			void PopBack()
			{
				if (NULL == _head)
					return;
				if (NULL == _head->_next)
				{
					delete _head;
					_head = NULL;
				}
				else
				{
					Node* cur = _head;
					Node* prev = NULL;
		
					while (cur && cur->_next)
					{
						prev = cur;              //记录最后一个节点的前一个节点的位置
						cur = cur->_next;
					}
					delete cur;
					prev->_next = NULL;
				}
			}
			void Insert(T pos, const T& x)       //默认pos合法，第一个元素的pos为1
			{
				if (0 == pos)
					PushFront(x);
				else
				{
					Node* cur = new Node(x);
					Node* prev = _head;
					while (--pos)
					{
						prev = prev->_next;
					}
		
					cur->_next = prev->_next;
					prev->_next = cur;
				}
			}
			void Erase(T pos)                     //默认pos合法
			{
				if (NULL == _head)
					return;
		
				if (1 == pos)
				{
					Node* cur = _head->_next;
		
					delete _head;
					_head = cur;                   //注意改变 _head的指向
				}
				else
				{
					Node* prev = _head;
					Node* cur = _head;
		
					while (--pos)
					{
						prev = cur;
						cur = cur->_next;
					}
		
					prev->_next = cur->_next;
					delete cur;
					cur = NULL;
				}
			}
			void Remove(const T& x)               //找到就删除，找不到返回NULL
			{
				if (NULL == _head)
					return;
				
				if (_head->_data == x)
				{
					Node* NextNode = _head->_next;
		
					delete _head;
					_head = NextNode;
				}
				else
				{
					Node* cur = _head;
					Node* prev = NULL;
		
					while (cur && cur->_data != x)
					{
						prev = cur;
						cur = cur->_next;
					}
		
					if (NULL == cur)
						return;
					else
					{
						prev->_next = cur->_next;
						delete cur;
						cur = NULL;
					}
				}
			}
			Node* Find(const T& x)                   //找到之后返回节点的位置，否则返回NULL
			{
				if (NULL == _head)
					return NULL;
		
				Node* cur = _head;
				while (cur && x != cur->_data)
				{
					cur = cur->_next;
				}
		
				return cur;
			}
		
		private:
			Node* _head;         //指向第一个节点的指针
		};
		
		
```

<br>

### 链表相关的面试题

-----------------------------------------------------------------

<br>

##### 从尾到头打印单链表

```cpp

	void FromTailToHeadPrint(const List<T>& l)
	{
		stack<T> s;                      
		ListNode<T>* cur = l._head;

		while (cur)
		{
			s.push(cur->_data);                     //先将链表的所有数据入栈
			cur = cur->_next;
		}

		while (!s.empty())
		{
			cout << s.top() << "->";                //边打印边出栈即为逆序
			s.pop();
		}
		cout << "NULL" << endl;
	}
	
```






