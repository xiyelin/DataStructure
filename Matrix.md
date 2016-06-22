##<a name="index"/>目录

* [对称矩阵](#对称矩阵)
	* [对称矩阵的压缩存储](#对称矩阵的压缩存储)
	* [代码](#代码)
* [稀疏矩阵](#稀疏矩阵)
	* [稀疏矩阵的压缩存储](#稀疏矩阵的压缩存储)
* [矩阵的转置](#矩阵的转置)
	* [代码](#代码)
* [矩阵的快速转置](#矩阵的快速转置)
	* [代码](#代码)



<br>

#### 对称矩阵
	
	对称矩阵，即a[i][j]=a[j][i]。一个 N x N 的对称矩阵，可以视为上三角或者下三角矩阵。


![image](http://hbimg.b0.upaiyun.com/3475bf5f4f2c75abee7cb11945136c87e0beba3b484c-edUG1B_fw658)


#### 对称矩阵的压缩存储

	用一个大小为 n(n+1)/2 的一维数组可以存储对称矩阵的上三角或者下三角。注意按照行优先存储，方便还原。

	
![image](http://hbimg.b0.upaiyun.com/7d5692349850431cb3c795d0476d2e0d0ed68ce995a4-4dRXKS_fw658)


#### 代码	

```cpp

	cpp code:
		
		
		
		
		
		
		
		
		
		
		
		
```


<br>

#### 稀疏矩阵

	M*N的矩阵，矩阵中有效值的个数远小于无效值的个数，且这些数据的分布没有规律。


![image](http://hbimg.b0.upaiyun.com/11b7a96f2de317596bee3b94e737806c65a1f1503a2b-xxMaCC_fw658)


#### 稀疏矩阵的压缩存储

	压缩存储值存储极少数的有效数据。使用{row,col,value}三元组存储每一个有效数据，三元组按原矩阵中的位置，
	
	以行优先级先后顺序依次存放。
	

![image](http://hbimg.b0.upaiyun.com/b5177e9adba536e4d471f2055f3638e4713ce6b98d7d-VQlJ4W_fw658)


```cpp

	三元组的结构为：
	
		struct Triple
		{
			int row;
			int col;
			int value;
		};
		
```

<br>

#### 矩阵的转置

	矩阵的转置即将矩阵的行和列对换。
	
![image](http://hbimg.b0.upaiyun.com/123e1f3e26bbb3950f5dfc9d0de519d434a9b150b18c-2Ul9Oi_fw658)


#### 代码

```cpp

	cpp code:
	
		
		
		
		
		
		
		
		
		
```


<br>

#### 矩阵的快速转置

	


	







