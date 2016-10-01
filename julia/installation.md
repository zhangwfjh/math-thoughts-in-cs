# 安装及使用

![](http://docs.julialang.org/en/release-0.5/_static/julia-logo.svg)

Julia可以通过官方网站([julialang.org](http://julialang.org/downloads/))直接下载安装即可，也可以通过注册登录([juliabox.com](https://www.juliabox.com/))直接在线使用。

实际上，任何一个文本编辑器都可以用于编程。为了提升效率和编程舒适度，我们推荐两个非常出色的编辑器，一个是[Atom](https://atom.io/)，一个是[Sublime](http://www.sublimetext.com/)。一些软件商甚至将代码、编译、调试和输出功能集成在一个编辑器中，称为**集成开发环境**，Julia官方推荐的集成开发环境为Juno([junolab.org](http://junolab.org/))。

双击Julia可执行文件或在命令行中输入```julia```即可进入Julia编程环境。输入```1+1```，然后回车，则julia即刻返回2。若要执行已经在文本编辑器中写完的程序，可以输入```include("文件名.jl")```，Julia程序默认的文件扩展名为.jl。输入```exit()```可以退出Julia编程环境。

若在Julia使用过程中想要查看某个函数的使用方法，可以首先输入```?```，再输入相应命令，即可获得使用帮助。也可以查看Julia的[使用文档](http://docs.julialang.org/)。我们还提供了一个[快速参考手册](https://trello.com/b/fppQ3unx/julia-manual)。