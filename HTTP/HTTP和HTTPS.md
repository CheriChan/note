# HTTP和HTTPS

[TOC]



> 1.https 是在 http 协议的基础上加了个 SSL；
> 2.主要包括：握手(凭证交换和验证)和记录协议(数据进行加密)。

## HTTP和HTTPS有何区别

+ HTTP默认80端口，HTTPS默认443端口
+ HTTPS需要申请证书
+ HTTP是超文本传输协议，是明文传输；HTTPS是经过SSL加密的协议，传输更安全
+ HTTPS比HTTP慢，因为HTTPS除了TCP握手的三个包，还要加上SSL握手的九个包