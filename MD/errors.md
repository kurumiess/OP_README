- ## 编译错误自查法

- # <img src="https://github.com/kurumiess/OP_README/blob/master/doc/er9.png" />

- # <img src="https://github.com/kurumiess/OP_README/blob/master/doc/er10.png" />

- 如果编译的时候出现编译出错的话，有好多都是可以自己查出来的，下面就来说说怎么查看日志找错误

- 出现编译错误，首先要看看是不是在【开编译固件】步骤出现错误，我现在只说是出现在【开编译固件】步骤的错误

- 【开编译固件】步骤出现错误的话，首先就去【下载软件包】的步骤查看你编译进固件的插件是否缺依赖，比如以下图片
- # <img src="https://github.com/kurumiess/OP_README/blob/master/doc/er2.png" />
---
- 如果以上没有发现缺依赖，或者说虽然有插件缺依赖，但是你又没选择这个插件，那就下载日志查看错误了，下载日志方法看下面图片
- # <img src="https://github.com/kurumiess/OP_README/blob/master/doc/er4.png" />
- 日志下载来后就查看日志了，日志都带有你编译的文件夹名字的，比如我这个编译的是[Lede_source],在日志的名字就是[编译 Lede_source.txt]
- 编译出错一般都是显示在差不多最下面的，你可以从下面往上面翻，仔细的查看日志的内容，其实有错误的地方都有特点的，就是会有[ * ]米字符号标记出来的
- 我来说说我一般是怎么搜索的，把查看日志时候拉到最下面，然后又往上拉一下，不要拉太上去，然后按键盘的 Ctrl+f 出现搜索框，然后输入关键字搜索
- 关键字一般是《error 1》或者《Collected errors》
- 搜索error 1如果出现附近有 make[5]: *** 字样的就在附近上下的一起看，make[5] 那个[5]是个数值每个错误都不一样，1、2、3、4、5、6、7、8之类的
- 搜索Collected errors的时候有这个关键词的话，一般就往下翻然后看 make[5]: *** 之类的字样
- 以上搜索都难找的话，还可以搜索《: *** 》，就是取 make[5]: ***  后面的 : *** 当关键词搜索
- 下面我拿2个编译错误为例：
---
- # <img src="https://github.com/kurumiess/OP_README/blob/master/doc/er5.png" />
- 上面这个图片的错误是找出来了，但是看不太明白，实在是我也不知道那里错了，如果是出现这个情况的话，你就把你编译的文件夹里面的.config配置文件，只保留机型的3项，然后再编译一次，如果还出现错误，那就是源码有问题，源码有问题就去找该源码的作者，在他的仓库提issues，我每个文件夹都有标注出源码出处的链接的，提issues的时候要把错误的详细也发过去，这样人家才知道你什么错误怎么解决
---
- 再来看个简单点的，以下图片为例
- # <img src="https://github.com/kurumiess/OP_README/blob/master/doc/er7.png" />
