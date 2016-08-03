## 堆(二叉堆)及其应用

-----------------------------

### 什么是二叉堆？

	二叉堆是一种特殊的堆，二叉堆是完全二元树（二叉树）或者是近似完全二元树（二叉树）。二叉堆有两种：
	
	最大堆和最小堆。最大堆：父结点的键值总是大于或等于任何一个子节点的键值；最小堆：父结点的键值总是
	
	小于或等于任何一个子节点的键值。
	
	
### 二叉堆的存储结构：

	二叉堆一般用数组来存储；如果一个节点的下标为 i ，其左孩子下标为 2*i+1，其右孩子下标为 2*i+2；
	
	第一个非叶子节点的下标为 size/2-1；
	

### 二叉堆的性质及其常见操作：

	1.最小的元素在顶端（最大的元素在顶端）
	
	2.每个元素都比它的父节点小（大），或者和父节点相等
	
	操作：Insert() / Remove / Top() / Empty() 


<br>

### 二叉堆的实现：


```cpp


		# include <iostream>
		using namespace std;
		# include <vector>
		# include <assert.h>
		
		//小堆
		struct Less
		{
		public:
		    bool operator()(const int& l, const int& r)
		    {
		        return l < r;
		    }
		};
		//大堆
		struct Greater
		{
		public:
		    bool operator()(const int& l, const int& r)
		    {
		        return l > r;
		    }
		};
		
		template<class Compare>      //仿函数实现比较器
		class Heap
		{
		public:
		    Heap()
		    {}
		    Heap(int *str, int size)
		    {
		        for (int i = 0; i < size; ++i)
		        {
		            _array.push_back(str[i]);
		        }
		
		        int begin = size / 2 - 1;
		        for (; begin >= 0; --begin)
		        {
		            _AdjustDown(begin);
		        }
		    }
		    Heap(vector<int> str)
		    {
		        _array.swap(str);
		
		        int begin = (_array.size() / 2) - 1;
		        for (; begin >= 0; --begin)
		        {
		            _AdjustDown(begin);
		        }
		    }
		
		    void Insert(const int x)
		    {
		        _array.push_back(x);
		        int size = _array.size() - 1;
		        _AdjustUp(size);
		    }
		
		    void Remove()
		    {
		        _array[0] = _array[_array.size() - 1];
		        _array.pop_back();
		
		        _AdjustDown(0);
		    }
		
		    const int Top()
		    {
		        return _array[0];
		    }
		
		    bool Empty()
		    {
		        return _array.empty();
		    }
		
		protected:
		    void _AdjustDown(int root)             //向下调整
		    {
		        int size = _array.size();
		
		        while (root < size)
		        {
		            int left = root * 2 + 1;
		            int right = left + 1;
		            int key = left;
		            if (right < size)
		            {
		                key = Compare()(_array[left], _array[right]) ? left : right;
		            }
		
		            if (key < size && Compare()(_array[key], _array[root]))
		            {
		                swap(_array[root], _array[key]);
		                root = key;                                    //注意要调整完
		            }
		            else
		                break;
		        }
		    }
		
		    void _AdjustUp(int child)                                  //向上调整
		    {
		        while (child >= 0)
		        {
		            int root = (child - 1) / 2;
		
		            if (Compare()(_array[child], _array[root]))
		            {
		                swap(_array[root], _array[child]);
		                child = root;
		            }
		            else
		                break;
		        }
		    }
		
		private:
		    vector<int> _array;
		};
		
		
		int main()
		{
		    int str[] = { 10, 16, 18, 12, 11, 13, 15, 17, 14, 19 };
		
		    Heap<Greater> h(str, 10);
		
		    //h.Insert(1);
		    //h.Remove();
		
		    //堆排序
		    /*for (int i = 0; i < 10; ++i)
		    {
		        str[i] = h.Top();
		        h.Remove();
		    }*/
		        
		    system("pause");
		    return 0;
		}
		
		
```

<br>

### 优先级队列：

	在C++的STL中提供了优先队列，定义方式为priority_queuepriq;默认为每一次弹出队列中最大的元素，与二叉堆
	
	性质相似，很多时候可以直接当作二叉堆使用。
	

```cpp

		class PriorityQueue
		{
		public:
		    void Push(const int& x)
		    {
		        _heap.Insert(x);
		    }
		    void Pop()
		    {
		        _heap.Remove();
		    }
		    const int& top()
		    {
		        return _heap.Top();
		    }
		    bool empty()
		    {
		        return _heap.Empty();
		    }
		
		private:
		    Heap _heap;                        //优先级队列利用二叉堆来实现
		};
		
		
		
```

<br>

### 二叉堆的应用：


	1. 优先级队列；
	
	2. 100w个数中找出最大的前K个数；

	3. 2015年春节期间，A公司的支付软件某宝和T公司某信红包大乱战。春节后高峰以后，公司Leader要求后台的攻城狮对后台
	   的海量数据进行分析。先要求分析出各地区发红包金额最多的前100用户。现在知道人数最多的s地区大约有1000w用户。
	   要求写一个算法实现。【扩展：海量数据处理】

	4. 堆排序；






















