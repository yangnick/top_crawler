top_crawler
===========
首先实现功能，以前用过requests和beautifulsoup做简单的网管登陆脚本，所以爬虫也选择了这两个模块。

主要思路如下：

visited=set()
urls=queue()

for i in urls:
    if i is a item page:
        download the page
    else:
        a=find_all_href()
        for j in a:
            if j not in visited:
                queue.add(j)

只下载了url中包含item的，因为其他图片基本是这个item的缩略图，
并且在包含item的page下的链接不再保存，因为都可以由其他链接访问到

然后加入线程
Queue.queue已经是线程安全的了，不用管。
创建线程类和方法，run方法实现上述伪代码。

用bloom filter来优化visited。

加入命令行控制，以getopt实现。
对于-l选项，以前的停止策略是queue为空，
加入下载图片上限后，新加入lock和event用来使主进程退出，
下载的图片可能比-l指定的限制多，因为为了减少线程公用代码段，lock的代码很少。

还可以加入配置文件（未实现）


如果要复用这个程序，最好将每个模块独立出来，做成接口，
如判断一个url是否满足访问条件，url访问出错处理等。
