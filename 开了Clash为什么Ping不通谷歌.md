# clash

clash使用的socks5协议，而socks5协议在osi七层模型中属于第五层会话层。

SOCKS是一种[网络传输协议]，主要用于客户端与外网服务器之间通讯的中间传递。当防火墙后的客户端要访问外部的服务器时，就跟SOCKS代理服务器连接。这个代理服务器控制客户端访问外网的资格，允许的话，就将客户端的请求发往外部的服务器。                    --维基百科

# ping

ping属于ICMP(internet control message protocol)协议，在OSI七层模型中的第三层网络层。

而上一层协议的代理，对下层没有任何作用。也就是说，socks5的代理对ping不起作用，ping到的依然是国内网络，所以ping不通。