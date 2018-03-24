+++
topics = ["java"]
tags = ["java","rsa"]
draft = false
date = "2018-03-22T20:40:46+08:00"
title = "java Rsa pkcs#1 加解密及签名验签"
+++

最近对接api接口，需要和对方进行rsa验签，但是对方rsa密钥格式是RSA_PKCS1_PADDING格式，java中一般都是用pkcs#8，本文对密钥格式转换，加密过程进行记录，以供大家借鉴。转载请注明出处：*[defned.com/post/java-rsa-pkcs1/](http://defned.com/post/java-rsa-pkcs1)*
<!--more-->
### 公钥
RSA 密钥体系中对外公开的部分，通常用于数据加密、验证数字签名。

### 私钥
RSA 密钥体系中非公开的部分，通常用于数据解密、数据签名。

### 签名
就是只有信息的发送者才能产生的，别人无法伪造的一段数字串，它同时也是对发送者发送的信息的真实性的一个证明。

### java生成pkcs1密钥对
```java
KeyPairGenerator rsa1 = KeyPairGenerator.getInstance("RSA", new BouncyCastleProvider());
rsa1.initialize(1024, new SecureRandom());
KeyPair keyPair = rsa1.generateKeyPair();

PublicKey pub = keyPair.getPublic(); // pkcs8公钥
byte[] pubBytes = pub.getEncoded();
SubjectPublicKeyInfo spkInfo = SubjectPublicKeyInfo.getInstance(pubBytes);
ASN1Primitive primitive = spkInfo.parsePublicKey();
byte[] publicKeyPKCS1 = primitive.getEncoded(); // pkcs1公钥

PrivateKey priv = keyPair.getPrivate(); // pkcs8私钥
byte[] privBytes = priv.getEncoded();
PrivateKeyInfo pkInfo = PrivateKeyInfo.getInstance(privBytes);
ASN1Encodable encodable = pkInfo.parsePrivateKey();
ASN1Primitive primitive = encodable.toASN1Primitive();
byte[] privateKeyPKCS1 = primitive.getEncoded(); // pkcs1私钥
```

### pkcs1公钥加密
```java
public static byte[] Encrypt(String data, org.bouncycastle.asn1.pkcs.RSAPublicKey rsaPublicKey) throws Exception {
        Security.addProvider(new org.bouncycastle.jce.provider.BouncyCastleProvider());
        Cipher cipher = Cipher.getInstance("RSA/None/PKCS1Padding", "BC");
        Key publicKey = new java.security.interfaces.RSAPublicKey() {
            @Override
            public BigInteger getPublicExponent() {
                return rsaPublicKey.getPublicExponent();
            }

            @Override
            public String getAlgorithm() {
                return "RSA";
            }

            @Override
            public String getFormat() {
                return null;
            }

            @Override
            public byte[] getEncoded() {
                return new byte[0];
            }

            @Override
            public BigInteger getModulus() {
                return rsaPublicKey.getModulus();
            }
        };
        cipher.init(Cipher.ENCRYPT_MODE, publicKey);
        // 对数据分段
        byte[] b = data.getBytes();
        int inputLen = b.length;
        ByteArrayOutputStream out = new ByteArrayOutputStream();
        int offSet = 0;
        byte[] cache;
        int i = 0;
        while (inputLen - offSet > 0) {
            if (inputLen - offSet > MAX_ENCRYPT_BLOCK) {
                cache = cipher.doFinal(b, offSet, MAX_ENCRYPT_BLOCK);
            } else {
                cache = cipher.doFinal(b, offSet, inputLen - offSet);
            }
            out.write(cache, 0, cache.length);
            i++;
            offSet = i * MAX_ENCRYPT_BLOCK;
        }
        return out.toByteArray();
    }
```

### pkcs1公钥解密
```java
public static byte[] Decrypt(byte[] data, org.bouncycastle.asn1.pkcs.RSAPublicKey rsaPublicKey) throws Exception {
        Security.addProvider(new org.bouncycastle.jce.provider.BouncyCastleProvider());
        Cipher cipher = Cipher.getInstance("RSA/None/PKCS1Padding", "BC");
        Key publicKey = new java.security.interfaces.RSAPublicKey() {
            @Override
            public BigInteger getPublicExponent() {
                return rsaPublicKey.getPublicExponent();
            }

            @Override
            public String getAlgorithm() {
                return "RSA";
            }

            @Override
            public String getFormat() {
                return null;
            }

            @Override
            public byte[] getEncoded() {
                return new byte[0];
            }

            @Override
            public BigInteger getModulus() {
                return rsaPublicKey.getModulus();
            }
        };
        cipher.init(Cipher.DECRYPT_MODE, publicKey);
        // 对数据分段解密
        byte[] b = data;
        int inputLen = b.length;
        ByteArrayOutputStream out = new ByteArrayOutputStream();
        int offSet = 0;
        byte[] cache;
        int i = 0;
        while (inputLen - offSet > 0) {
            if (inputLen - offSet > MAX_DECRYPT_BLOCK) {
                cache = cipher.doFinal(b, offSet, MAX_DECRYPT_BLOCK);
            } else {
                cache = cipher.doFinal(b, offSet, inputLen - offSet);
            }
            out.write(cache, 0, cache.length);
            i++;
            offSet = i * MAX_DECRYPT_BLOCK;
        }
        return out.toByteArray();
    }
```

