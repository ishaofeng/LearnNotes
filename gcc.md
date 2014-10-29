##gcc
---
###1.预处理

* 输出预处理到文件`gcc -E main.c -o main.i`
* 保留头文件注释`gcc -C -E main.c -o main.i`
* -Dname定义宏, -Uname取消宏定义
* -ldirectory 设置头文件搜索路径
* 查看依赖文件`gcc -M -I./lib main.c`
* 查看依赖文件(依赖标准库)`gcc -MM -I./lib main.c`

###2.汇编

* gcc -S main.c 生成汇编代码 main.s

###3.链接

* 参数-c仅生成目标文件(.o)
* -l链接其它库

###4.动态链接库

* 使用`-fPIC -shared`参数生成动态库
* 静态库 `ar rs libmy.a mylib.o`

###5.优化

* -O0关闭优化(默认)
* -O1让可执行文件更小,速度更快
* -O2采用所有优化手段
* -pg在程序中添加性能分析函数，用于统计程序中最耗时间的函数, 使用gprof命令查看
