# 使用GraalVM Native Image 编译Java



GraalVM 是一种高性能 JDK，旨在加速 Java 应用程序性能，同时消耗更少的资源。 GraalVM 提供两种运行 Java 应用程序的方法：在 HotSpot JVM 上使用 Graal JIT编译器或作为AOT)编译的本机可执行文件。除了 Java，它还为 JavaScript、Ruby、Python 和许多其他流行语言提供运行时。 GraalVM 的多语言能力使得在单个应用程序中混合编程语言成为可能，同时消除了任何外语调用成本。



## 准备环境

### 下载GraalVM

GraalVM有两个实现，一个是[Oracle GraalVM](https://www.graalvm.org/)，另一个是基于社区版的[Oracle GraalVM](https://www.graalvm.org/)开发的[Liberica NIK](https://bell-sw.com/liberica-native-image-kit/)。

本文主要使用[Liberica NIK](https://bell-sw.com/liberica-native-image-kit/)。

[进入下载页面](https://bell-sw.com/pages/downloads/native-image-kit/#/nik-22-17)，下载最新版本的包。

此处下载NIK 22 ( JDK 17 )版本， 解压后如下

![image-20221201223349736](https://s2.loli.net/2023/04/01/vUhoMaFywEJZxYN.png)

### 配置GraalVM

需要将GraalVM配置到环境变量中

1. 首先创建GRAALVM_HOME

   ![image-20221201222848734](https://s2.loli.net/2023/04/01/TvXtYfOGyVz4Lu7.png)

2. 创建JAVA_HOME

   ![image-20221201223102651](https://s2.loli.net/2023/04/01/BqIZ3p4zdtnwPue.png)

3. 添加Path

   ![image-20221201222928832](https://s2.loli.net/2023/04/01/IGPeCjF8zgwNxYH.png)

4. 验证配置正确

   ![image-20221201223545546](https://s2.loli.net/2023/04/01/tJVYmSnBrLlbows.png)



### 下载Visual Studio 

使用[Visual Studio Installer](https://visualstudio.microsoft.com/zh-hans/vs/) 安装编译环境， 勾上MSVC v143， Windows 10 SDK等， [可参考Here](https://medium.com/graalvm/using-graalvm-and-native-image-on-windows-10-9954dc071311)

[Visual Studio command line doc](https://learn.microsoft.com/en-us/cpp/build/building-on-the-command-line?view=msvc-170&viewFallbackFrom=vs-2019#developer_command_prompt_shortcuts)

![image-20221201223919628](https://s2.loli.net/2023/04/01/pWE5kRarf6iDjvh.png)

![image-20221201224034174](https://s2.loli.net/2023/04/01/Ep5aBr8tJdeh4lm.png)

打开**开始菜单的Visual Studio 2022文件夹**下的Developer Command Prompt for VS 2022，输入 `cl`出现如下结果，则安装Visual Studio成功。

![image-20221201225137306](https://s2.loli.net/2023/04/01/VwC6hFkDZ5WBMPc.png)



**不过另开一个cmd窗口后， 又会提示cl命令不存在，则需要在环境中在另行配置Visual Studio环境**

![image-20221201225743394](https://s2.loli.net/2023/04/01/yGzQ4nukcPTXYgO.png)



### 配置Visual Studio 编译环境

1. 创建INCLUDE

   ![image-20221201230303291](https://s2.loli.net/2023/04/01/crUstGBpM5Wb1Vd.png)

2. 添加LIB

   ![image-20221201230120502](https://s2.loli.net/2023/04/01/v5IanUMEqTdwxDb.png)

3. Path添加bin

   ![image-20221201230138207](https://s2.loli.net/2023/04/01/nT5bguoqkWxLPNe.png)

4. 验证

   ![image-20221201230425105](https://s2.loli.net/2023/04/01/uiDK4FSAULTeNWy.png)



## 编译一个Class

先写一个简单的hello world

```java
public class App {
    public static void main(String...args) {
        System.out.println("hello world");
    }
}
```

1. 使用javac 编译java文件
2. 使用native-image.cmd 编译App, 生成二进制文件
3. 运行exe二进制文件, 输出hello world

![image-20221201231457716](https://s2.loli.net/2023/04/01/VkIayOJs18vUSgb.png)