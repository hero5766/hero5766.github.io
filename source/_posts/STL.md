---
title: STL
author: hero576
tags:
  - C/C++
categories:
  - programme
date: 2020-11-07 19:32:22
---
> 
<!--more-->

# 简介
## 介绍
- STL 是“Standard Template Library”的缩写，中文译为“标准模板库”。STL 是 C++ 标准库的一部分，不用单独安装。

- C++ 对模板（Template）支持得很好，STL 就是借助模板把常用的数据结构及其算法都实现了一遍，并且做到了数据结构和算法的分离。

功能|说明
-|-
容器|顺序容器：vector、deque、list<br>关联容器：set、multiset、map、multimap
迭代器|前向迭代器、双向迭代器、随机访问迭代器
算法|find、find_if、reverse、transform

# string类
- 使用时引用：`#include <string>`

## 操作

操作|c|c++
-|-|-
声明|`char cname[20]="123";`<br>`char *cname="123";`|`string name("123");`<br>`string name="123";`
转化|c++->c:`char* cname=name.c_str();`|c->c++：`const char* cname="cchar";string name(cname);`
复制|`const char* cname="123";`<br>`char* cname2=new char(strlen[cname]+1);`<br>`strcpy(cname,cname2);`<br>`delete[] cname2;`|`string name2(name);`<br>`复制一部分：string name3(name,3)`<br>复制10个a，并初始化到name`string name(10,'a');`
迭代|-|传统方法：`string name="123";for(size_t i=0;i< name.length();++i){cout<< name[i]<< endl;}`<br>使用迭代器:`string::const_interator it;for(it=name.begin();it!=name.end();++it){cout<<*it<<endl;}`
连接|-|`name+=name2;`<br>`name.append(name3);`<br>c风格的字符拼接`name.append(cname);`
查找|-|从起始位置0开始查找1个，未找到则返回-1(string::npos)`size_t index = name.find("day",0);if(size_t!=string::npos){}`<br>查找多个`size_t index = name.find("day",0);while(size_t!=string::npos){index=name.find("day",index+1)}`
截短|-|`name.erase(13,28);`

## 算法
- 需要引用：`#include <algorithm>`

算法|说明
-|-
反转|`reverse(name.begin(),name.end());`
大小写转换|`transform(name.begin(),name.end(),name.begin(),::toupper);`toupper,tolower


# vector类
- 引用：`#include<vector>`
- 命名空间：`using std::vector;`

## 操作

操作|说明
-|-
实例化|`vector<int> a;`类型可以使用任意的类型，在编译时会生成对应的vector类<br>`vector<int> b(10,2);`b中有10个2，不写2则为0
插入|后插`a.push_back(1);`
大小|`a.size()`
下标|`a[0]`要用配套的`vector<int>::size_type`类型作为下标
复制|`vector<int> a(b);`要保证类型相同
删除|`a.pop()`


# deque类

操作|说明
-|-
实例化|`deque<int> a;`
插入|后插`a.push_back(1);`<br>前插`a.push_front(1);`
弹出|`a.pop_front();a.pop_back();`


# set、multiset类
- 引入：`#include <set>`
- set不允许重复，插入数据会自动排序
- multiset可以重复
- 可以找到成员，但不能通过迭代器更改

操作|说明
-|-
实例化|`set<int> a;`<br>`multiset<int> ma;`
插入|`a.insert(1);`<br>`ma.insert(a.begin(),a.end());`
统计数量|`ma.count(1);`
遍历|`for(set<int>::const_iterator i=a.begin();i!=a.end();++i){cout<<*i<<endl;}`
查找|`set<int>iterator i=a.find(-1);`
删除|`a.erase(1);`
清空|`a.clear();`

# map、multimap类
- 引入：`#include <map>`
- map键不允许重复，multimap可以重复
- 不允许修改

操作|说明
-|-
实例化|`map<int,string> a;`<br>`multimap<int,string> ma;`
插入|`a.insert(map<int,string>::value_type(1,"one"));`<br>`a.insert(make_pair(-1,"minus one"))`<br>`a.insert(pair<int,string>(1000,thousand)`<br>`a[10000]="millons"`这种方法只能用于map<br>`ma.insert(a.begin(),a.end());`
查找|`map<int,string>::const_iterator i = a.find(1);`
迭代|`for (map<int,string>::const_iterator i=a.begin();i!=a.end() ;++i ){cout<<"key:"<<i->first<<"\tval:"<<i->second<<endl;}`
统计数量|`ma.count(3);`
删除|`int status = ma.erase(1);`删除成功返回>0<br>`ma.erase(ma.begin());`<br>`ma.erase(ma.lower_bound(100),ma.upper_bound(1000));`

