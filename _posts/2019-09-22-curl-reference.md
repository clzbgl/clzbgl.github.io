---
layout: post
title:  "curl速查手册"
date:   2019-09-22 19:51:42 +0800
categories: tech
typora-root-url: ..
---

curl就是客户端(client)的URL工具的意思。

熟练curl取代postman

## 基本用法

### 查看网站源码

```shell
curl baidu.com
```

![](/assets/img/2019/09/20190922195655.png)

### 将服务器的回应保存成文件，等同于wget

```shell
curl -o baidu.txt baidu.com
```

### 重定向，针对301

> 默认不跟随重定向

```shell
curl -L www.sina.com
```

### HEAD，显示头信息

```shell
# -i参数可以显示http response的头信息，连同网页代码一起
# -I参数则是只显示http response的头信息
curl -i www.sina.com
```

![](/assets/img/2019/09/20190922200143.png)

### 发送表单信息

```shell
# get
curl example.com/form.cgi?data=xxx
# post
curl -X POST --data "data=xxx" example.com/form.cgi
```

### 发送POST请求的数据体

```shell
curl -d 'login=emma&password=123' -X POST https://google.com/login
curl -d 'login=emma' -d 'password=123' -X POST https://google.com/login
# 使用-d参数以后，HTTP请求会自动加上标头Content-Type: application/x-www-form-urlencoded
# 并且会自动将请求转为POST方法，因此可以省略-X POST

# -d 参数可以读取本地文本文件的数据，向服务器发送
curl -d '@data.txt' https://google.com/login
```

### 将发送的数据进行URL编码

```shell
# 让curl给数据表单编码，数据有空格需要进行URL编码
curl -X POST --data-urlencode "data=April 1" example.com/form.cgi
```

### 构造查询字符串

```shell
# get请求
# -G用来构造URL的查询字符串
curl -G -d 'q=kitties' -d 'count=20' https://google.com/search
```

### HTTP动词，-X

```shell
curl -X POST www.example.com
curl -X DELETE www.example.com
```

### 文件上传???

```shell
curl --form upload=@localfilename --form press=OK [URL]
```

### 上传二进制文件

```shell
curl -F 'file=@photo.png' https://google.com/profile
# 上面命令会给HTTP请求加上标头Content-Type: multipart/form-data
# 然后将文件 photo.png作为file字段上传
# -F参数可以指定MIME类型
curl -F 'file=@photo.png;type=image/png' https://google.com/profile
# -F参数可以指定文件名，服务器接收到的文件名为me.png
curl -F 'file=@photo.png;filename=me.png' https://google.com/profile
```

### Referer字段

> 表示你是从哪里跳转过来的

```shell
# -e --referer
curl --referer http://www.example.com http://www.example.com
# -H参数可以通过直接添加标头Referer，达到同样效果
curl -H 'Referer:https://google.com?q=example' https://www.example.com
```

### 增加头信息

```shell
# -H --header
curl --header "Content-Type:application/json" http://example.com
curl -H 'Accept-Language: en-US' -H 'Secret-Message: xyzzy' https://google.com
```

### User Agent字段

> 表示客户端的设备信息，服务器有时会根据这个字段，针对不同设备，返回不同格式的网页，比如手机版和桌面版

```shell
# Wdindows Chrome
Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/77.0.3865.90 Safari/537.36

# -A --user-agent
curl --user-agent "[User Agent]" [URL]
# 也可以通过-H参数直接指定
curl -H 'User-Agent: php/1.0' https://gogole.com
```

### Cookie

```shell
# -b --cookie
curl --cookie "name=xxx" www.example.com

# cookie的值，可以从http response头信息的'Set-Cookie'字段中得到
# -c cookie-file可以保存服务器返回的cookie文件, 
# -b cookie-file可以使用这个文件作为cookie信息，进行后续的请求
curl -c cookies.txt http://example.com
curl -b cookies.txt http://example.com
```

### 发送JSON请求

```shell
curl -d '{"login": "emma", "pass": "123"}' -H 'Content-Type: application/json' https://google.com/login
```

