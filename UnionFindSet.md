### 并查集(UnionFindSet)
* [并查集的定义](#并查集的定义)
* [并查集的操作](#并查集的操作)
* [并查集的实现](#并查集的实现)
* [小米笔试题](#并查集的例题)

<br>

--------------------------------------------------

<br>

### 并查集的定义：

	　　将N个不同的元素分成一组不相交的集合。开始时，每个元素就是一个集合，然后按一定顺序将属于同一组的元素所在的集合
	
	合并，其间要反复查找一个元素在哪个集合中。
	
<br>

	为什么要使用并查集？
	
	　　其特点是看似并不复杂，但数据量极大，若用正常的数据结构来描述的话，往往在空间上过大，计算机无法承受；即使在空间
	
	上勉强通过，运行的时间复杂度也极高，根本就不可能在比赛规定的运行时间（1～3秒）内计算出试题需要的结果，只能用并查集
	
	来描述。
	

<br>
<br>

### 并查集的操作：

	1.并查集的初始化
	
	2.找一个元素的根
	
	3.合并两个集合
	
	4.返回一共有多少个不相交集合


<br>
<br>

### 并查集的实现：

```cpp


		# include <iostream>
		using namespace std;
		
		class UnionFindSet
		{
		public:
			UnionFindSet(int n)
				:_n(n)
			{
				_ufs = new int[n + 1];
		
				for (int i = 0; i <= _n; ++i)
				{
					_ufs[i] = -1;
				}
			}
			~UnionFindSet()
			{
				delete[] _ufs;
				_ufs = NULL;
			}
		
		protected:
			int _FindRoot(int x)                     //找一个朋友圈的根
			{
				while (_ufs[x] >= 0)
				{
					x = _ufs[x];
				}
		
				return x;
			}
		
		public:
			void Merge(int x1, int x2)               //合并两个朋友圈
			{
				int root1 = _FindRoot(x1);
				int root2 = _FindRoot(x2);
		
				if (root1 != root2)
				{
					_ufs[root1] += _ufs[root2];
					_ufs[root2] = root1;
				}
			}
		
			int GetDifferentCircleNum()                 //返回朋友圈的个数
			{
				int FCCount = 0;
		
				for (int i = 0; i < _n + 1; ++i)
				{
					if (_ufs[i] < 0)
						++FCCount;
				}
		
				return --FCCount;
			}
		
		private:
			int* _ufs;              
			int _n;      //人数
		};
		
		
		
```

<br>
<br>

























