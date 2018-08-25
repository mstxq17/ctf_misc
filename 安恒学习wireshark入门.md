1.流量题 

这次安恒的题 很明显就是朔源的题目 所以实战来说肯定是朔源的角度 	(ps:现在的的人学ctf,求速成求刷题,却忘记了ctf是提高自己技术的一个目的)

第一步总体入手查看 

通过协议分级 得知webone http协议占了99% 那么重点关注对话的80端口

![image-20180825221301305](https://ws4.sinaimg.cn/large/006tNbRwgy1fumcf89e8pj30k40gewkm.jpg)



![image-20180825221749981](https://ws4.sinaimg.cn/large/006tNbRwgy1fumcfakjzoj31fk0nmwnn.jpg)



这里很明显看到了tcp分组有61728个 其中在80端口发现  ip:192.168.94.59->192.168.21.189 的80端口发送了大量的tcp连接 (tcp 握手客户端自己开端口 所以port A 不一样 )

这里很明显就可以得知了在这个ip不正常啊 正常不会那么频繁访问  那么这个时候基本可以确定是黑客地址(ps:我一般再去看看http的内容 然后发现一些恶意请求payload 个人喜好)



这里由于总共有70w数据包在wireshark处理很麻烦 所以坐下数据清洗

导出分组为新的数据包:

![image-20180825221226417](https://ws1.sinaimg.cn/large/006tNbRwgy1fumcg70v5hj31kw17g4qp.jpg)![image-20180825222305300](https://ws4.sinaimg.cn/large/006tNbRwgy1fumcg2xt6mj310w0qyqbc.jpg)



然后打开这个数据包开始分析:

![image-20180825222429451](https://ws4.sinaimg.cn/large/006tNbRwgy1fumcfyfes7j31kw0d348m.jpg)

按序号慢慢拖下去:

![image-20180825222456577](https://ws4.sinaimg.cn/large/006tNbRwgy1fumcfvp4o2j31kw073tdg.jpg)

再序号23 发现了扫描器痕迹

随便看下下面的数据包:

awvs特征参考:https://blog.csdn.net/SKI_12/article/details/78705998

![image-20180825222648428](https://ws4.sinaimg.cn/large/006tNbRwgy1fumcfs64zkj312y0auad8.jpg)

所以说这些请求都是awvs的爬虫请求

简单看看题目要求是啥:

![image-20180825222934646](https://ws1.sinaimg.cn/large/006tNbRwgy1fumcfqb1vsj30yg0lk7dc.jpg)

(1)第一题秒解

(2)排除下awvs的部分扫描

!(http.request.line == "Acunetix-Aspect-Queries: filelist;aspectalerts\x0d\x0a") &&http.request.method=="POST"

看下post内容 点下no 排个序:

![image-20180825223651118](https://ws2.sinaimg.cn/large/006tNbRwgy1fumcfn3yxqj31kw08wqbq.jpg)

(3)这个考虑导出的时候 把ip.src=192.168.32.189 也导出来

然后用 http.response.code==302 

然后跟踪下tcp流 

![image-20180825224919188](https://ws2.sinaimg.cn/large/006tNbRwgy1fumcfjch8kj30tk0n0wnn.jpg)

(4)后面的题目不想写了 后面就是朔源的耐心了 

有时候如何节省时间去思考提高自己的技术对于我来说真的该好好学习

(P神:不求甚解 难当优秀)

学了两天的wireshark 在印象笔记随便记录了下 感兴趣可以看看(ps:自己毕竟菜b)

 https://github.com/mstxq17/ctf_misc 

 