### HTTP认证

```shell
# -u --user
curl --user name:password example.com
curl https://bob:12345@google.com/login
# 上面命令能够识别 URL 里面的用户名和密码，将其转为上个例子里面的 HTTP 标头。
```

### 指定代理

```shell
curl -x socks5://james:cats@mtproxy.com:8080 www.baidu.com
curl -x 127.0.0.1:8081 www.baidu.com
# 代理协议默认为HTTP
```

![](/assets/img/2019/09/20190922202315.png)

## 调试

### 模拟网速环境

> 限制HTTP请求和回应的宽带，模拟慢网速的环境

```shell
curl --limit-rate 200k https://google.com
```



### 显示通信过程

```shell
# -v参数可以显示一次http通信的整个过程，包括端口连接和http request头信息
curl -v www.sina.com
# 查看更详细的通信过程,输出原始的二进制数据
curl --trace output.txt www.sina.com
curl --trace-ascii output.txt www.sina.com
```

![](/assets/img/2019/09/20190922200453.png)

### 不输出错误信息和进度信息

```shell
curl -s https://www.example.com
# 不产生任何输出
curl -s -o /dev/null https://google.com
```

### 只输出错误信息

```shell
curl -S https://example.com
```

### 跳过SSL检测

```shell
curl -k https://www.example.com
```

## 查看帮助