# stack类
- LIFO结构，引用:`#include<stack>`

操作|说明
-|-
实例化|`stack<int,deque<int>>a;`默认<br>`stack<int,vector<int>>b;`<br>`stack<int,list<int>>c;`
判断是否为空|`a.empty()==false;`
检查大小|`a.size();`
弹出不返回|`a.pop();`
查看栈顶并返回|`a.top();`
压入堆栈|`a.push(item);`

# queue类
- 队列FIFO，引入：`#include<queue>`

操作|说明
-|-
实例化|`queue<int,deque<int>>a;`默认<br>`queue<int,list<int>>b;`
判断是否为空|`a.empty()==false;`
检查大小|`a.size();`
查看队首|`a.front();`
查看队尾|`a.back();`
队首弹出不返回|`a.pop();`
压入堆栈|`a.push(item);`


# priority_queue类
- 最大值优先级队列，最小值优先级队列
- 引入：`#include<priority_queue类>`

操作|说明
-|-
实例化|`priority_queue类<int,deque<int>,less<int>>a;`默认最大值优先级队列，`greater<int>`最小值<br>`priority_queue类<int,vector<int>>b;`
判断是否为空|`a.empty()==false;`
检查大小|`a.size();`
查看队首|`a.top();`
队首弹出不返回|`a.pop();`
压入堆栈|`a.push(item);`

# 函数对象
- 函数对象就是重载调用操作符的类，在STL中很多算法中被使用。相比函数，函数对象可以保留参数状态，使用更灵活强大。
- class和struct一样，除了默认class是private，struct是public

```cpp
class abs{
public:
	int operator()(int val){
		return val < 0?-val:val;
	}
};
struct abs2{
public:
	int operator()(int val){
		return val < 0?-val:val;
	}
};
int main(){
	abs obj;
	cout<<obj(-32)<<endl;
	return 0;
}
```

- 带模板的函数对象
```cpp
template<typename elemType>
struct DisplayElem{
	void operator()(const elemType & elem)const{
		cout<<elem<<endl;
	}
};
int main(){
	vector<int>a;
	a.push_back(1);
	list<char>b;
	b.push_back(1);
	for_each(a.begin(),a.end(),DisplayElem<int>());
	return 0;
}
```

- 函数对象一个参数，则称之为一元函数，返回是bool类型，则称之为一元谓词
- 函数对象两个参数，则称之为二元函数，返回是bool类型，则称之为二元谓词

```cpp
template<typename numberType>
struct IsMultiple{
	numberType m_Divisor;
	IsMultiple(const numberType& divisor){
		m_Divisor = divisor;
	}
	bool operator()(const numberType& elem)const{
		return (elem%m_Divisor==0);
	}
};
int main(){
	vector<int> vec;
	for (int i=0;i<100 ;++i ){
		vec.push_back(1+i*3);
	}
	IsMultiple<int>a(4);
	vector<int>::iterator elem = find_if(vec.begin(),vec.end(),a);
	while(elem!=vec.end()){
		cout<<"elem:"<<*elem<<endl;
		elem = find_if(++elem,vec.end(),a);
	}
	return 0;
}
```

```cpp
template<typename elemType>
struct CMultiple{
	elemType operator()(const elemType& elem1,const elemType& elem2){
		return elem1*elem2;
	}
};
int main(){
	vector<int>a,b,vec;
	for (int i=0;i<10 ;++i ){
		a.push_back(i);
		b.push_back(100+i);
	}
	vec.resize(10);
	transform(a.begin(),a.end(),b.begin(),vec.begin(),CMultiple<int>());
	for (size_t i=0;i<vec.size() ;++i ){
		cout<<vec[i]<<endl;
	}
	return 0;
}
```

- 二元谓词
```cpp
struct CompareNoCase{
	bool operator()(const string& str1,const string& str2)const{
		string str1Lower,str2Lower;
		str1Lower.resize(str1.size());
		str2Lower.resize(str2.size());
		transform(str1.begin(),str1.end(),str1Lower.begin(),::tolower);
		transform(str2.begin(),str2.end(),str2Lower.begin(),::tolower);
		return (str1Lower==str2Lower);
	}
};
int main(){
	set<string,CompareNoCase>names;
	names.insert("1a");
	names.insert("2b");
	set<string,CompareNoCase>::iterator iter = names.find("2A");
	if (iter!=names.end()){
		cout<<"found"<<endl;
	}else{
		cout<<"not found"<<endl;	
	}
	return 0;
}
```