### pkcs1私钥加密
```java
public static byte[] Encrypt(String data,org.bouncycastle.asn1.pkcs.RSAPrivateKey rsaPrivateKey) throws Exception {
        Security.addProvider(new org.bouncycastle.jce.provider.BouncyCastleProvider());
        Cipher cipher = Cipher.getInstance("RSA/None/PKCS1Padding", "BC");
        Key privateKey = new java.security.interfaces.RSAPrivateKey() {
            @Override
            public BigInteger getPrivateExponent() {
                return rsaPrivateKey.getPrivateExponent();
            }

            @Override
            public String getAlgorithm() {
                return null;
            }

            @Override
            public String getFormat() {
                return null;
            }

            @Override
            public byte[] getEncoded() {
                return new byte[0];
            }

            @Override
            public BigInteger getModulus() {
                return rsaPrivateKey.getModulus();
            }
        };
        cipher.init(Cipher.ENCRYPT_MODE, privateKey);
        // 对数据分段
        byte[] b = data.getBytes();
        int inputLen = b.length;
        ByteArrayOutputStream out = new ByteArrayOutputStream();
        int offSet = 0;
        byte[] cache;
        int i = 0;
        while (inputLen - offSet > 0) {
            if (inputLen - offSet > MAX_ENCRYPT_BLOCK) {
                cache = cipher.doFinal(b, offSet, MAX_ENCRYPT_BLOCK);
            } else {
                cache = cipher.doFinal(b, offSet, inputLen - offSet);
            }
            out.write(cache, 0, cache.length);
            i++;
            offSet = i * MAX_ENCRYPT_BLOCK;
        }
        return out.toByteArray();
    }
```

### pkcs1私钥解密
```java
public static byte[] Decrypt(String data, org.bouncycastle.asn1.pkcs.RSAPrivateKey rsaPrivateKey) throws Exception{
        Security.addProvider(new org.bouncycastle.jce.provider.BouncyCastleProvider());
        Cipher cipher = Cipher.getInstance("RSA/None/PKCS1Padding", "BC");
        Key privateKey = new java.security.interfaces.RSAPrivateKey() {
            @Override
            public BigInteger getPrivateExponent() {
                return rsaPrivateKey.getPrivateExponent();
            }

            @Override
            public String getAlgorithm() {
                return null;
            }

            @Override
            public String getFormat() {
                return null;
            }

            @Override
            public byte[] getEncoded() {
                return new byte[0];
            }

            @Override
            public BigInteger getModulus() {
                return rsaPrivateKey.getModulus();
            }
        };
        cipher.init(Cipher.DECRYPT_MODE, privateKey);
        // 对数据分段解密
        byte[] b = data.getBytes();
        int inputLen = b.length;
        ByteArrayOutputStream out = new ByteArrayOutputStream();
        int offSet = 0;
        byte[] cache;
        int i = 0;
        while (inputLen - offSet > 0) {
            if (inputLen - offSet > MAX_DECRYPT_BLOCK) {
                cache = cipher.doFinal(b, offSet, MAX_DECRYPT_BLOCK);
            } else {
                cache = cipher.doFinal(b, offSet, inputLen - offSet);
            }
            out.write(cache, 0, cache.length);
            i++;
            offSet = i * MAX_DECRYPT_BLOCK;
        }
        return out.toByteArray();
    }
```

### 签名
* 对报文进行特征值计算，相关算法有：md5、sha1、sha256、sha512等
* 用私钥对特征值加密，获得签名数据

### 验签
* 对报文用公钥解密，获得特征值
* 对报文进行特征值计算
* 对比两次的特征值是否相等

---
>注意：

>- 公钥、私钥都可以加密解密
>- 以1024bit长度密钥为例，加密报文最大长度为1024/8-11，即117字节，解密报文最大长度为 1024/8，即128字节