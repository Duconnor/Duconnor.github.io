---
layout: post
title: C++——函数返回引用类型
data: 2018-05-24 14:26
catagories: C++
---

## 当函数的返回值为引用时

首先需要注意的是，不能为局部变量的引用，因为这些变量的引用在函数结束调用后会变为垃圾值。局部变量包括  
* 函数内部定义的变量  
* 以值拷贝形式传入函数的参数  
函数的返回类型为引用类型，则声明的变量也要是引用类型，如  
{% highlight c++ %}
int &func1();

int &i = func1(); // 此处i也要声明为引用类型，否则还是值传递
{% endhighlight %}  
上述代码中，func1的返回类型为int型的引用，因此在利用其为整型变量初始化值时，也应当声明这个变量为引用类型

想到这个主要是在写二叉搜索树的插入和删除算法时，利用对于指针的引用，可以在不知道一个结点双亲结点的条件下进行插入操作和删除操作，具体实现时，如下代码所示  
{% highlight c++ %}
template<class T>
class BST {
private:
    TreeNode<T> *root;
    int numOfElements;

    TreeNode<T> *&search(T &element, TreeNode<T> *&root);

    void erase(T &element, TreeNode<T> *&node);

    void preOrderTraversal(TreeNode<T> *root, T *elePreOrder, int &indexNow);

public:
    BST() = default;

    BST(T *array, int size); // construct a BST from an array
    bool insert(T &element);

    TreeNode<T> *&search(T &elemet);

    void erase(T &element);

    void preOrderTraversal();
};
{% endhighlight %}  
上述代码中，我们定义search函数的返回值为指针的引用，这样在insert操作中先调用search后便可以直接利用与指向待插入结点的指针“同名”的指针进行操作。需要特别注意的是，在search函数中，第二个参数定义为指针的引用便是为了返回时不会返回的是局部变量的引用。