## 预定义函数对象
- 引用：`#include<functional>`

预定义|说明
-|-
negate<type>()|取负值
plus<type>()|
minus<type>()|
multiplies<type>()|
divides<type>()|
modulus<type>()|取模运算
equal_to<type>()|
not_equal_to<type>()|
less<type>()|
greater<type>()|
less_equal<type>()|
greater_equal<type>()|
logical_not<type>()|
logical_and<type>()|
logical_or<type>()|

## 预定义函数适配器
- 将函数对象和值绑定起来

预定义|说明
-|-
bind1st(op,value)|
bind2nd(op,value)|
not1(op)|
not2(op)|
mem_fun_ref(op)|
mem_fun(op)|
ptr_fun(op)|


# bitset类
- 二进制的运算，引入：`#include<bitset>`

bitset|说明
-|-
初始化|`bitset<32>a;`<br>`bitset<16>a(0xffff);`<br>`bitset<128>a(0xffff);`<br>`bitset<32>a(100);`<br>`bitset<16>a("111101101",5,4);`
检查至少有一个1|`a.any()`
一个1都没有|`a.none()`
1的个数|`a.count()`
位数|`a.size()`
0的个数|`a.size()-a.count()`
更改|`a[5]=1`
置0|`a.reset()`<br>指定某位置零`a.reset(5)`
翻转|`a.flip()`<br>指定某位翻转`a.flip(1)`
置1|`a.set()`
转为10进制|`unsigned long b = a.to_ulong()`
位与|`a&b`，未操作的优先级别比较低
位或|`a|b`
位异或|`a ^ b`

# vector<bool>
- bitset的缺点是大小声明的时候固定了，而vector<bool>是动态的

vector<bool>|说明
-|-
初始化|`vector<bool>x(3)`
插入|`x[0]=true;x[1]=true;x[2]=true;x.push_back(true);`
翻转|`x.flip()`


# 算法

非变续算法|说明
-|-
计数算法|count()、count_if()
搜索算法|search()、find_end()、search_n(b,e,c,v[,p])、search_n_if(b,e,c,p)、find()、find_if()、find_first_of(b,e,sb,se[,bp])、adjacent_find(b,e[,p])
搜索算法(已排序)|binary_search(b,e,v[,p])、includes(b,e,v[,p])、lower_bound()、upper_bound()
区间比较算法|equal(b,e,b2[,p])、mismatch(b,e,b2[,p])、lexicographical_compare(b,e,b2[,p])
最值算法|min_element(b,e[,op])、max_element(b,e[,op])

变续算法|说明
-|-
删除算法|remove()、remove_if()、remove_copy()、remove_copy_if()、unique()、unique_if()
修改算法|for_each()、transform()、copy()、copy_backward()、merge()、swap_range()、fill()、fill_n()、generate()、generate_n()、replace()、replace_if()、replace_copy()、replace_copy_if()、
排序算法|sort()、stable_sort()、partial_sort()、partial_sort_copy()、nth_element()、partition()、stable_partition()
堆排序|make_heap()、push_heap()、pop_heap()、sort_heap()
排列组合|next_permutation()、prev_permutation()、reverse()、reverse_copy()、rotate()、rotate_copy()、random_shuffle()

## 算法举例

算法|说明
-|-
计数算法|`size_t c=count(a.begin(),a.end(),1);`<br>`size_t c=count(a.begin(),a.end(),1,isEven<int>);`最后一个是谓词
搜索算法|`iter = find(a.begin(),a.end(),3);`
搜索算法|`iter = search(a.begin(),a.end(),s.begin(),s.end());`查找多个数据




# 例子
## 简单使用
```cpp
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;
int main(){
	int a;
	vector<int>v;
	v.push_back(50);
	v.push_back(10);
	v.push_back(523);
	v.push_back(534);
	vector<int>::iterator i = v.begin();
	while(i!=v.end()){
		cout<<*i<<endl;
		++i;
	}
	vector<int>::iterator ifind = find(v.begin(),v.end(),534);
	if(ifind!=v.end()){
		int npos=distance(v.begin(),ifind);
		cout<<"num:"<<*ifind<<endl;
		cout<<"distance:"<<npos<<endl;
		++i;
	}	
	return 0;
}
```


## 通用打印函数
```cpp
template<typename Container>
void Print(const Container & c){
	for (Container::const_iterator i=c.begin;i<c.end() ;i++ )
	{
		cout<<*i<<endl;
	}
}
```

