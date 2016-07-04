### 二叉搜索树(Search binary tree)

* [二叉搜索树的定义](#定义)

* [二叉搜索树的实现](#代码)
	* [删除操作(重点)](#删除操作详解)
	
* [二叉搜索树的分析](#优缺点分析)

<br>

-----------------------------------------------------


<br>

#### 定义

	二叉查找树（Binary Search Tree），（又：二叉搜索树，二叉排序树）它或者是一棵空树，或者是具有下列性质的二叉树：
	
	1.每个节点都有一个作为搜索依据的关键码（key），所有节点的关键码互不相同。
	
	2.左子树上所有节点的关键码（key）都小于根节点的关键码（key）。
	
	3.右子树上所有节点的关键码（key）都大于根节点的关键码（key）。
	
	4.左右子树都是二叉搜索树。
	
	

![image](http://hbimg.b0.upaiyun.com/aa651b8ff11e4b033d14ab4fc5484b9a02fa4810c42c-IsrrW2_fw658)


<br>
<br>

#### 代码

```cpp

	cpp node:
	
		# include <iostream>
		using namespace std;
		
		template<class K, class V>
		struct SBTreeNode
		{
			K _key;
			V _value;
		
			SBTreeNode<K, V>* _left;
			SBTreeNode<K, V>* _right;
		
			SBTreeNode(const K& key = K(), const V& value = V())
				:_key(key)
				,_value(value)
				,_left(NULL)
				,_right(NULL)
			{}
		};
		
		template<class K, class V>
		class SBTree
		{
			typedef SBTreeNode<K, V> Node;
		public:
			SBTree()
				:_root(NULL)
			{}
			~SBTree()
			{
				//析构函数
			}
		
		public:
			void InorderSBTree()
			{
				_Inorder(_root);
				cout << endl;
			}
			bool Insert(const K& key, const V& value)
			{
				if (NULL == _root)
				{
					_root = new Node(key, value);
					return true;
				}
		
				Node* cur = _root;
				Node* prev = NULL;
				Node* tmp = new Node(key, value);
		
				while (cur)
				{	
					prev = cur;
					if (key > cur->_key)
						cur = cur->_right;
					else if (key < cur->_key)
						cur = cur->_left;
					else
						return false;
				}
				if (prev->_key > key)
					prev->_left = tmp;
				else
					prev->_right = tmp;
		
				return true;
			}
		
			Node* Find(const K& key)
			{
				Node* cur = _root;
		
				while (cur)
				{
					if (key > cur->_key)
						cur = cur->_right;
					else if (key < cur->_key)
						cur = cur->_left;
					else
						return cur;
				}
		
				return NULL;
			}
		
			void Remove(const K& key)
			{
				Node* cur = _root;
				Node* prev = cur;
		
				while (cur)
				{
					if (cur->_key > key)
					{
						prev = cur;
						cur = cur->_left;
					}
					else if (cur->_key < key)
					{
						prev = cur;
						cur = cur->_right;
					}
					else
						break;
				}
		
				//删除的节点如果没有左子树用右子树填补
				if (cur && NULL == cur->_left && cur->_right)
				{
					if (cur->_key < prev->_key)
						prev->_left = cur->_right;
					else
						prev->_right = cur->_right;
					
					delete cur;
				}
				//删除的节点如果没有右子树用左子树填补
				else if (cur && NULL == cur->_right && cur->_left)
				{
					if (cur->_key < prev->_key)
						prev->_left = cur->_left;
					else
						prev->_right = cur->_left;
		
					delete cur;
				}
				//删除的节点如果有左右子树用右子树中最小值填补
				else if (cur && cur->_left && cur->_right)
				{
					//cur右子树中最小的节点就是cur的右
					Node* MinSubR = cur->_right;
		
					cur->_key = MinSubR->_key;
					cur->_value = MinSubR->_value;
		
					if (NULL == MinSubR->_left)
					{
						if (MinSubR->_right)
							cur->_right = MinSubR->_right;
						else
							cur->_right = NULL;
			
						delete MinSubR;
						MinSubR = NULL;
					}
					else
					{
						Node* prev_MinSubR = MinSubR;
		
						//右子树最小值为 MinSubR
						while (MinSubR && MinSubR->_left)
						{
							prev_MinSubR = MinSubR;
							MinSubR = MinSubR->_left;
						}
		
						if (MinSubR->_right)
							prev_MinSubR->_left = MinSubR->_right;
						else
							prev_MinSubR->_left = NULL;
		
						delete MinSubR;
						MinSubR = NULL;
					}
				}
				//删除叶子节点
				else if (cur)
				{
					if (cur->_key > prev->_key)
						prev->_right = NULL;
					else
						prev->_left = NULL;
		
					delete cur;
				}
				//没有找到要删除的节点
				else
					cout << "没有找到此节点！" << endl;
			}
		
		protected:
			void _Inorder(Node* root)
			{
				if (NULL == root)
					return;
		
				_Inorder(root->_left);
				cout << "(" << root->_key << "," << root->_value << ")" << " ";
				_Inorder(root->_right);
			}
		
		private:
			Node* _root;
		};
	
	
	
	
```

<br>

#### 删除操作详解







<br>
<br>

#### 优缺点分析













