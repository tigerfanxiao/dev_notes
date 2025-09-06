### Cryptograph

### 对称密码系统
加密和解密数据都用相同的秘钥, 有中间人共计
### 非对称密码系统
加密和解密使用不同的秘钥. 区分公钥和私钥. 
- 公钥: 可以在公网上传输的秘钥, 一般用于加密
- 私钥: 只能在本地保存, 不能在公网上传输的秘钥. 一般用于解密
- 使用公钥加密的信息, 只有用自己的私钥才能解密
#### 加密通信
使用公钥加密, 使用私钥解密. 
当代通信协议是非对称密码系统和对称密码系统的结合. 因为非对称秘钥系统的加密速度慢, 资源消耗大, 但是安全性高, 不怕中间人攻击. 所以用来传输加密秘钥. 而对称加密系统的效率高, 但是害怕中间人攻击, 所以作为对海量数据的加密工具. 

#### 数字签名
使用私钥加密, 使用公钥解密. 只有签名的用户A的私钥加密的文件. 所有收到A 的公钥的人都能用公钥正常解密文件

# TLS
TLS 的角色: client, server, CA

TLS 的应用场景
- HTTPS 的协议=HTTP + TLS: 通信的双方是浏览器 browser 作为 client, CA 机构作为证书服务器
钓鱼网站场景
1. 黑客伪造了一个和银行一样的网站. 用户作为浏览器无法判断打开的网页是黑客伪装的, 还是真正的网页. 
2. 黑客伪造的网站, 也可以给客户发送公钥, 然后用户用接受到的公钥, 加密自己的对称加密算法的密码, 发给黑客伪造的网站. 造成密码泄露. 那么如果防止这个问题呢

3. **Server sends its certificate to the browser**
    - The certificate contains:
        - Domain name (example.com)
        - Public key
        - Issuer (CA)
        - Validity period
        - CA’s digital signature
4. Why a fake website cannot easily get a valid certificate**
- A fake website like https://examp1e.com cannot get a valid certificate for example.com from a **trusted CA**.
- Trusted CAs will only issue certificates to the **domain owner** after proper validation (DNS or file-based proof).
- If an attacker tries to issue their own certificate, the browser will **not trust it**, because it’s either self-signed or signed by an untrusted CA.
- 软件验证: windows 会通过 TLS 的方式验证安装的软件是否是可靠的. 此时 CA 为微软的服务器
- 每个操作系统都维护了一个 trusted CA 的列表
- 但是当我们访问一些内部网站, 设备管理页面. 我们拿到的证书不是操作系统上 trusted CA list 上时, 浏览器就会提示风险 
### CA Certificate Authority
专门分发公钥证书的机构, 验证某个网站是否是安全可靠的. 为了防止用户从黑客那边获得伪造的证书 
- CA 机构分发证书. 证书本身使用了数字签名的技术, 其中包含公钥密码, 网站的名字和子域名
在通信过程中, CA 也可以向 Client(browser) 索要证书. 这个操作一般在浏览器后台完成 

```json
            "Action": [
                "aws-portal:ViewAccount",
                "aws-portal:ViewBilling",
                "aws-portal:ViewUsage"
```