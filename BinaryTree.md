## 目录
+ [二叉树的定义](#二叉树的定义)
+ [二叉树的性质](#二叉树的性质)
+ [二叉树的节点](#二叉树的节点)
+ [二叉树的结构](#二叉树的存储结构)
+ [二叉树的代码](#二叉树的代码)
+ [二叉树的应用](#二叉树的应用)

--------------------------------------------


<br>

### 二叉树的定义

	定义：二叉树是每个节点最多有两个子树的树结构。通常子树被称作“左子树”（left_subtree）和“右子树”（right_subtree）;
	
	     或者称作“左孩子”和“右孩子”。二叉树常被用于实现二叉查找树和二叉堆。
		  

![image](http://hbimg.b0.upaiyun.com/9c037ee24cea26ea0ba35b1403351a84945271da4ce8-X2Kb4Y_fw658)


<br>

	二叉树的五种基本形态：
	
	
![image](http://hbimg.b0.upaiyun.com/e6de5e8b15f88f51788fca7f76cecab7f2bd1235a333-po1Ohh_fw658)


<br>

	二叉树的类型：

	(1)完全二叉树-------若设二叉树的高度为h，除第 h 层外，其它各层 (1～h-1) 的结点数都达到最大个数，第h层有叶子
	   结点，并且叶子结点都是从左到右依次排布，这就是完全二叉树。
	
	(2)满二叉树---------除了叶结点外每一个结点都有左右子叶且叶子结点都处在最底层的二叉树。
	
	(3)平衡二叉树-------平衡二叉树又被称为AVL树（区别于AVL算法），它是一棵二叉排序树，且具有以下性质：它是一棵空
	   树或它的左右两个子树的高度差的绝对值不超过1，并且左右两个子树都是一棵平衡二叉树。
	   

	   
<br>

## 二叉树的性质


	(1) 在非空二叉树中，第i层的结点总数不超过, i>=1；
	
	(2) 深度为h的二叉树最多有个结点(h>=1)，最少有h个结点；
	
	(3) 对于任意一棵二叉树，如果其叶结点数为N0，而度数为2的结点总数为N2，则N0=N2+1；
	
	(4) 具有n个结点的完全二叉树的深度为
	
	(5)有N个结点的完全二叉树各结点如果用顺序方式存储，则结点之间有如下关系：
	   		若I为结点编号则 如果I>1，则其父结点的编号为I/2；
			如果2*I<=N，则其左儿子（即左子树的根结点）的编号为2*I；若2*I>N，则无左儿子；
			如果2*I+1<=N，则其右儿子的结点编号为2*I+1；若2*I+1>N，则无右儿子。
			
	(6)给定N个节点，能构成h(N)种不同的二叉树。
			h(N)为卡特兰数的第N项。h(n)=C(2*n，n)/(n+1)。
			
	(7)设有i个枝点，I为所有枝点的道路长度总和，J为叶的道路长度总和J=I+2i。  


<br>

### 二叉树的节点

```cpp

			struct BinaryTreeNode
			{
				DataType _value;              //节点值
				BinaryTreeNode* _left;        //左子树
				BinaryTreeNode* _right;       //右子树
			};
			

```


<br>

### 二叉树的存储结构


![image](http://hbimg.b0.upaiyun.com/ec607a0fda8c716f36e1c93b14d7f66440ab172b99439-f9zGLc_fw658)



<br>

### 二叉树的代码


```cpp

	cpp code:
		
		
		
		
		
		
		
		
		
		
```


<br>

### 二叉树的应用

	1.文件系统---目录树
	
	2.Java集合中的TreeSet和TreeMap，C++ STL中的set、map，以及Linux虚拟内存的管理，都是通过红黑树去实现的。
	


	扩展：计算机中的树
	
![image](http://hbimg.b0.upaiyun.com/402c6a6b5aef28922ae1ffdacfd44d71211cea2f3bc0-kqbHUm_fw658)







	