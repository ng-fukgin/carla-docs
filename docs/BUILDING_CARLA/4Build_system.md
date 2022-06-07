

系统构建
====

设置中最具挑战性的部分是编译所有依赖项和模块以兼容 a) 服务器端的虚幻引擎和 b) 客户端的 Python。

目标是能够从单独的 Python 进程调用虚幻引擎的函数。

![modules](/img/build_modules.jpg)

在 Linux 中，我们使用 clang-8.0 和 C++14 标准编译 CARLA 和所有依赖项。 然而，我们根据代码的使用位置来链接不同的运行时 C++ 库，因为所有要与虚幻引擎链接的代码都需要使用 libc++ 进行编译。

* * *

设置
--

命令

    make setup
    

获取和编译依赖项

llvm-8（libc++ 和 libc++abi）  
rpclib-2.2.1（两次，使用 libstdc++ 和 libc++）  
boost-1.72.0（libstdc++ 的头文件和 boost\_python）  
googletest-1.8.1（带有 libc++）

* * *

LibCarla
--------

使用 CMake 编译（最低版本需要 CMake 3.9）。

命令

    make LibCarla
    

两种配置:

 

服务器

客户端

**单元测试**

Yes

No

**需要条件**

rpclib, gtest, boost

rpclib, boost

**std runtime**

LLVM's `libc++`

Default `libstdc++`

**输出**

headers and test exes

`ibcarla_client.a`

**Required by**

Carla plugin

PythonAPI

* * *

CarlaUE4 和 Carla 插件
-------------------

两者都使用虚幻引擎构建工具在同一步骤编译。 它们需要 UE4\_ROOT 环境变量集。

命令

    make CarlaUE4Editor
    

启动虚幻引擎的编辑器运行

    make launch
    

* * *

PythonAPI
---------

### Versions 0.9.12+

使用 Python 的 setuptools（“setup.py”）编译。 目前需要在机器中安装以下内容：Python、libpython-dev 和 libboost-python-dev、pip>=20.3、wheel 和 auditwheel。

命令:

    make PythonAPI
    

创建两个文件，每个文件都包含客户端库并对应于系统上支持的 Python 版本。 一个文件是 .whl 文件，另一个是 .egg 文件。 这允许选择两种不同的、相互排斥的方式来使用客户端库。

> **A. .whl file**
> 
> > `.whl` 使用此命令安装:
> > 
> >      pip install <wheel_file>.whl
> >     
> > 
> > 无需像以前版本或 .egg 文件那样直接在脚本中导入库路径； 导入 carla 就足够了。
> 
> **B. .egg file**
> 
> > 参见下文

### Versions prior to 0.9.12

使用 Python 的 setuptools（“setup.py”）编译。 目前需要在机器中安装以下内容：Python、libpython-dev 和 libboost-python-dev。

命令

    make PythonAPI
    

会创建两个 "egg" 文件包

*   `PythonAPI/dist/carla-X.X.X-py2.7-linux-x86_64.egg`
*   `PythonAPI/dist/carla-X.X.X-py3.7-linux-x86_64.egg`

这个包可以通过添加到系统路径直接导入到 Python 脚本中。

    #!/usr/bin/env python
    
    import sys
    
    sys.path.append(
        'PythonAPI/dist/carla-X.X.X-py%d.%d-linux-x86_64.egg' % (sys.version_info.major,
                                                                 sys.version_info.minor))
    
    import carla
    
    # ...
    

也可以被 `easy_install`命令安装

    easy_install2 --user --no-deps PythonAPI/dist/carla-X.X.X-py2.7-linux-x86_64.egg
    easy_install3 --user --no-deps PythonAPI/dist/carla-X.X.X-py3.7-linux-x86_64.egg


