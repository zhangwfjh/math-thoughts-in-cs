# 控制流

在命令式语言中，程序的基本执行结构有三类：顺序结构、条件结构和循环结构，分别对应顺序执行、选择执行和重复执行。

## 复合语句

顺序结构指逐条按照指令执行，顺序结构也是Julia的默认执行顺序。有时，可以将多条语句组合起来作为一条指令，称为**复合语句**。在上一节中我们见到的**链语句**便是复合语句的一种，链语句通常写在同一行中。若多条语句不在同一行，可使用**块语句**。块语句以begin开头，end结尾，返回end之前最后一个语句的值。例如
```
julia> a,b,c = 1,-3,2;
julia> x1, x2 = begin
                  D = b^2-4a*c
                  t = -b+sqrt(D)
                  2c/t, t/2a
                end
(1.0,2.0)
```

## 条件语句

条件语句根据条件的真假判断是否执行语句。在Julia中，条件语句有多种形式。

### ```if-elseif-else```语句

```if-elseif-else``` 语句是最常见的条件语句，从第一个if开始判断，如果成立，则执行相应语句，之后整个if语句结束；若不成立则依次判断下一个elseif；如果所有if均不成立，则执行else相应语句；若不存在else语句，则不执行。

```
julia> x = 75;
julia> if x > 60 println("及格") end
及格
julia> if x >= 90
         println("优秀")
       else
         println("尚需努力")
       end
尚需努力
julia> if x >= 90
         println("优秀")
       elseif x >= 80
         println("良好")
       elseif x >= 60
         println("及格")
       else
         println("不及格")
       end 
及格
```

if语句还可以嵌套使用，请看下例
```
julia> x = 75;
julia> if x >= 60
         if x >= 80 println("[80,100]")
         else println("[60,80]")
         end
       else
         println("[0,60)")
       end
[60,80]
```

### ```?:```语句

```?:``` 语句可以看作是```if-else```语句，请看例子
```
julia> eveness(x) = x%2 == 0 ? true : false;
julia> eveness(1), eveness(2)
（false, true)
```

### ```ifelse```函数

```ifelse``` 函数区别于```if-else```语句，可看作分段函数，例如
```
julia> f(x) = ifelse(x >= 0, x, -x);
julia> f(1), f(-2)
(1,2)
```

### &&和||

回忆之前提到，合取&&和析取||具有短路现象，即当且仅当第一个操作数不能确定结果时才计算第二个操作数。因此，&&相当于```if-then```，||则相当于```if not-then```。合取和析取的应用通常能简化代码，例如
```
julia> positive(x) = x>0 && true;
julia> nonnegative(x) = x<0 || false;
julia> negative(x) = x<0 && true || false;
julia> positive(1), nonnegative(1), negative(1)
(true,true,false)
```

## 循环语句

循环语句每次判断循环条件是否成立，若成立则执行循环体，否则退出循环。在Julia中有while和for两种循环。

### while循环

```
julia> i = 1;
julia> while(i<=10)
         print(i)
         i += 1
       end
12345678910
```
特别需要注意，在while的循环体中一定要有改变循环条件状态的语句，否则程序将进入**死循环**，最终导致程序崩溃。

### for循环

for循环常用于遍历一个容器或一个区间的所有元素，对每一个元素进行相应计算，然后退出循环。例如
```
julia> for x in [2,0,3,0,4]
         print(x+1)
       end
31415
```

通常，区间集合{s,s+t,s+2t,…,s+nt<=e}可表示为```s:t:e```（若t为1，则可表示为```s:e```)，则可用for循环遍历区间如下
```
julia> for x = 1:2:10
          print(x+1)
       end
246810
```

有时，给遍历的元素加入一个序号能给程序带来许多方便，Julia允许我们使用`enumerate`函数实现这一点。
```
julia> for (i,x) in enumerate("abc")
         println("$i=>$x")
       end
1=>a
2=>b
3=>c
```

在Julia中，for循环还可以遍历两个集合的笛卡尔积。
```
julia> for x=1:2, y=1:3
         println("$x×$y=$(x*y)")
       end
1×1=1
1×2=2
1×3=3
2×1=2
2×2=4
2×3=6
```

### continue, break语句

在循环体当中，有时需要对某个状态进行特殊处理。这时我们可以嵌套if语句进行特例判断，使用continue语句可以使循环跳过当前状态进行下一次循环。例如
```
julia> set = [0,1,3];
julia> for x in set, y in set
         y == 0 && continue
         println("$x÷$y=$(x/y)")
       end
0÷1=0.0
0÷2=0.0
1÷1=1.0
1÷2=0.5
2÷1=2.0
2÷2=1.0
```

有时，退出循环的条件需要在循环中确定，于是我们需要使用break语句来终止循环。请看例子
```
julia> x = 5;
       while(true)
         abs(x-sqrt(2)) < 1e-5 && break
         println(x)
         x -= (x^2-2)/2x
       end
5
2.7
1.7203703703703703
1.44145536817765
1.414470981367771
```