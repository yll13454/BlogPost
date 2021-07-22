使用HTTP/1.1协议的curl，发送一个请求，在post数据量超过1K的时候，接口会返回:

HTTP/1.1 100 Continue

HTTP/1.1 200 OK
Date: Sat, 07 Dec 2013 10:09:11 GMT
Server: Apache/2.2.24 (Unix) PHP/5.3.25
X-Powered-By: PHP/5.3.25
Content-Length: 43
Content-Type: text/html

从表面上看，100 Continue表明请求一次没发送完，需要继续发送；搜索后看到现在新浪微博PHP架构师的一片博文，介绍了100的信息；详见在使用curl做POST的时候, 当要POST的数据大于1024字节的时候, curl并不会直接就发起POST请求, 而是会分为俩步,；

流程如下：
\1. 发送一个请求, 包含一个Expect:100-continue, 询问Server使用愿意接受数据
\2. 接收到Server返回的100-continue应答以后, 才把数据POST给Server





使用100（不中断，继续）状态码的目的是为了在客户端发出请求体之前，让服务器根据客户端发出的请求信息（根据请求的头信息）来决定是否愿意接受来自客户端的包含了请求内容的请求；在某些情况下，在有些情况下，如果服务器拒绝查看消息主体，这时客户端发送消息主体是不合适的或会降低效率

对HTTP/1.1客户端的要求：
-如果客户端在发送请求体之前，想等待服务器返回100状态码，那么客户端必须要发送一个Expect请求头信息，即：”100-continue”请求头信息；

-如果一个客户端不打算发送请求体的时候，一定不要使用“100-continue”发送Expect的请求头信息；





简单而言，100 Continue就是告诉客户端，请求的开始部分已经被接受，而且没有被拒绝，请客户端继续发送余下的请求；如果请求完成，忽略响应；在请求结束后，服务器必须要发送一个最终响应；
当然解决的方法也很简单，就是强制让客户端不要发送Expect，
curl_setopt($ch, CURLOPT_HTTPHEADER, array(‘Expect:’)); //这句话的意思是在请求的头信息中，强制清空Expect，这样服务器也就不会返回100 Continue，这样在curl中会增加请求延时的概率；
// Disable Expect: header (lighttpd does not support it)

经过测试，增加这一条头信息，的确是搞定了100响应头信息的问题；
为了进一步测试，我们使用了tcpdump所有的头信息：
tcpdump -ieth1 -s0 host 10.12.194.23 and port 80 -w/data/x.pcap
然后使用Wireshark查看相应的pcap文件，对比前后两处的请求，的确很明显的出现了100信息；

[HTTP1.1请求返回状态码为100 Continue的分析说明和解决方法](http://www.webdoes.com/blog/archives/349)



