# [为什么都说首屏html大小限制在14KB以内](https://github.com/879479119/879479119.github.io/issues/7)

**为什么首屏的html资源要限制在14KB以内呢？**

1. 最普遍的说法（各种博客）：

   反正是14kb可以最好的利用网络带宽，可以让html在一次就传输完所以最快咯~

2. 详细的说法（谷歌开发者文档）：

   https://developers.google.com/speed/docs/insights/mobile


>鉴于 TCP 评估连接状况的方式（即 [TCP 慢启动](http://en.wikipedia.org/wiki/Slow-start)），新的 TCP 连接无法立即使用客户端和服务器之间的全部有效带宽。因此，在通过新连接进行首次往返的过程中，服务器最多只能发送 10 个 TCP 数据包（约 14KB），然后必须等待客户端确认已收到这些数据，才能增大拥塞窗口并继续发送更多数据.
>
>另外还需注意的是，10 个数据包 ([IW10](http://tools.ietf.org/html/draft-hkchu-tcpm-initcwnd-01)) 这一限值源自 TCP 标准的最近一次更新：您应确保自己的服务器已升级到最新版本，以便能够充分利用这次更新。否则，这一限值可能会降低到 3-4 个数据包！
>
>考虑到 TCP 的这种行为，请务必优化您的内容，以尽可能减少为传输必要数据（以完成网页的首次呈现）而需进行的网络往返的次数。理想情况下，ATF 内容应小于 98KB，这样浏览器才能在 3 次网络往返之后即可显示网页内容，以便为服务器响应延迟和客户端呈现留出充足的时间预算。

看到上面的一段我们其实已经大概明白了，原来是因为慢启动的存在导致我们第一屏只能发送14kb的内容，那为什么是1.4kb * 10这么计算呢？在旧版本的服务器上又是指的哪个版本之前呢？这里我们分两块说

## 什么是TCP慢启动

TCP网络通信占了我们日常使用的网络通信的绝大部分，网络环境质量变化都十分复杂，同时为了避免流量攻击等行为，TCP实际上有一些防止拥塞的策略，主要是依靠四个算法来实现：**1）慢启动**，**2）拥塞避免**，**3）拥塞发生**，**4）快速恢复**

> 慢启动,主要目的就是为了避免新加入的连接直接把带宽打满，也可以避免一些恶意行为。

计算机网络的概念：

> MTU: Maximum Transmission Unit，是指一种通信协议的某一层上面所能通过的最大数据包大小（以字节为单位）。最大传输单元这个参数通常与通信接口有关（网络接口卡、串口等）。

> MSS: Maximum Segment Size，是传输控制协议(TCP)的一个参数，以字节数定义一个计算机或通信设备所能接受的分段的最大数据量。 它并不会计算 TCP 或 IP 协议头的大小。

从算法上来说原理（不论系统实现），慢启动的算法如下(cwnd全称Congestion Window)：

1）连接建好的开始先初始化cwnd = 1，表明可以传一个MSS大小的数据。

2）每当收到一个ACK，cwnd++; 呈线性上升

3）每当过了一个RTT，cwnd = cwnd*2; 呈指数让升

4）还有一个ssthresh（slow start threshold），是一个上限，当cwnd >= ssthresh时，就会进入“拥塞避免算法”（去参考文章里面看）

所以，我们可以看到，如果网速很快的话，ACK也会返回得快，RTT也会短，那么，这个慢启动就一点也不慢。但是我们首屏注重的就是第一个横线，在这么看来只有1个MSS肯定是不够使的

实际上我们工作中就不是从1开始慢慢增长了，需要提一下的是一篇Google的论文《[An Argument for Increasing TCP’s Initial Congestion Window](http://static.googleusercontent.com/media/research.google.com/zh-CN//pubs/archive/36640.pdf)》Linux 3.0后采用了这篇论文的建议——把cwnd 初始化成了 10个MSS。

而Linux 3.0以前，比如2.6，Linux采用了[RFC3390](http://www.rfc-editor.org/rfc/rfc3390.txt)，cwnd是跟MSS的值来变的，如果MSS< 1095，则cwnd = 4；如果MSS>2190，则cwnd=2；其它情况下，则是3。

## MSS大小的计算

我们已经知道了1.4K * 10之中的10是从哪里来的了，10就是指的10个MSS，那1.4k就是这个MSS的最大大小了吧。熟悉计算机网络的同学肯定已经知道是怎么回事了

因为我们一般认为终端用户使用的网络MTU是1500Byte来的，由于那相当于是我们数据链路层能够传输的大小了，我们实际能传输的IP数据包内容肯定是比他少的，简单的算一算

一个TCP包(数据段)的荷载 <= MSS < MTU

> PPPoE首部6，PPP协议2
> 数据链路层最大data为1500-8=1492
> IPv4首部最少20，IPv6首部40，TCP首部最少20
> MSS最大为1492-20-20=1452

现在得到了MSS最大是1.452K这个结果，正是与我们预期一致的



[TCP的那些事儿（下） 陈皓]https://coolshell.cn/articles/11609.html

[goolge 移动网络分析]https://developers.google.com/speed/docs/insights/mobile

[TCP/IP数据包结构详解 [水沐清華](https://me.csdn.net/prsniper)]https://blog.csdn.net/prsniper/article/details/6762145

[TCP一次数据包最大负载是多少？[**wind5o**](https://segmentfault.com/u/win5do)]https://segmentfault.com/a/1190000012962389%

