[Demo git hub路径](https://github.com/spursy/C-Arithmetic)
>在VSCode 官网上看到VSCode 神一样的支持很多[language](https://marketplace.visualstudio.com/VSCode)，以下将记录在使用OSX版本的VSCode 配置编辑与编译C++环境所踩过的深坑。

### "clang++"一款OSX系统上的C++ 编译器
>Clang 是一个 C++ 编写、基于 [LLVM](http://www.oschina.net/p/llvm)、发布于 LLVM BSD 许可证下的 C/C++/Objective C/Objective C++ 编译器，其目标（之一）就是超越 [GCC](http://www.oschina.net/p/gcc)。

clang 的出现绝不是一次事故,[Apple 使用 LLVM 在不支持全部 OpenGL 特性的 GPU (Intel 低端显卡) 上生成代码 (JIT)，令程序仍然能够正常运行。之后 LLVM 与 GCC 的集成过程引发了一些不快，GCC 系统庞大而笨重，而 Apple 大量使用的 Objective-C 在 GCC 中优先级很低。此外 GCC 作为一个纯粹的编译系统，与 IDE 配合很差。加之许可证方面的要求，Apple 无法使用修改版的 GCC 而闭源。于是 Apple 决定从零开始写 C family 的前端，也就是基于 LLVM 的 Clang 了](http://www.oschina.net/p/clang)。
备注：相关[Clang WIKI 信息](https://en.wikipedia.org/wiki/Clang)。
### 安装VSCode 的C++ extention - [cpptools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)

![安装cpptools](http://upload-images.jianshu.io/upload_images/704770-75bb0cae12f3b017.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### 配置task.json 文件
> 使用快捷键command + shift + B 组合，VSCode 会出现提示。

![生成task.json 文件](http://upload-images.jianshu.io/upload_images/704770-db806b962ce5377a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
点击"配置生成任务"， 出现task.json 类型的dropdown list。

![task.json dropdown list](http://upload-images.jianshu.io/upload_images/704770-68b39941896e90fc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
选择Others， 系统生成task.json, 然后修改文件如下：[task.json 文件格式](https://go.microsoft.com/fwlink/?LinkId=733558）
```
{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "0.1.0",
    "command": "clang++",
    "isShellCommand": true,
    "args": [
        "./SyntaxDemo/demo.cpp", // 入口文件
        "-g", // 是否生成调试信息
        "-O0", // 提示编译速度
        "-m32" // 64bit下
    ],
    "showOutput": "always"
}
```
### 配置setting.json文件

![生成setting.json 文件](http://upload-images.jianshu.io/upload_images/704770-fb77baedf970ef7a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
单击iostream，选择转义定义，系统自动生成setting.json。
```
{
    "files.associations": {
        "iostream": "cpp"
    }
}```
###  生成可执行文件
> 在配置完task.json 后操作快捷键 Command + Shift + B 生成可执行的文件（VSCode 默认生成a.out）。

![生成a.out 文件](http://upload-images.jianshu.io/upload_images/704770-d98cb9e5d1d93f68.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> **OSX 同样可以通过命令行工具直接生成C++ 可执行的文件**
```
clang++ -o 《out file》《C++ file》
```

###  执行a.out 文件

> 通过命令行工具，进入a.out 所在的文件夹路劲下，然后执行 **./a.out** 

### 配置launch.json 文件配置VSCode debugger 环境
> press F5 快捷键，选择配置环境**C++ (GDB/LLDB)**

![配置launch.json 文件](http://upload-images.jianshu.io/upload_images/704770-c6be4f6db3afbed7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "(lldb) Launch",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceRoot}/a.out",  // 指向生成的C++ 可执行文件
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceRoot}",
            "environment": [],
            "externalConsole": true,
            "MIMode": "lldb"
        }
    ]
}
```