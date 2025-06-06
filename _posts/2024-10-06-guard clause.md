---
published: true
---
# 卫函数

* 卫函数 guard clause
  大量return是比嵌套更恶心的东西 经验之谈
* 使用do while(false)
* goto
* Pattern matching才是真扁平, eager return的作用只是分离edge cases, 可读性强点, 但是逻辑还是嵌套. 一旦分支间的权重比较均匀的时候, 这种写法就看不到好处了.
* 现在这种c++11下用lambda+return更方便，do while（0）还是更多用在宏里
* 答案是不return而是 raise 错误，要稳定运行就函数外面加异常捕获
* 我只会推荐上状态机来管理
* 最好用策略或者状态，增加点复杂度，但是可读性和可维护性都要强很多。
  if嵌套 策略
  https://www.bilibili.com/video/BV1r2xueXEQG/
  
---
## guard clause

s2 C里面这样随意return可不太行，尤其是函数顶端有用heap的地方。
这种得用宏封装一个资源释放，然后每个return前都调用一遍...还得用宏封装一个错误处理...具体工程反正都会很麻烦就是了

s1 C也可以 把资源申请的写在外函数
内函数不牵扯任何资源申请
就能改成这种方法了

s2 你说的当然有道理，但是这是由局限性的，外层申请资源有时候需要传二级指针，而且不是所有情况都能这样做


提前判断错误提前返回能消除很多嵌套。不过越往后写，就越需要记住前面之所以没有提前返回的原因，避免在后面又重复判断前面判断过的事情
大量return是比嵌套更恶心的东西 经验之谈
因为不能一眼看出这个函数是在哪里结束的，你需要断点一句一句的查看
单return可以在你第一眼看这个函数时能明确清楚这个函数的结束点，但是多return不行，当然找到真正的问题所在还是要靠调试断点
嵌套if在when的表达上比平面return更具可读性，并且能够最大程度获得静态类型的约束和动态类型的推断。但是嵌套if和大量平面return都应该是尽量避免的，因为两者都不具备良好的可读性
最终只return符合预期的结果，不符合预期的应该用throw而不是return

---
使用do{...}while(false)结构可以简化多级判断时代码的嵌套。

举个例子：现在要实现一个功能，但需要A、B、C、D四个前提条件，并且这四个前提条件都存在上级依赖，即B依赖于A，C依赖于A和B，D依赖于A、B和C。如果按照一般的写法，是这样：
```c
if( A==true )
{
    if( B==true )
    {
        if( C==true )
        {
            if( D==true )
            {
                //实现功能代码
            }
        }
    }
}
    可能看出来
```

一种解决的办法是使用goto语句，
```c
if( A==false )
    goto tag;
if( B==false )
    goto tag;
if( C==false )
    goto tag;
if( D==false )
    goto tag;
//实现功能代码
 
tag:
...
```

用do while语句也可以实现类似goto的功能
```c
do
{
    if( A==false )
        break;
    if( B==false )
        break;
    if( C==false )
        break;
    if( D==false )
        break;
    //实现功能代码
}while(false);
...
```
  https://blog.csdn.net/this_capslock/article/details/41843371
