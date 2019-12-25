# jdk 源代码笔记
## jdk 源代码目录

```
common
corba 
hotspot hotspot虚拟机的实现，大部分代码在这里，我们只需要关注这里即可
jaxp
jaxws webService的实现
jdk 自带的java代码，如：Unsafe, Crypto等Java
langtools
make 编译相关
nashorn
test
```

```
//hotspot的src目前可以看到有相关cpu和os的实现，这也是java跨平台的一个原因所在
hotspot\src\cpu
hotspot\src\os
hotspot\src\os_cpu
//我们主要关注share文件夹下面的代码
hotspot\src\share
```

```
大部分代码都在share文件夹下面的vm文件夹里面
hotspot\src\share\vm\adlc
hotspot\src\share\vm\asm
hotspot\src\share\vm\c1
hotspot\src\share\vm\ci
hotspot\src\share\vm\classfile  类相关的代码：类加载、类结构标识、常量池等
hotspot\src\share\vm\code  字节码相关：虚拟机寄存器、虚拟机程序计数器，虚拟机scope、
hotspot\src\share\vm\compiler 编译器
hotspot\src\share\vm\gc_implementation 垃圾收集器的实现
hotspot\src\share\vm\gc_interface 垃圾收集器接口
hotspot\src\share\vm\interpreter 解释器
hotspot\src\share\vm\libadt 
hotspot\src\share\vm\memory 内存管理：内存模型的具体实现、内存分配管理、内存分代等
hotspot\src\share\vm\oops Java对象的c++描述
hotspot\src\share\vm\opto 
hotspot\src\share\vm\precompiled
hotspot\src\share\vm\prims
hotspot\src\share\vm\runtime 运行时的基础依赖：虚拟机线程、虚拟机栈、栈帧、threadLocal的实现、mutex信号量、jni、原子类等
hotspot\src\share\vm\services
hotspot\src\share\vm\shark
hotspot\src\share\vm\trace
hotspot\src\share\vm\utilities
```