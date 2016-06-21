

-----------------------------------------------------------------------------------------------------------------------------------------

栈 
            是一组同类型的数据元素的有序序列，只能在一端访问，称为栈顶；这种后进先出的数据结构称为栈。
            
基本操作：
            .创建一个栈（通常是空的）
            .Empty() 检查栈是否为空
            .Push() 在栈顶插入一个元素
            .Pop() 删除栈顶的元素
            .Top() 提取栈顶元素
            
实现方式：
            1.静态数组
            2.动态数组
            3.链表实现

-----------------------------------------------------------------------------------------------------------------------------------------

	No.1 利用动态数组实现


```cpp

	cpp code:
	
	
                /*栈：线性结构，后进先出，只能在栈顶进行操作*/
                class Stack
                {
                public:
                	Stack(size_t capacity = 1)
                		:_capacity(capacity)
                		,_size(0)
                	{
                		_a = new int[capacity];
                		memset(_a, 0, sizeof(int) * capacity);
                	}
                	~Stack()
                	{
                		if (_a)
                		{
                			delete[] _a;
                			_a = NULL;
                		}
                	}
                	Stack(const Stack& s)
                		:_capacity(s._capacity)
                		,_size(s._size)
                	{
                		_a = new int[_capacity];
                
                		for (size_t i = 0; i < _size; ++i)
                		{
                			_a[i] = s._a[i];
                		}
                	}
                	Stack& operator=(const Stack& s)
                	{
                		if (this != &s)                 //防止自己给自己赋值
                		{
                			if (_a)
                			{
                				delete[] _a;            //防止内存泄露
                			}
                			_capacity = s._capacity;
                			_size = s._size;
                			_a = new int[_capacity];
                
                			for (size_t i = 0; i < _size; ++i)
                			{
                				_a[i] = s._a[i];
                			}
                		}
                
                		return *this;
                	}
                
                public:
                	size_t Size()
                	{
                		return _size;
                	}
                
                	bool Empty()
                	{
                		return (_size == 0);
                	}
                
                	void Push(int num)
                	{
                		CheckCapacity();
                
                		_a[_size] = num;
                		++_size;
                	}
                
                	void Pop()
                	{
                		if (!Empty())
                		{
                			--_size;
                		}
                	}
                
                	int Top()
                	{
                		if (_size > 0)
                		{
                			return _a[_size - 1];
                		}
                		else
                		{
                			cout << "Stack is empty" << endl;
                			assert(false);
                		}
                	}
                
                	void ShowStack()
                	{
                		if(0 == _size)
                		{
                			cout << "Stack is empty" << endl;
                		}
                		else
                		{
                			for (int i = _size - 1; i >= 0; --i)
                			{
                				cout << "   " << _a[i] << endl;
                			}
                			cout << endl;
                		}
                	}
                
                protected:
                	void CheckCapacity()                        //扩容
                	{
                		if (_size >= _capacity - 1)
                		{
                			_capacity *= 2;
                
                			int *tmp = new int[_capacity + 1];
                			
                			for (size_t i = 0; i < _size; ++i)
                			{
                				tmp[i] = _a[i];
                			}
                
                			delete[] _a; 
                			_a = tmp;
                		}
                	}
                
                private:
                	int *_a;                    
                	size_t _size;                   //数据有效个数
                	size_t _capacity;               //数组的容量
                };
                
                
                
```
<br>
<br>

	No.2 利用vector<T>实现,提高效率及复用性


```cpp

	cpp code:
	
                # include <iostream>
                using namespace std;
                # include <vector>
                # include <assert.h>
               
                template<class T>
                class Stack
                {
                public:
                	size_t Size()
                	{
                		return _a.size();
                	}
                
                	bool Empty()
                	{
                		return (_a._size() == 0);
                	}
                
                	void Push(T num)
                	{
                		_a.push_back(num);
                	}
                
                	void Pop()
                	{
                		_a.pop_back();
                	}
                
                	T Top()
                	{
                		if (_a.size() > 0)
                		{
                			return _a[_a.size() - 1];
                		}
                		else
                		{
                			cout << "Stack is empty" << endl;
                			assert(false);
                		}
                	}
                
                	void ShowStack()
                	{
                		if (0 == _a.size())
                		{
                			cout << "Stack is empty" << endl;
                		}
                		else
                		{
                			for (int i = _a.size() - 1; i >= 0; --i)
                			{
                				cout << "   " << _a[i] << endl;
                			}
                			cout << endl;
                		}
                	}
                
                private:
                	vector<T> _a;
                };

-----------------------------------------------------------------------------------------------------------------------------------------

优缺点：
        笔者在这里只用动态数组（Vector<T>更高效，详见STL）来实现，原因是相比于静态数组，动态数组的动态增长显得更加的灵活，
    
    而且，栈是后进先出的数据结构，只在数组的一端操作，如果使用链表，每次都得遍历一遍去找尾，浪费了很多的时间。使用数组直
    
    接操作下标方便了元素的插入删除，节省时间，提高效率。

-----------------------------------------------------------------------------------------------------------------------------------------


