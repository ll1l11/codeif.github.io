---
layout: post
title: rsa反解简单分析
tags: [python]
---


可以再维基百科看 [RSA加密演算法](http://zh.wikipedia.org/wiki/RSA%E5%8A%A0%E5%AF%86%E6%BC%94%E7%AE%97%E6%B3%95) 相关介绍

这里结合python的rsa库来说明, 先随机生成一个简单的pub\_key, private\_key
为了例子看起来简单, 我们用生成一个最短位数的key pair, 也就是16位的

    pub_key, priv_key = rsa.newkeys(16)
    print pub_key, priv_key

输出:

    PublicKey(35237, 65537) PrivateKey(35237, 65537, 27893, 211, 167)

将各个数值保存下来:

    n, e, d, p, q = 35237, 65537, 27893, 211, 167

根据rsa算法, 这几个变量的关系是, 先找到两个大的质数p, q, p!=q, 也就是上面的 211, 167, 他们的乘积作为n, 也就是 35237, 根据欧拉函数得到 r = (p-1) * (q-1), 得到r是: 34860, 然后选择一个小于r的正式e, 也就是这里的65537

这里我们求得e关于r的[模反元素](http://zh.wikipedia.org/wiki/%E6%A8%A1%E5%8F%8D%E5%85%83%E7%B4%A0), 求d的函数可以在 [扩展欧几里得算法](http://zh.wikipedia.org/wiki/%E6%89%A9%E5%B1%95%E6%AC%A7%E5%87%A0%E9%87%8C%E5%BE%97%E7%AE%97%E6%B3%95) 找到

可以用下面的函数找到模拟元素 d

    def egcd(a, b):
        """
        可以用扩展欧几里得算法求模逆元素。
        设exgcd扩展欧几里得算法的函数，它接受两个整数a,b，输出三个整数g,x,y。g,x,y满足等式ax+by=g，且g是a,b的最大公约数。
        现在，求a关于模b的模逆元素（a<b）。egcd(a,b)={g,x,y}。若g=1，则该模逆元素存在，为b+x；若g不等于1，则该模逆元素不存在。
        """
        if a == 0:
            return b, 0, 1
        else:
            g, y, x = egcd(b % a, a)
            return g, x - (b // a) * y, y
    
    
    def modinv(a, b):
        """
        :return: 求模拟元素
        """
        g, x, y = egcd(a, b)
        if g != 1:
            raise Exception('modular inverse does not exist')
        else:
            # return x % b
            return b + x
    
将e, r带入modinv, 返回 d = 27893

**整个过程就是找到两个大的不同的质数, p,q求的 n=pq, r=(p-1)(q-1), 随机一个小于r的数e, 然后求的e关于r的模拟元素d**

为了简化加密解密, 我们将文本转化为数字, 数字转化为文本的过程省略了, 比如这里我们以加密整数 123 为例, 并解密

ps: 加密解密我们可以使用数学计算求解:

    x ** y % z

但是这个公式计算量太大, 加密长度太长时上面的方法并不合适 网上可以搜到如下的方法:

    def modexp(g, u, p):
        """
        :return: g ** u % p
        """
        s = 1
        while u:
            if u & 1:
                s = (s * g) % p
            u >>= 1
            g = (g * g) % p
        return s

在python中我们直接用下面的函数
    
    pow(x, y, z)

对一个明文加密为密文, 加密方只有公钥, 也就是n和e:

    plain = 123
    c1 = plain ** e % n
    c2 = modexp(plain, e, n)
    c3 = pow(palin, e, n)
    print c1, c2, c3

得到c是一样的,33185

然后我们将 33185 解密:

    c = 33185
    print c ** d % n
    print modexp(c, d, n)
    print pow(c, d, n)

三种方法都会输出123


上面说的是rsa的加密解密过程, 现在再说一下破解的过程, 也就是现在我们能拿到公钥 n, e, 加密后的密文c, 现在我们解出明文来, 

    n, e = 35237, 65537
    c = 33185

首先求得生成n的两个指数p, q, 这里我们用到一个开源的工具 msieve, 如下:

    > ./msieve 35237
    > cat msieve.log
    Wed Aug 13 10:39:45 2014
    Wed Aug 13 10:39:45 2014
    Wed Aug 13 10:39:45 2014  Msieve v. 1.51 (SVN Unversioned directory)
    Wed Aug 13 10:39:45 2014  random seeds: 8588fa0c ebe4f6a2
    Wed Aug 13 10:39:45 2014  factoring 35237 (5 digits)
    Wed Aug 13 10:39:45 2014  p3 factor: 167
    Wed Aug 13 10:39:45 2014  p3 factor: 211
    Wed Aug 13 10:39:45 2014  elapsed time 00:00:00

得到p, q = 167, 211, 然后根据提到的求解 d的方法,计算出d, 就可以解密成明文










