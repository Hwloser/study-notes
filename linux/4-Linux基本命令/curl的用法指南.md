# curl

## curl简介

curl是常用的命令行工具，用于请求web服务器。curl是由client和URL工具组合而成的。

不带有任何参数时，curl及时 GET 请求。

向 www.baidu.com 发送 GET 请求，server 返回的内容会在命令行中输出。
```bash
 curl https://www.baidu.com
```

## Option

### -A 

`-A` 参数指定客户端的用户代理标头，即 `User-Agent`。curl的默认用户代理字符串是 `curl/[version]`。

`curl -A 'Mozilia/5.0 (windows NT 10.0); Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.100 Safari/537.36' https:google.com`

还可以移除 `User-Agent`标头。

也可以通过 `-H` 参数来指定标头，更改 `User-Agent`。

```bash
 curl -H 'User-Agent: php/1.0' https://google.com
```

### -b

`-b`参数用来向服务器发送 `Cookie`。

```bash
curl -b 'fool=bat;foo2=bar2' https://google.com
```

将 `cookie` 写入 `cookie.txt` 文件。

```bash
# 写入cookie文件
curl -c cookie.txt https://www.google.com
# 读取cookie文件
curl -b cookie.txt https://www.google.com
```

### -c

`-c`参数将服务器设置的 `Cookie` 写入一个文件。

将服务器的 HTTP 回应所设置 Cookie 写入文本文件cookies.txt。
```bash
$ curl -c cookies.txt https://www.google.com
```

### -d

`-d`参数用于发送 POST 请求的数据体。

```bash
curl -d 'login=huanwei&passwd=123' -X POST https://google.com
# 或者
curl -d 'login=huanwei' -d 'passwd=123' -X POST https://google.com
```

`-d`参数可以读取本地文本文件的数据，向服务器发送。

读取data.txt文件的内容，作为数据体向服务器发送。
```bash
curl -d '@data.txt' https://google.com
```

### --data-urlencode

`--data-urlencode`参数等同于`-d`，发送 POST 请求的数据体，区别在于会自动将发送的数据及进行URL编码。

在下面的代码中，发送的数据`hello world`中间有一个空格，需要进行URL编码。
```
curl --data-urlencode 'comment=hello world' https://google.com
```

### -e

`-e`参数用来设置 HTTP 的标头 `Referer`，表示请求的来源。

将`Referer`标头设置为`https://google.com?q=example`。
```bash
curl -e 'https://google.com?q=example' https://www.example.com
```
`-H`参数可以直接添加标头`Referer`，达到相同的效果。

```bash
curl -H 'Refer: https://google.com?q=example' https://google.com
```

### -F

`-F`参数用来向服务器上传二进制文件。

给HTTP请求头加上标头`Content-Type: multipart/form-data`，然后将文件`photo.png`作为 file 字段上传。
```bash
curl -F 'file=@phote.png' https://google.com/profile
```

`-F`参数可以指定 MIME 类型。

```bash
curl -F 'file=@photo.png; type=image/png' https://google.com/profile
```
上面命令指定 MIME 类型为image/png，否则 curl 会把 MIME 类型设为application/octet-stream。

-F参数也可以指定文件名。

```bash
$ curl -F 'file=@photo.png;filename=me.png' https://google.com/profile
```

### -G

`-G`用于构造 URL 的查询字符串。

```bash
curl -G -d 'q=post' -d 'count=post' https://google.com/search
```

上面命令会发出一个 GET 请求，实际请求的 URL 为https://google.com/search?q=kitties&count=20。如果省略--G，会发出一个 POST 请求。

如果数据需要 URL 编码，可以结合--data--urlencode参数。

### -H

-H参数添加 HTTP 请求的标头。

```bash
curl -H 'Accept-Languaue: en-US' https://google.com
```

上面命令添加 HTTP 标头Accept-Language: en-US。

```bash
curl -H 'Accept-Language: en-US' -H 'Secret-Message: message' https://google.com
```
上面的命令添加两个HTTP标头。

```bash
curl -d '{"login":"loginName","passwd":"passwd"}' -H 'Content-Type: application/json' https://google.com
```

上面命令添加 HTTP 请求的标头是Content-Type: application/json，然后用-d参数发送 JSON 数据。

### -i

-i参数打印出服务器回应的 HTTP 标头。

```bash
curl -i https://google.com
```

上面命令收到服务器回应后，先输出服务器回应的标头，然后空一行，再输出网页的源码。

### -I

-I参数向服务器发出 HEAD 请求，然会将服务器返回的 HTTP 标头打印出来。

```bash
curl -I https://google.com
```

上面命令输出服务器对 HEAD 请求的回应。

--head参数等同于-I。

```bash
curl --head https://www.example.com
```

### -o

-o 参数将服务器的response保存成文件，等同于wget命令。

```bash
curl -o local_file.html https://www.example.com
```

### -O

-O参数将服务器回应保存成文件，并将 URL 的最后部分当作文件名。

```bash
curl -O  https://www.example.com/foo/bar.html
```

### -u

-u 参数用于设置服务器认证的用户名和密码。

```bash
curl -u 'userName:password' https://google.com
```

上面的命令将用户名设置为`userName`，将密码设置为`passowrd`，然后将其转为 HTTP 的标头 `Authorization: Basic 。。。。。。`。

curl 可以识别 URL 中的用户名和密码。

```bash
curl https://bob:12345@google.com/login
```

上面命令能够识别 URL 里面的用户名和密码，将其转为上个例子里面的 HTTP 标头。

```bash
curl -u 'bob' https://google.com/login
```

上面命令只设置了用户名，执行后，curl 会提示用户输入密码。