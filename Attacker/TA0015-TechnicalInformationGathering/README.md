## Anonymous FTP Login
FTP can allow anonymous login sometimes which can allow you to get valuable information from a computer.

`ftp ftp.example.com`
`login : anonymous`
`ls`


## HTTP ENUMERATION
`nc -v www.example.com`
HEAD / HTTP/1.1

    root@kaliInternet:~# HEAD / HTTP/1.1
    200 OK
    Content-Length: 912
    Content-Type: text/html
    Last-Modified: Mon, 13 Jan 2014 08:31:54 GMT
    Client-Date: Sun, 04 May 2014 00:40:54 GMT

    404 Not Found
    Cache-Control: no-store, no-cache, must-revalidate, post-check=0, pre-check=0
    Connection: close
    Date: Sun, 04 May 2014 00:40:50 GMT
    Pragma: no-cache
    Server: Apache/2
    Vary: Accept-Encoding,User-Agent
    Content-Type: text/html
    Expires: Thu, 19 Nov 1981 08:52:00 GMT
    Client-Date: Sun, 04 May 2014 00:40:55 GMT
    Client-Peer: 162.243.109.195:80
    Client-Response-Num: 1
    Set-Cookie: PHPSESSID=4q8en4snv5o0vf2l119msocfk6; path=/
    Set-Cookie: http_pyrocms=6FqZqZ0yzSMmpIwNULbqW0hsg94xyez2eUTWPQzynHGWbNScPBeHpKr4EH6Td9%2Blcj0OToh0jZxmngQZfSXkCXY0gu1K8Sl1suVZ1cwJ99ohdkC3FYaoqGR7nL2O38BKy202ONsvVrF6qaEGckwEUWZSG3vYbSRB6bUIgnjUT7Gh9Q5iLs1He5K7vWeS1BWKrp69zoSp%2Bd9zuMK%2FU5uyAilirmEcWDjtcgfyfbkSB8ersN%2FmbScYPYWXUv7Usnhwo7MXS1QkMIAKn%2Be9EZasyF6cogPywcep0V5DQQO9j419vAmnH88qUlaJaNkOqFzwDw8F1k1dVagZG8v0MQ30iQ%3D%3D; path=/; domain=http.com.au
    X-Powered-By: PHP/5.3.28

## SSL ENUMERATION
`openssl s_client -quiet -connect www.example.com:443`

## IPSec Discovery
UDP 500

To exploit an IPSec VPN in an attack you must enumerate the VPN infrastructure that manages key negotiations, Internet Key Exchange (IKE), to determine where exactly IPSec is and where to poke at it.  Port scan of UDP 500 wont show up as per the RFC 'incorrectly formatted packets should be silently ignored by any ipsec service.

`ike-scan` by NTA Monitor crafts packets that IKE is expecting so it allows the infrastructure to reveal information that it shouldnt.

Some information that it shows is as follows
- Is the VPN using Agressive or Main Mode
- What encryption protocols
- Device vendor

Agressive mode means the ability to brute force or dictionary attack the hash and discover the original key.