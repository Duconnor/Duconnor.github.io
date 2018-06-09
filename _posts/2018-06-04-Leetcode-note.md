---
layout: post
title: Leetcode刷题随笔
date: 2018-06-04 9:36
catagories: algorithm
---

## 对于动态规划的优化  
动态规划，如果不加任何优化的话，从本质上来讲就是枚举算法，对于动态规划的优化主要是利用memory table来将已经计算过的结果存储起来，避免后面的重复计算

## 对于不同进制之间的转化
* Leetcode 168 *
对于10进制，如果需要将其转化为k进制，则按照如下步骤进行操作  
* 取该数在十进制乘法下对k的模m，即为转换后最低位的数字  
* 将该数除以k，取整数部分，递归调用转换函数

## C++STL中的unique()函数  
这个函数的作用是去除vector中（或者其它容器中）**相邻** 位置的重复元素，而且其只是将重复的元素移到了队尾并没有删除，因此想要使用这个函数来删除重复元素需要按照如下使用方式  
{% highlight c++ %}
#include <vector>
#include <algorithm>

using std::vector;

int main() {
	vector<int> vec = {1, 2, 3, 1};
	std::sort(vec.begin(), vec.end());
	vec.erase(std::unique(vec.begin(), vec.end()), vec.end());
	return 0;
}
{% endhighlight %}  
上述代码中，由于vec中的重复元素“1”没有处在相邻的位置，如果直接使用unique的话没有任何改变发生，因此需要先排序，将重复的元素集中在一起。unique函数返回的是第一个重复元素的位置，因此可以直接利用erase函数将重复元素删除。
