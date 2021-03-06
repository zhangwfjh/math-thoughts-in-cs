# 函数

在Julia中，函数不仅包括狭义上的数学函数，根据自变量返回因变量，还包括运算过程，即一系列计算的集合。

## 基本数学函数

| 函数 | 说明 | 示例 |
| -- | -- | -- |
| ```e, pi, im``` | 自然对数底，圆周率，虚单位 | ```e,pi,2im```返回```(e = 2.7182818284590...,π = 3.1415926535897...,0 + 2im)``` |
| ```round, floor, ceil``` | 四舍五入, 向下取整, 向上取整 | ```round(1.5), floor(1.5), ceil(1,5)```返回```(2.0,1.0,2.0)``` |
| ```div, rem, mod, mod1``` | 整除，余数 | ```div(5,2), rem(5,2), mod(4,2), mod1(4,2)```返回```(2,1,0,2)``` |
| ```abs, abs2, sign``` | 绝对值，绝对值平方，符号值 | ```abs(-2), abs(-2), sign(-2), sign(0), sign(2)```返回```(2,4,-1,0,1)```
| ```sqrt, exp, log, log10``` | 平方根，立方根，自然指数，自然对数/对数，底10对数 | ```sqrt(16), exp(1), log(e^3), log(3,81), log10(100)```返回```(4.0,2.718281828459045,3.0,4.0,2.0)``` |
| ```sin/sind/sinpi, cos/cosd/cospi, tan/tand``` | 三角函数 | ```sin(pi/2), cospi(1), tand(90)```返回```(1.0,-1.0,Inf)``` |

## 函数定义

通常，为了实现一些较为复杂且需要重复使用的功能，我们可以自定义函数。函数主要分为以下几个部分，函数名、函数参数、函数体和返回值。

### 简单函数

简单函数通常实现较为简单的功能，定义也非常直接，如
```
julia> p(x)=2x^4-3x+1; p(2)
27
julia> dist(u,v)=sqrt(u^2+v^2); foo(3,4)
5.0
julia> mean(x,y)=((x+y)/2,sqrt(x*y)); mean(2,8)
(5.0,4.0)
```
注意，在一个语句末尾添加分号；可以抑制不必要的输出。有时，为了语法需要，可以用括号将多个语句括起来(;)作为一个语句，叫做**链语句**。例如
```
julia> S(a,b,c)=(p=(a+b+c)/2; sqrt(p*(p-a)*(p-b)*(p-c)); S(3,4,5)
6.0
```

### 块函数

当一个函数有较多中间步骤时，常使用块函数。块函数以function关键字开头，end关键字结尾，通过return关键字返回函数值，并结束函数计算（即return之后的任何语句都将被忽略）。请看下例
```
julia> function line(x1,y1,x2,y2)
         k = (y2-y1)/(x2-x1)
         b = y1-k*x1
         f(x) = k*x+b
         return f
       end;
julia> l = line(1,2,2,3);
julia> l(1), l(2), l(3)
(2.0,3.0,4.0)
```
从上例我们发现，函数返回值不必要是数，还可以是函数。

### 匿名函数

同样，函数的参数也可以不是数，而是函数。通常，为了避免不必要的定义，我们可以使用匿名函数。匿名函数用```->```定义，请看
```
julia> eval(f,x) = f(x);
julia> eval(x->x+1, 2), eval(x->x*x, 3)
(3,9)
julia> ((x,y)->x*y)(2,3)
6
```

## 函数参数

### 可选参数

函数的可选参数常用于设定函数的默认表现，当该参数被设置时，函数按照该参数设置进行计算；若未被设置，则函数按默认设置进行计算。注意，可选参数必须放在非可选参数之后。请看下例
```
julia> linear(k,b=0,x=1) = k*x+b;
julia> linear(1,2,3), linear(1,2), linear(1)
(5,3,1)
```

### 关键字参数

有时，一个函数可能含有多个参数，为了方便设置参数，可以给参数设置一个关键字，则调用时可以将该参数放置在非关键字参数后的任意位置。请看下例
```
julia> quadratic(a; axis=0, height=0) = x->a*(x-axis)^2+height;
julia> quadratic(1,height=3)(0), quadratic(2, height=0, axis=1)(2)
(3,2)
```

### 变长参数

变长参数用于函数参数个数不确定，但不影响计算方法的函数。定义时在变长参数后面添加```...```，在传入列表参数时，也需在参数后面添加```...```。请看下例
```
julia> ktimes(k,xs...)=k*sum(xs);
julia> ktimes(2,1), ktimes(2,1,2,3)
(2,12)
julia> ktimes(2,[1,2,3]), ktimes(2,[1,2,3]...)
([2,4,6],12)
```
