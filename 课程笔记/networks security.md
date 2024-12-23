## 重点复习

DES is comprised of three phases:

initial permutaion

16 rounds of processing

final permutation



DES uses two important components:

a key generator

an expansion function



the key generator generates an inital 64-bit key and 8-bits are removed from the inital key

then the  56-bit key is divided into two 28-bits keys. Bit shifting is performed on the left and right 28-bit keys to ensure that an unique subkey is generated during each processing round.

the expansion function receives a 48-bit subkey and XORs it with a plaintext block to produce a 48-bit block

then this 48-bit block is divided into 8 6-bit blocks and each 6-bit block is compressed into a 4-bit block using a substitution method

finally all the 8 4-bit blocks are concatenated into a 32-bit block





the following subkey is generated in round 3, 1101010

what is the subkey for round 4 (bit shifting)



given B1 = 110110 find C1



diffi helman exchange process



RSA

given p=3 q=5, calculate the public and private key pairs

s1: n = p x q = 3 x 5 = 15

s2:  $\phi(n) = (p-1) \times (q-1) = 8$

s3:  choose public key e such that e is relatively prime to $\phi(n)$ and less than $\phi(n)$

s4: finding d such that de = 1 mod $\phi(n)$  and d less than 160

$\frac{1 + \phi(n) k}{e}$



加密与解密： $c = m^e mod \ \ n,   m = c^d mod \ \ n$





security management, firewall,  malware





## 复习课

### part a 30 3 题 (都是DES的内容，会提到DEA), 

DEA 的几个主要处理阶段 (第一题)

3个阶段：

- initial permutation （对明文重新打乱）
- 16 operations (rounds) （permutation 和 substitution， 随机子密钥生成）
- Final permutation（initial permutation的逆运算）

第二题：

密钥生成的方式：随机数，位移的方式

用哪一种比较合适，为什么？

循环左移比较合适

解释：每一轮都需要产生不同的子密钥，但是使用随机数的时候，会有可能产生相同的子密钥，所以不是一个理想的方案。而位移的方式，可以确定不会产生一样的子密钥

第三题：f function

f 要有一个很好的雪崩效应，avalanche，

如果任意的改变一位，可以导致输出有翻天复地的变化

就是由permutation和substitution来实现雪崩效应 

### part b 40 3 题 （场景题）, 

作为密码安全的顾问，你的客户会让你做风险评估 （第一题）

医疗机构，对屏幕进行了敲诈勒索（补丁，更新，安全评估，审查）

五个方面提出改进的意见：

先列出五个方面，然后再详细解释

技术控制，员工安全意识培训，灾难恢复，监视员工使用的行为，保险的审核，审查

对邮件的过滤，防火墙的更新

可疑的邮件，钓鱼网站的识别

建立完备的备份，以及恢复的计划

监视员工在网络中使用的行为

审查保险是否有包括到数据泄露和灾难恢复的响应



识别用户的身份，   （第二题）

比较用户输入的密码，明文存放，密文存放，哈希存放

ppt中有



防火墙      （第三题）

packet filter 是防火墙最重要的一环

stateful firewall还是stateless firework

为什么会，为什么不会？





### part c 30 2题

第一部分

第一题

malware 恶意程序 malicious software

识别病毒或者恶意软件的签名，病毒的特征码

signature-based， a form of blacklisting

anomaly-based, look for unusual behaviour

这两种方法的优缺点是什么，正好是相反的

第二题

攻击者是怎么样把病毒嵌入可执行程序中去的？

有冗余部分，可以嵌入代码洞，可以插入多个代码洞，会导致代码长度的增加



第二部分

密码学

第一题

密钥的分发，rsa（慢） and DH （快）

根据一个场景，选择这两个，

题目可能是，分发的时间很短，建议是使用DH

rsa 当密钥长度变小的时候，安全性会降低



第二题：

rsa的计算







