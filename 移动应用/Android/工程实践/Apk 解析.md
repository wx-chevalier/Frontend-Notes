# Apk 解析

系统为每个 APK 创建进程时，都会通过 PathClassLoader 类进行加载，同时开发者也可以通过 DexClassLoader 动态加载额外的 Dex 文件，有点类似于 dlopen 和 dlsym 函数的作用。PathClassLoader 和 DexClassLoader 两者都承继自 BaseDexClassLoader，最终都是通过 DexFile 完成对 dex 的加载。一般情况下每个 ClassLoader 对应一个 DexFile，但其本身是可以包含多个 DexFile 的，当要加载一个 Class 时，会遍历各个 DexFile。

类 DexFile 底层是通过 JNI 方式实现的，针对 APK 文件(包括 jar 和 zip)和二进制字节流，系统分别提供了 dvmDexFileOpenFromFd 和 dvmDexFileOpenPartial 两个函数进行处理。这两个流程最终目的都是构造出 DexOrJar 结构体，并通过 JNI 接口把该结构体的地址保存到 DexFile 的私有成员变量 mCookie。DexOrJar 结构主要由 RawDexFile 和 JarFile 组成。
