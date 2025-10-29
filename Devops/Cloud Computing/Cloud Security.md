
Encryption at rest 一般是说对数据库中的数据进行加密
Encryption in transit 一般是说 TLS/SSL 在数据传输的过程中加密
Encryption in use 一般说的是数据在内存中的时候就加密了, 且在加密的情况下一样进行计算. 

加密有分为 Client-side 和 Server - side
Client-side 一般说是数据在 client端就已经被加密, Server端没有密钥, 也没法解密
Server-side 一般说是数据在传递到 server 端的时候才加密. Occurs after cloud storage receives your data but before the data is written to disk and stored. 由 server 端的key management service工具来管理密钥

BYOK Bring your own keys
KYOK Keep your own keys


SSL Secure Sockets Layer 
TLS Transport Layer Security