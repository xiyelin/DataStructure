### 红黑树(RedBlankBinaryTree)
* [红黑树的定义](#红黑树的定义)
* [红黑树的性质](#红黑树的性质)
* [红黑树的实现](#红黑树的代码)
* [红黑树的应用](#红黑树的应用)

<br>

---------------------------------------------------


### 红黑树的定义：

	红黑树是一棵二叉搜索树，它在每个节点上增加了一个存储位来表示节点的颜色，可以是Red或Black。通过对任何一条从根到叶子简单
	
	路径上的颜色来约束，红黑树保证最长路径不超过最短路径的两倍，因而近似于平衡。


```cpp

		template<class K, class V>
		struct RBTreeNode
		{
			K _key;
			V _value;
			
			RBTreeNode<K, V>* _left;
			RBTreeNode<K, V>* _right;
			RBTreeNode<K, V>* _parent;
		};
		
		
```


<br>

### 红黑树的性质：

	1. 每个节点，不是红色就是黑色的
	2. 根节点是黑色的
	3. 如果一个节点是红色的，则它的两个子节点是黑色的
	4. 对每个节点，从该节点到其所有后代叶节点的简单路径上，均包含相同数目的黑色节点。
	5. 每个叶子节点都是黑色的（这里的叶子节点是指的NIL节点（空节点））

![image](http://images.cnitblog.com/i/497634/201403/251730074203156.jpg)

<br>

	思考：为什么满足上面的颜色约束性质，红黑树能保证最长路径不超过最短路径的两倍？
	
	

<br>

### 红黑树的代码：

```cpp

		# include <iostream>
		using namespace std;
		
		enum Color
		{
			RED,
			BLACK,
		};
		
		template<class T>
		struct RBTreeNode
		{
			T _key;
		
			RBTreeNode<T>* _left;
			RBTreeNode<T>* _right;
			RBTreeNode<T>* _parent;
		
			Color _color;
		
			RBTreeNode(const T& key)
				:_key(key)
				,_left(NULL)
				,_right(NULL)
				,_parent(NULL)
				,_color(RED)
			{}
		};
		
		
		template<class T>
		class RBTree
		{
			typedef RBTreeNode<T> Node;
		public:
			RBTree()
				:_root(NULL)
			{}
		
			void InorderRBTree()
			{
				_InorderRBTree(_root);
				cout << endl;
			}
		
		protected:
			void _InorderRBTree(Node* root)
			{
				if (NULL == root)
					return;
		
				_InorderRBTree(root->_left);
				cout << root->_key << " ";
				_InorderRBTree(root->_right);
			}
		
			void RotateR(Node*& parent)
			{
				Node* subL = parent->_left;
				Node* subLR = subL->_right;
		
				parent->_left = subLR;
				if (subLR)
					subLR->_parent = parent;
		
				subL->_right = parent;
				subL->_parent = parent->_parent;
				parent->_parent = subL;
		
				parent = subL;
		
				Node* grandfather = parent->_parent;
		
				if (NULL == grandfather)
					_root = parent;
				else
				{
					if (parent->_key > grandfather->_key)
						grandfather->_right = parent;
					else
						grandfather->_left = parent;
				}
			}
		
			void RotateL(Node*& parent)
			{
				Node* subR = parent->_right;
				Node* subRL = subR->_left;
		
				parent->_right = subRL;
				if (subRL)
					parent->_right = subRL;
		
				subR->_left = parent;
				subR->_parent = parent->_parent;
				parent->_parent = subR;
		
				parent = subR;
		
				Node* grandfather = parent->_parent;
				if (NULL == grandfather)
					_root = parent;
				else
				{
					if (parent->_key > grandfather->_key)
						grandfather->_right = parent;
					else
						grandfather->_left = parent;
				}
			}
		
		public:
			bool Insert(const T& key)
			{
				if (NULL == _root)
				{
					_root = new Node(key);
					_root->_color = BLACK;
		
					return true;
				}
		
				Node* cur = _root;
				Node* parent = cur->_parent;
				while (cur)
				{
					if (cur->_key < key)
					{
						parent = cur;
						cur = cur->_right;
					}
					else if (cur->_key>key)
					{
						parent = cur;
						cur = cur->_left;
					}
					else
						return false;
				}
		
				cur = new Node(key);
				if (parent->_key > key)
				{
					parent->_left = cur;
					cur->_parent = parent;
				}
				else
				{
					parent->_right = cur;
					cur->_parent = parent;
				}
		
				//调整颜色
				while (cur != _root && parent->_color == RED) //cur不是根且parent为红色，则parent不是root
				{
					Node* grandfather = parent->_parent;      //所以grandfather一定存在
		
					if (parent == grandfather->_left)                   //需要右旋  
					{
						Node* uncle = grandfather->_right;
						if (uncle && uncle->_color == RED)
						{
							parent->_color = uncle->_color = BLACK;
							grandfather->_color = RED;
		
							cur = grandfather;
							parent = cur->_parent;
						}
						else
						{
							//1.左右双旋
							//2.右单旋
							if (cur == parent->_right)
							{
								RotateL(parent);
								swap(parent, cur);         //旋转之后parent和cur的位置改变了
							}
		
							cur->_color = grandfather->_color = RED;
							parent->_color = BLACK;
							RotateR(grandfather);
						}
					}
					else                                                //需要左旋
					{
						Node* uncle = grandfather->_left;
		
						if (uncle && uncle->_color == RED)
						{
							parent->_color = uncle->_color = BLACK;
							grandfather->_color = RED;
		
							cur = grandfather;
							parent = cur->_parent;
						}
						else
						{
							//1.左右双旋
							//2.左单旋
							if (cur == parent->_left)
							{
								RotateR(parent);
								swap(parent, cur);         //旋转之后parent和cur的位置改变了
							}
		
							cur->_color = grandfather->_color = RED;
							parent->_color = BLACK;
							RotateL(grandfather);
						}
					}
				}
		
				_root->_color = BLACK;
		
				return true;
			}
		
		private:
			Node* _root;
		};
		
		
		
```


<br>

### 红黑树的应用：

	
	
	
	
	
	
	


	
	
