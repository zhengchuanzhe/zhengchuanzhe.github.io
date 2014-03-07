---
title: 约瑟夫环问题
description: 递推解决约瑟夫环问题
keywords: suanfa
layout: post
tags: [算法]
---

   原来做约翰夫环类问题时都是进行模拟，今天看到一个方法用数学方法，递推得到结果的方法觉得挺好就写下来了。时间复杂度为o（n）

####一、问题描述####

   约瑟夫环是一个数学的应用问题：已知n个人（以编号1，2，3...n分别表示）围坐在一张圆桌周围。从编号为k的人开始报数，数到m的那个人出列；他的下一个人又从1开始报数，数到m的那个人又出列；依此规律重复下去，直到圆桌周围的人全部出列。

####二、思路####

   问题描述：n个人（编号0~(n-1）或者1~n，思路一样，注意代码有一点区别），从0开始报数，报到（m-1）的退出，剩下的人继续从0开始报数。求胜利者的编号。

   我们知道第一个人（编号一定是（m-1） mod n) 出列之后，剩下的n-1个人组成了一个新的约瑟夫环（以编号为k=m mod n的人开始）：

   k k+1 k+2 ... n-2,n-1,0,1,2,... k-2

   并且从k开始报0。

   我们把他们的编号做一下转换：

   k --> 0

   k+1 --> 1

   k+2 --> 2

   ...

   k-2 --> n-2

  变换后就完完全全成为了（n-1）个人报数的子问题，假如我们知道这个子问题的解：例如x是最终的胜利者，那么根据上面这个表把这个x变回去不刚好就是n个人情况的解吗？变回去的公式很简单：x'=(x+k) mod n

  如何知道（n-1）个人报数的问题的解？对，只要知道（n-2）个人的解就行了。（n-2）个人的解呢？当然是先求（n-3）的情况 ---- 这显然就是一个倒推问题！好了，思路出来了。

####三、代码####

C++代码：
   <pre class="code">
   
   #include<iostream>
   using namespace std;
   int main()
   {
	cout<<"请输入总人数n和报数m"<<endl;
	int n,m,s(0);
	cin>>n>>m;
	for(int i=2;i<=n;i++)
	{
		s=(s+m)%i;
	}
	cout<<"最后剩的人的编号"<<s<<endl;
	system("pause");
	return 0;
   }
   
   </pre>
   
   
  注意如果编号是1~n则最后s+1；或者写成：
    <pre class="code">
      for(int i=2;i<=n;i++)
      {
           s=((s+m)%i-1+i)%i; 
      }
    </pre>


  不能写成：
    <pre class="code">
      for(int i=2;i<=n;i++)
      {
           s=(s+m)%i+1; 
      } 
    </pre>