```shell
[root@s13 ~]# curl -h
Usage: curl [options...] <url>
Options: (H) means HTTP/HTTPS only, (F) means FTP only
     --anyauth       Pick "any" authentication method (H)
 -a, --append        Append to target file when uploading (F/SFTP)
     --basic         Use HTTP Basic Authentication (H)
     --cacert FILE   CA certificate to verify peer against (SSL)
     --capath DIR    CA directory to verify peer against (SSL)
 -E, --cert CERT[:PASSWD] Client certificate file and password (SSL)
     --cert-type TYPE Certificate file type (DER/PEM/ENG) (SSL)
     --ciphers LIST  SSL ciphers to use (SSL)
     --compressed    Request compressed response (using deflate or gzip)
 -K, --config FILE   Specify which config file to read
     --connect-timeout SECONDS  Maximum time allowed for connection
 -C, --continue-at OFFSET  Resumed transfer offset
 -b, --cookie STRING/FILE  String or file to read cookies from (H)
 -c, --cookie-jar FILE  Write cookies to this file after operation (H)
     --create-dirs   Create necessary local directory hierarchy
     --crlf          Convert LF to CRLF in upload
     --crlfile FILE  Get a CRL list in PEM format from the given file
 -d, --data DATA     HTTP POST data (H)
     --data-ascii DATA  HTTP POST ASCII data (H)
     --data-binary DATA  HTTP POST binary data (H)
     --data-urlencode DATA  HTTP POST data url encoded (H)
     --delegation STRING GSS-API delegation permission
     --digest        Use HTTP Digest Authentication (H)
     --disable-eprt  Inhibit using EPRT or LPRT (F)
     --disable-epsv  Inhibit using EPSV (F)
 -D, --dump-header FILE  Write the headers to this file
     --egd-file FILE  EGD socket path for random data (SSL)
     --engine ENGINGE  Crypto engine (SSL). "--engine list" for list
 -f, --fail          Fail silently (no output at all) on HTTP errors (H)
 -F, --form CONTENT  Specify HTTP multipart POST data (H)
     --form-string STRING  Specify HTTP multipart POST data (H)
     --ftp-account DATA  Account data string (F)
     --ftp-alternative-to-user COMMAND  String to replace "USER [name]" (F)
     --ftp-create-dirs  Create the remote dirs if not present (F)
     --ftp-method [MULTICWD/NOCWD/SINGLECWD] Control CWD usage (F)
     --ftp-pasv      Use PASV/EPSV instead of PORT (F)
 -P, --ftp-port ADR  Use PORT with given address instead of PASV (F)
     --ftp-skip-pasv-ip Skip the IP address for PASV (F)
     --ftp-pret      Send PRET before PASV (for drftpd) (F)
     --ftp-ssl-ccc   Send CCC after authenticating (F)
     --ftp-ssl-ccc-mode ACTIVE/PASSIVE  Set CCC mode (F)
     --ftp-ssl-control Require SSL/TLS for ftp login, clear for transfer (F)
 -G, --get           Send the -d data with a HTTP GET (H)
 -g, --globoff       Disable URL sequences and ranges using {} and []
 -H, --header LINE   Custom header to pass to server (H)
 -I, --head          Show document info only
 -h, --help          This help text
     --hostpubmd5 MD5  Hex encoded MD5 string of the host public key. (SSH)
 -0, --http1.0       Use HTTP 1.0 (H)
     --ignore-content-length  Ignore the HTTP Content-Length header
 -i, --include       Include protocol headers in the output (H/F)
 -k, --insecure      Allow connections to SSL sites without certs (H)
     --interface INTERFACE  Specify network interface/address to use
 -4, --ipv4          Resolve name to IPv4 address
 -6, --ipv6          Resolve name to IPv6 address
 -j, --junk-session-cookies Ignore session cookies read from file (H)
     --keepalive-time SECONDS  Interval between keepalive probes
     --key KEY       Private key file name (SSL/SSH)
     --key-type TYPE Private key file type (DER/PEM/ENG) (SSL)
     --krb LEVEL     Enable Kerberos with specified security level (F)
     --libcurl FILE  Dump libcurl equivalent code of this command line
     --limit-rate RATE  Limit transfer speed to this rate
 -l, --list-only     List only names of an FTP directory (F)
     --local-port RANGE  Force use of these local port numbers
 -L, --location      Follow redirects (H)
     --location-trusted like --location and send auth to other hosts (H)
 -M, --manual        Display the full manual
     --mail-from FROM  Mail from this address
     --mail-rcpt TO  Mail to this receiver(s)
     --mail-auth AUTH  Originator address of the original email
     --max-filesize BYTES  Maximum file size to download (H/F)
     --max-redirs NUM  Maximum number of redirects allowed (H)
 -m, --max-time SECONDS  Maximum time allowed for the transfer
     --metalink      Process given URLs as metalink XML file
     --negotiate     Use HTTP Negotiate Authentication (H)
 -n, --netrc         Must read .netrc for user name and password
     --netrc-optional Use either .netrc or URL; overrides -n
     --netrc-file FILE  Set up the netrc filename to use
 -N, --no-buffer     Disable buffering of the output stream
     --no-keepalive  Disable keepalive use on the connection
     --no-sessionid  Disable SSL session-ID reusing (SSL)
     --noproxy       List of hosts which do not use proxy
     --ntlm          Use HTTP NTLM authentication (H)
 -o, --output FILE   Write output to <file> instead of stdout
     --pass PASS     Pass phrase for the private key (SSL/SSH)
     --post301       Do not switch to GET after following a 301 redirect (H)
     --post302       Do not switch to GET after following a 302 redirect (H)
     --post303       Do not switch to GET after following a 303 redirect (H)
 -#, --progress-bar  Display transfer progress as a progress bar
     --proto PROTOCOLS  Enable/disable specified protocols
     --proto-redir PROTOCOLS  Enable/disable specified protocols on redirect
 -x, --proxy [PROTOCOL://]HOST[:PORT] Use proxy on given port
     --proxy-anyauth Pick "any" proxy authentication method (H)
     --proxy-basic   Use Basic authentication on the proxy (H)
     --proxy-digest  Use Digest authentication on the proxy (H)
     --proxy-negotiate Use Negotiate authentication on the proxy (H)
     --proxy-ntlm    Use NTLM authentication on the proxy (H)
 -U, --proxy-user USER[:PASSWORD]  Proxy user and password
     --proxy1.0 HOST[:PORT]  Use HTTP/1.0 proxy on given port
 -p, --proxytunnel   Operate through a HTTP proxy tunnel (using CONNECT)
     --pubkey KEY    Public key file name (SSH)
 -Q, --quote CMD     Send command(s) to server before transfer (F/SFTP)
     --random-file FILE  File for reading random data from (SSL)
 -r, --range RANGE   Retrieve only the bytes within a range
     --raw           Do HTTP "raw", without any transfer decoding (H)
 -e, --referer       Referer URL (H)
 -J, --remote-header-name Use the header-provided filename (H)
 -O, --remote-name   Write output to a file named as the remote file
     --remote-name-all Use the remote file name for all URLs
 -R, --remote-time   Set the remote file's time on the local output
 -X, --request COMMAND  Specify request command to use
     --resolve HOST:PORT:ADDRESS  Force resolve of HOST:PORT to ADDRESS
     --retry NUM   Retry request NUM times if transient problems occur
     --retry-delay SECONDS When retrying, wait this many seconds between each
     --retry-max-time SECONDS  Retry only within this period
 -S, --show-error    Show error. With -s, make curl show errors when they occur
 -s, --silent        Silent mode. Don't output anything
     --socks4 HOST[:PORT]  SOCKS4 proxy on given host + port
     --socks4a HOST[:PORT]  SOCKS4a proxy on given host + port
     --socks5 HOST[:PORT]  SOCKS5 proxy on given host + port
     --socks5-basic  Enable username/password auth for SOCKS5 proxies
     --socks5-gssapi Enable GSS-API auth for SOCKS5 proxies
     --socks5-hostname HOST[:PORT] SOCKS5 proxy, pass host name to proxy
     --socks5-gssapi-service NAME  SOCKS5 proxy service name for gssapi
     --socks5-gssapi-nec  Compatibility with NEC SOCKS5 server
 -Y, --speed-limit RATE  Stop transfers below speed-limit for 'speed-time' secs
 -y, --speed-time SECONDS  Time for trig speed-limit abort. Defaults to 30
     --ssl           Try SSL/TLS (FTP, IMAP, POP3, SMTP)
     --ssl-reqd      Require SSL/TLS (FTP, IMAP, POP3, SMTP)
 -2, --sslv2         Use SSLv2 (SSL)
 -3, --sslv3         Use SSLv3 (SSL)
     --ssl-allow-beast Allow security flaw to improve interop (SSL)
     --stderr FILE   Where to redirect stderr. - means stdout
     --tcp-nodelay   Use the TCP_NODELAY option
 -t, --telnet-option OPT=VAL  Set telnet option
     --tftp-blksize VALUE  Set TFTP BLKSIZE option (must be >512)
 -z, --time-cond TIME  Transfer based on a time condition
 -1, --tlsv1         Use => TLSv1 (SSL)
     --tlsv1.0       Use TLSv1.0 (SSL)
     --tlsv1.1       Use TLSv1.1 (SSL)
     --tlsv1.2       Use TLSv1.2 (SSL)
     --trace FILE    Write a debug trace to the given file
     --trace-ascii FILE  Like --trace but without the hex output
     --trace-time    Add time stamps to trace/verbose output
     --tr-encoding   Request compressed transfer encoding (H)
 -T, --upload-file FILE  Transfer FILE to destination
     --url URL       URL to work with
 -B, --use-ascii     Use ASCII/text transfer
 -u, --user USER[:PASSWORD]  Server user and password
     --tlsuser USER  TLS username
     --tlspassword STRING TLS password
     --tlsauthtype STRING  TLS authentication type (default SRP)
     --unix-socket FILE    Connect through this UNIX domain socket
 -A, --user-agent STRING  User-Agent to send to server (H)
 -v, --verbose       Make the operation more talkative
 -V, --version       Show version number and quit
 -w, --write-out FORMAT  What to output after completion
     --xattr        Store metadata in extended file attributes
 -q                 If used as the first parameter disables .curlrc

```



参考：

- [Curl Cookbook](https://catonmat.net/cookbooks/curl)
- [curl网站开发指南](http://www.ruanyifeng.com/blog/2011/09/curl.html)
- [curl用法指南](http://www.ruanyifeng.com/blog/2019/09/curl-reference.html)