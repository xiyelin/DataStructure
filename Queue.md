## 目录
* [队列的定义](#定义)

* [队列的实现](#数组实现)
	* [数组实现](#数组实现)
	* [链表实现](#)
	
* [队列的应用](#)

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

	code:
	
	
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
	
	方法二：使用数组实现循环队列。
	
![image](http://hbimg.b0.upaiyun.com/503fbfda75689798464317eae6df24d78be56dde59bd-numenq_fw658)	







