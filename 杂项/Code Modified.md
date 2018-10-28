# Code Modified

++++++

形式不重要，重要的是匹配的程度。

比如一个m*n的vector 被resize 到 m\*n，那么我们可以这样计算下标：

1维vector的第k个等于原来的m,n中的第$(int(k/n)+1,k \%n+1)$个

(当然，以0为开头就是 $(int(k/n),k\%n)$)个，这样就是拉长的算法

那比如k\*m\*n 呢？

很简单啊，就是一个递归嘛

相当于是先把(m,n)拉长成m\*n,然后把这个再和k拉长，假设(k,m,n)拉长到了k\*m\*n,那么对其中的元素count，我们应该如何找原来的下标呢？

简单：(Tested and works)

```python
k_=int(count/(m*n));m_n=count%(m*n)
m_=int(m_n/n);n_=m_n%n
```

其它其实照旧，也就是说这个本质是一个递归的过程(以上代码对于`torch.view`适用,对`numpy` 也适用)



