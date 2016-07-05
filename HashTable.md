### 哈希表(Hash Table)
* [哈希表的定义](#定义)
* [构造哈希表的方法](#构造哈希表的方法)
* [哈希冲突](#哈希冲突)
* [哈希表的负载因子](#负载因子)
* [哈希表的实现](#代码)
* [哈希表的性能与应用](#性能分析与应用)

<br>

--------------------------------------------------



### 定义：

	散列表（Hash table，也叫哈希表），是根据关键码值(Key value)而直接进行访问的数据结构。也就是说，它通过把关键码值
	
	映射到表中一个位置来访问记录，以加快查找的速度。这个映射函数叫做散列函数，存放记录的数组叫做散列表。
	

<br>

### 构造哈希表的方法:

	1. 直接定址法--取关键字的某个线性函数为散列地址，Hash（Key）= Key 或 Hash（Key）= A*Key + B，A、B为常数。
	
	2. 除留余数法--取关键值被某个不大于散列表长m的数p除后的所得的余数为散列地址。Hash（Key）= Key % P。
	
	3. 平方取中法--当无法确定关键字中哪几位分布较均匀时，可以先求出关键字的平方值，然后按需要取平方值的中间几位作为哈希地址。
	
	4. 折叠法--将关键字分割成位数相同的几部分，最后一部分位数可以不同，然后取这几部分的叠加和（去除进位）作为散列地址。

	5. 随机数法--选择一随机函数，取关键字的随机值作为散列地址，通常用于关键字长度不同的场合。

	6. 数学分析法--因此数字分析法就是找出数字的规律，尽可能利用这些数据来构造冲突几率较低的散列地址。
	
	
<br>

### 哈希冲突：
	
	哈希函数：不同的值通过相同的函数，得到哈希表的散列地址，这个函数就叫做 “哈希函数”。
	
	哈希冲突：不同的值通过相同的函数，可能会映射到相同的散列地址，这种不同的值映射到相同位置的现象叫做 “哈希冲突”。
	
			 也叫做 “哈希碰撞”。哈希冲突是任何哈希函数不能避免的。
			 
	
	解决哈希冲突的办法：
	
		1.闭散列法：
			
			1.1： 线性探测--冲突时，将冲突的值放到后面的第一个值为空的散列地址上去。这样可能会产生连锁式的哈希冲突。
			
			1.2： 二次探测--冲突时，每次向后移动i^2个位置，不再是加一，使他们的位置相对分散，缓解哈希冲突。
			
		2.开链法(哈希桶)：
		
			它采用指针数组+链表的方法来实现。即使不同的值映射到相同的位置，也没关系。数组保存的是链表头节点的地址，
			
			如果产生冲突，就将元素挂在链表上。
			
			优化：这里可以采用指针数组+红黑树的方式实现，在查找等操作时效率会比链表大大提高。
	
	
<br>	
	
### 负载因子：

	哈希表的负载因子： α = 填入表中元素的个数 / 哈希表的长度 。
	
	哈希表的负载因子是哈希表的装满程度的标志因子。由于哈希表长度是定值，所以α得值和填入元素的个数成正比。
	
	对于开链法来说，我们应该将负载因子α严格限制在 0.7~0.8 之间，如果超过了0.8，CPU在查表时的缓冲不命中率按指数形式上升；
	
	在Java库中，负载因子限制在0.75，超过了这个值，就会重新 resize 哈希表的大小，然后将所有元素重新定址。
	
	
<br>

### 代码：

```cpp

		# include <iostream>
		using namespace std;
		# include <vector>
		# include <string>
		
		template<class K>
		struct DefaultHashFunc
		{
			size_t operator() (const K& key)
			{
				return key;
			}
		};
		
		template<>
		struct DefaultHashFunc<string>
		{
			static size_t _BKDRHash(const char* str)
			{
				unsigned int seed = 131; // 31 131 1313 13131 131313
				unsigned int hash = 0;
				while (*str)
				{
					hash = hash * seed + (unsigned char)(*str++);
				}
				return (hash & 0x7FFFFFFF);
			}
		
			size_t operator() (const string& key)
			{
				return _BKDRHash(key.c_str());
			}
		};
		
		template<class K, class V>
		struct HashTableNode
		{
			K _key;
			V _value;
		
			HashTableNode<K, V>* _next;
		
			HashTableNode(const K& key, const V& value)
				:_key(key)
				,_value(value)
				,_next(NULL)
			{}
		};
		
		
		template<class K, class V, class HashFunc = DefaultHashFunc<K>>
		class HashTable
		{
			typedef HashTableNode<K, V> Node;
		public:
			HashTable()
				:_size(0)
			{
				_tables.resize(10);
			}
			~HashTable()
			{
				for (size_t i = 0; i < _tables.size(); ++i)
				{
					Node* cur = _tables[i];
		
					while (cur)
					{
						Node* del = cur;
						
						cur = cur->_next;
						delete del;
					}
					_tables[i] = NULL;
				}
			}
		
		public:
			void PrintHashTables()
			{
				Node* cur = NULL;
		
				for (size_t i = 0; i < _tables.size(); ++i)
				{
					printf("HashTable[%d]：", i);
					cur = _tables[i];
					while (cur)
					{
						cout << "(" << cur->_key << "," << cur->_value << ")" << "->";
						cur = cur->_next;
					}
					cout << "NULL" << endl;
				}
			}
			bool Insert(const K& key, const V& value)
			{
				_CkeckCapacity();
		
				size_t index = _HashFunc(key);
				Node* cur = _tables[index];
				Node* tmp = new Node(key, value);
		
				while (cur)
				{
					if (key == cur->_key)
						return false;
		
					cur = cur->_next;
				}
		
				tmp->_next = _tables[index];
				_tables[index] = tmp;
		
				++_size;
		
				return true;
			}
		
			bool Remove(const K& key)
			{
				size_t index = _HashFunc(key);
		
				Node* cur = _tables[index];
				Node* prev = cur;
				while (cur)
				{
					if (key == cur->_key)
						break;
		
					prev = cur;
					cur = cur->_next;
				}
		
				if (cur && cur == _tables[index])
				{
					_tables[index] = cur->_next;
					delete cur;
					cur = NULL;
				}
				else if (cur && cur != _tables[index])
				{
					prev->_next = cur->_next;
		
					delete cur;
					cur = NULL;
				}
				else
					return false;
		
				return true;
			}
			
			Node* Find(const K& key)
			{
				size_t index = _HashFunc(key);
		
				Node* cur = _tables[index];
				while (cur)
				{
					if (key == cur->_key)
						return cur;
		
					cur = cur->_next;
				}
		
				return NULL;
			}
		
			V& operator[](const K& key)
			{
				size_t index = _HashFunc(key);
		
				Node* cur = _tables[index];
				while (cur)
				{
					if (key == cur->_key)
						return cur->_value;
		
					cur = cur->_next;
				}
			}
		
		
		protected:
			static size_t BKDRHash(const char * str)
			{
				unsigned int seed = 131; // 31 131 1313 13131 131313
				unsigned int hash = 0;
				while (*str)
				{
					hash = hash * seed + (*str++);
				}r
					eturn(hash & 0x7FFFFFFF);
			}
			size_t _HashFunc(const K& key)
			{
				return HashFunc()(key) % _tables.size();
			}
		
			void _CkeckCapacity()
			{
				if (_size == _tables.size())
				{
					size_t newsize = _GetNewSize(_size);
		
					if (newsize == _tables.size())
						return;
		
					vector<Node*> newtables;
					newtables.resize(newsize);
		
					for (size_t i = 0; i < _tables.size(); ++i)
					{	
						Node* cur = _tables[i];
						while (cur)
						{
							Node* tmp = cur;
							cur = cur->_next;
		
							size_t index = _HashFunc(tmp->_key);
							tmp->_next = newtables[index];
							newtables[index] = tmp;
						}
						_tables[i] = NULL;
					}
		
					swap(_tables, newtables);
				}
			}
			size_t _GetNewSize(size_t size)
			{
				const int _PrimeSize = 28;
				static const unsigned long _PrimeList[_PrimeSize] =
				{
					53ul, 97ul, 193ul, 389ul, 769ul,1543ul, 3079ul, 6151ul, 12289ul, 24593ul,
					49157ul, 98317ul, 196613ul, 393241ul,786433ul,1572869ul, 3145739ul, 
					6291469ul, 12582917ul,25165843ul,50331653ul, 100663319ul, 201326611ul,
					402653189ul,805306457ul,1610612741ul, 3221225473ul, 4294967291ul
				};
		
				for (size_t i = 0; i < _PrimeSize; ++i)
				{
					if (_PrimeList[i]>size)
						return _PrimeList[i];
				}
		
				return _PrimeList[_PrimeSize-1];
			}
		
		private:
			vector<Node*> _tables;    //_tables.size() 是哈希表的容量 
			size_t _size;             //哈希表中元素的个数
		}; 

		
		
```


<br>


### 性能分析与应用：

分析：

	查找过程中，关键码的比较次数，取决于产生冲突的多少，产生的冲突少，查找效率就高，产生的冲突多，查找效率就低。
	
	因此，影响产生冲突多少的因素，也就是影响查找效率的因素。影响产生冲突多少有以下三个因素：
		
	1. 散列函数是否均匀；
	2. 处理冲突的方法；
	3. 散列表的装填因子。


应用：
		
1.信息安全： 
	
[文件校验](http://baike.baidu.com/view/5094912.htm)
[数字签名](http://baike.baidu.com/view/7626.htm)
认证模式：MD5、SHA1的破解

2.布隆过滤器： 
	
[博客1](https://segmentfault.com/a/1190000002729689)
[博客2](http://www.cnblogs.com/haippy/archive/2012/07/13/2590351.html)
	
	
	
	
	
		
	
	


