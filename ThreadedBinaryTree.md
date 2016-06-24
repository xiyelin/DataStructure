## 线索化二叉树(Threaded Binary Tree)
* [定义](#线索化二叉树的定义)
* [节点结构](#线索化二叉树的节点结构)
* [代码](#代码)
* [与普通二叉树的区别](#比较)


-----------------------------------------------------------------

<br>
#### 线索化二叉树的定义

    对于n个结点的二叉树，在二叉链存储结构中有n+1个空链域，这些空间并没有利用而是浪费掉了。
    
    
![image](http://hbimg.b0.upaiyun.com/b8120340d13428b9f279a9b669bcb7b5975bb276aac3-NWiIgT_fw658)    


    
    利用这些空链域存放在某种遍历次序下该结点的前驱结点和后继结点的指针，这些指针称为线索，加上线索的二叉树称为线索二叉树。
    
    根据线索性质的不同，线索二叉树可分为前序线索二叉树、中序线索二叉树和后序线索二叉树三种。
    
    
<br>

#### 线索化二叉树的节点结构


![image](http://hbimg.b0.upaiyun.com/acb9d3eb8e92dcbef26953af7e8d5485e52e6bb33dd-Tv7Nj1_fw658)


    ltag=1 时lchild指向左孩子；
    ltag=0 时lchild指向前驱；
    rtag=1 时rchild指向右孩子；
    rtag=0 时rchild指向后继；

```cpp

        enum PointerTag 
        {
        	THREAD,                         //值为0，指向前驱或者后继
        	LINK                            //值为1，指向孩子节点
        };
        
        template <class T>
        struct ThreadedBinaryTreeNode
        {
        	typedef ThreadedBinaryTreeNode ThrBTNode;
        
        	T _data;						// 数据
        	ThrBTNode<T>* _Left;			// 左孩子
        	ThrBTNode<T>* _Right;			// 右孩子
        	ThrBTNode _LeftTag;				// 左孩子线索标志
        	ThrBTNode _RightTag;			// 右孩子线索标志
        };
        
        
```



        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
        
