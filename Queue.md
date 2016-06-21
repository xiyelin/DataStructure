## 目录
* [队列的定义](#定义)

* [队列的实现](#数组实现)
	* [数组实现](#数组实现)
	* [链表实现](#链表实现)
	
* [队列的应用](#队列的应用)

-----------------------------------------------
<br>
<br>
### 定义：

	只能在表的一端进行插入，在表的另一端进行删除运算的线性表。是先进先出(FIFO)的数据结构。
	
	队尾：允许插入的一端。(入队操作)
	
	队头：允许删除的一端。(出队操作)
	
	
<br>	
### 数组实现

	方法一：使用数组实现，每次删除的时候把后边的元素向前移动，效率很低，时间复杂度为 O(N)。

![image](http://hbimg.b0.upaiyun.com/6569df1370f159b282a4b3227742a8ba6e022d3b79da-g3pe6B_fw658)


```cpp

	cpp code:
	
	
		class Queue
		{
		public:
			Queue(int capacity)
			{
				_a = new int[capacity];				//严谨应检查是否抛出异常
				_size = 0;
				_capacity = capacity;
			}
			~Queue()
			{
				delete[] _a;
			}
		
		public:
			void Display()                                   //打印函数
			{
				int size = _size;
		
				if (0 == size)
				{
					cout << "Display : Queue is empty !" << endl;
				}
				else
				{
					int i = 0;
					while (size--)
					{
						cout << _a[i++] << " ";
					}
					cout << endl;
				}
			}
		
		
			bool Empty()                                    //判断队列是否为空
			{
				if (0 == _size)
					return true;
				else
					return false;
			}
			int Size()                                      //返回队列的大小
			{
				return _size;
			}
			int Front()                                     //返回队头元素
			{
				if (_size > 0)
					return _a[0];
			}
			int Back()                                      //返回队尾元素
			{
				if (_size > 0)
					return _a[_size - 1];
			}
			void Pop()                                      //删除
			{
				if (0 == _size)
					return;
				else if (1 == _size)
				{
					_size = 0;
					return;
				}
				else
				{
					for (int i = 1; i < _size; ++i)         //效率太低 O(N)
					{
						_a[i - 1] = _a[i];
					}
					--_size;
				}
			}
			void Push(int x)                                 //插入
			{
				if (_size < _capacity)
				{
					_a[_size] = x;
					++_size;
				}
				else
				{
					cout << "Queue is full !" << endl;
				}
			}
		
		private:
			int* _a;                          //数组
			int _size;						  //元素有效个数
			int _capacity;					  //数组的容量
		};
		
		
		
```

<br>
	
	方法二：使用数组实现循环队列。插入删除的时间复杂度都是 O(1)。
	
<br>

	当且仅当 Front = Rear 时，队列为空。初始条件 Front = Rear = 0；
	
	
![image](http://hbimg.b0.upaiyun.com/a5eb007aa7e0adf886c512ef71aa27c7f6ce55cb3e96-Oje8bX_fw658)	



	但是这会引发一个问题，如果队列满了的话，条件将会和判空的条件冲突，为了解决这个问题，队列不能插满。
	
	
![image](http://hbimg.b0.upaiyun.com/66a337d33c8817728662533877d417c41c2b9388150d-URf4ah_fw658)		
	


```cpp

	cpp code:
	
		//循环队列
		class LoopQueue
		{
		public:
			LoopQueue(int size)
			{
				_a = new int[size];
				_capacity = size;
				_Front = _Rear = 0;
			}
			~LoopQueue()
			{
				delete[] _a;
			}
		
		public:
			void Display()
			{
				if (_Front == _Rear)
				{
					cout << "Display : LoopQueue is full !" << endl;
					return;
				}
				else
				{
					for (int i = _Front + 1; i <= _Rear; ++i)
					{
						cout << _a[i] << " ";
					}
					cout << endl;
				}
			}
		
			bool Empty()                                  //判断队列是否为空
			{
				return (_Front == _Rear);
			}
			
			int Size()                                    //返回队列有效元素个数
			{
				return _Rear;
			}
		
			int Front()                                   //返回队首元素
			{
				if (_Front != _Rear)
					return (_a[_Front + 1]);
			}
		
			int Back()                                    //返回队尾元素
			{
				if (_Front != _Rear)
					return (_a[_Rear]);
			}
		
			void Pop()                                    //删除队头元素
			{
				if (_Front == _Rear)
					return;
				else
				{
					_Front += 1;
				}
			}
		
			void Push(int x)                              //在队尾插入一个元素
			{
				if (_capacity == _Rear + 1)
				{
					cout << "LoopQueue is full !" << endl;
					return;
				}
				else
					_a[++_Rear] = x;
			}
		
		private:
			int* _a;            //数组
			int _capacity;      //容量
			int _Front;         //队头
			int _Rear;          //队尾
		};	
			
		
		
```

<br>
### 链表实现


![image](http://hbimg.b0.upaiyun.com/115cfbb78ced7c562ef4495ddddef9dd6849cb6d25df-lonpnp_fw658)


	
	队列和栈一样既可以用数组实现也可以用链表实现。队列的链表实现方法，时间复杂度为 O(1)。
	

```cpp

	cpp code:
		
		
		
		
		
		
		
		
```


<br>

### 队列的应用


	




			




