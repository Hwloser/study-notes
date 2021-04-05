# java keystore

## what is a cacerts file

The `cacerts` file is a collection of trusted certificate authority (CA) certificates. 

Oracle includes a `cacerts` file with its SSL support in the Java™ Secure Socket Extension (JSSE) tool kit and JDK. It contains certificate references for well-known Certificate authorities, such as VeriSign™. Its format is the "keystore" format defined by Oracle. 

An administrator can edit the `cacerts` file with a command line tool (also provided by Oracle) called keytool. For more information about keytool, see the Oracle website.

```text
The default password for the cacerts file supplied by Oracle is changeit. You must use this password to view the contents or to import a new certificate. For security reasons, change the default password.
```

The essential requirement is that the certificate authority that signed the Service Manager server’s certificate must be in the list of certificate authorities named in this file. 

The essential requirement is that the certificate authority that signed the Service Manager server’s certificate must be in the list of certificate authorities named in this file. To use a self-issued server certificate created with OpenSSL or a tool such as Microsoft Certificate Server™, you must import the certificate for this private certificate authority into the `cacerts` file that the client uses for SSL. If you do not import the certificate, SSL connections fail because the Java SSL implementation does not recognize the certificate authority.

## keystore summary

Keytool是一个Java数据证书的管理工具 ,Keytool将密钥（key）和证书（certificates）存在一个称为keystore的文件中。

在keystore里，包含两种数据：

1. 密钥实体（Key entity）——密钥（secret key）又或者是私钥和配对公钥（采用非对称加密）。
2. 可信任的证书实体（trusted certificate entries）——只包含公钥。

ailas(别名)每个keystore都关联这一个独一无二的alias，这个alias通常不区分大小写。