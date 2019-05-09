## C/C++
C/C++学习笔记

关于STL
+ vector
  + 1关于vector初始的capacity不够的如何处理
```
//windows下的增长，如果不够,多分配原先的50%，至少+1.
	void push_back(value_type&& _Val)
		{
    ...
				_Reserve(1);
    ...
		}
    
	void _Reserve(size_type _Count)
		{	// ensure room for _Count new elements, grow exponentially
		if (_Unused_capacity() < _Count)
			{	// need more room, try to get it
			if (max_size() - size() < _Count)
				_Xlen();
			_Reallocate(_Grow_to(size() + _Count));
			}
		}
    
	size_type _Grow_to(size_type _Count) const
		{	// grow by 50% or at least to _Count
		size_type _Capacity = capacity();

		_Capacity = max_size() - _Capacity / 2 < _Capacity
			? 0 : _Capacity + _Capacity / 2;	// try to grow by 50%
		if (_Capacity < _Count)
			_Capacity = _Count;
		return (_Capacity);
		}
```
```
//linux下增加一倍 代码不贴了 懒
```

  + 2vector什么时候释放分配的内存
    destory释放数据,只调用<>内部数据的析构，并移除该位置，不释放vec分配的空间.
    只有调用析构函数才会释放对应的存储空间，故对于越来越大的vector采用swap一个空vector能更好的减少内存占用
 ```
    template<class _Ty> inline
	_Ty *_Allocate(size_t _Count, _Ty *)
	{	// allocate storage for _Count elements of type _Ty
	void *_Ptr = 0;

	if (_Count == 0)
		;
	else if (((size_t)(-1) / sizeof (_Ty) < _Count)
		|| (_Ptr = ::operator new(_Count * sizeof (_Ty))) == 0)
		_Xbad_alloc();	// report no memory

	return ((_Ty *)_Ptr);
	}

  void deallocate(pointer _Ptr, size_type)
  {	// deallocate object at _Ptr, ignore size
  ::operator delete(_Ptr);
  }
 ```
 
 ## 数据结构个之红黑树
 
