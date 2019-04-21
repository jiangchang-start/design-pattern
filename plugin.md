# 插件模式
插件模式在日常生活见得比较多，比如IDEA，vscode，MyBatis Generator等等都有插件模式，但是并没有去仔细的去了解过，GOF中也没有将插件模式作为一种设计模式，那么插件模式究竟是什么呢？  
## 插件模式能干嘛？
* 能够让第三方开发者为程序提供扩展
* 能够轻松的为程序添加新特性
* 能够减轻程序体积
## 插件机制
1. 插件模式使用[duck typing](https://en.wikipedia.org/wiki/Duck_typing)，定义一系列的接口，这些接口中有的是强制实现，有的是可选实现，插件负责实现这些接口。
2. 在主程序启动过程中，主程序需要提供一个插件管理服务，用来管理插件。包括去特定目录检查插件存在与否，加载插件，验证等工作。

# 参考链接
* https://en.wikipedia.org/wiki/Plug-in_(computing)
* https://softwareengineering.stackexchange.com/questions/163597/plug-in-based-software-design