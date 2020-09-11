Apple ld64

------

可编译的连接器 ld64-530

源码来自 Apple Open Source https://opensource.apple.com/tarballs/ld64/ld64-530.tar.gz



# 编译

ld64 依赖 [tapi](https://opensource.apple.com/release/developer-tools-1131.html，这个库就是用来解析 .tbd 文件的。 libtapi.dylib 随着 Xcode 一起发布了，放在 `/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/lib`，不过没有对应的头文件。我已经将 ld64 需要依赖的头文件全部放在了 external-include 目录下。



## external-include

* llvm 以及 llvm-c 是来自 [llvm-project](https://github.com/apple/llvm-project)，分支 stable/20190104，只所以使用这个分支是因为 ld64-530 对应的 [tapi-1100.0.11](https://opensource.apple.com/source/tapi/tapi-1100.0.11/) 在 readme 中提到了
* mach-o 来自 [dyld-750.5](https://opensource.apple.com/source/dyld/dyld-750.5/)，删除掉头文件中关于 bridgeos 的相关内容，这是什么系统我也拇指
* tapi 来自 [ tapi-1100.0.11](https://opensource.apple.com/source/tapi/tapi-1100.0.11/)



其他改动直接查看 git 日志即可



# 其他

编译出来的 ld 依赖 libtapi.dylib

```bash
> otool -L /path/to/Products/Debug/ld
	/usr/lib/libxar.1.dylib (compatibility version 1.0.0, current version 1.3.0)
	@rpath/libtapi.dylib (compatibility version 1.0.0, current version 1100.0.11)
	@rpath/libswiftDemangle.dylib (compatibility version 1.0.0, current version 1103.0.32)
	/usr/lib/libc++.1.dylib (compatibility version 1.0.0, current version 902.1.0)
	/usr/lib/libSystem.B.dylib (compatibility version 1.0.0, current version 1281.100.1)
```

ld64 工程会把 Xcode 自带的 libtapi.dylib 以及其他依赖的动态库连接到产出目录下面，如果你要把编译好的 ld 拷贝到其他目录执行，请把依赖的动态库也做相应的连接。

