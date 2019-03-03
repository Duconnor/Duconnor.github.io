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

## 74.Search a 2D Matrix
该题目为的是快速地在二维矩阵中搜索目标值，为了达到高速搜索的目的，容易想到的便是使用binary search，但是需要注意，如果将二维矩阵的所有元素拉通进行查找不利于充分使用cache，因此考虑一行一行进行搜索，同时需要注意的是，每行在进行搜索时，可以通过判断第一个以及最后一个元素与目标值的大小来减少搜索次数

## 75.Sort Colors
该题目表明，如果要求是排序，也不一定是要真正地去执行排序的动作（例如交换），可以采用计数+直接赋值的方法实现

## 77.Combination
在计算Combination时，可以考虑利用已经计算的结果生成新的结果，而不是每次都重新计算。例如，结果[2,3,4]，稍微进行修改便可以得到新的结果[1,3,4]，这样可以有效地提高程序的效率

## 78.Subsets
所有这种类似的题目，例如寻找所有可能的Combination、Permutation等等，都可以利用一个通用的模版，即backtracking的思路。具体为：使用一个循环，通过每次新加入一个元素，然后递归调用，每次递归时率先判断是否满足条件，若满足则将其加入最后结果中，递归调用的函数返回后删除刚刚加入的元素，并进入下轮循环，具体做法可以参考[Leetcode中的这篇文章](https://leetcode.com/problems/subsets/discuss/27281/A-general-approach-to-backtracking-questions-in-Java-(Subsets-Permutations-Combination-Sum-Palindrome-Partitioning))

## 79.Word Search
该题目是一个寻常的，利用回溯法解决的搜索类型的题目，但有一点值得注意以下，即对于这种每一步有多种可能的题目时，我们可以选择先判断，然后只去递归那些符合条件的，也可以选择先递归，然后在每个递归函数的一开始判断是否符合要求。显然，采用第二种方法会简化代码，值得尝试
