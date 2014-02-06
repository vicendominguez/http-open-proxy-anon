http-open-proxy-anon.nse
========================

Description
-----------

It is a new NSE script for NMap. The target is try to find full anonymous proxies.

The Nmap Scripting Engine (NSE) is one of Nmap's most powerful and flexible features. It allows users to write (and share) simple scripts to automate a wide variety of networking tasks. Those scripts are then executed in parallel with the speed and efficiency you expect from Nmap. Users can rely on the growing and diverse set of scripts distributed with Nmap, or write their own to meet custom needs.

It is based on **http-open-proxy**: http://nmap.org/nsedoc/scripts/http-open-proxy.html

http-open-proxy detects **open** proxys but we don't know if is possible to hide our IP with them. My PoC try to solve that.


Install
-------

* Upload ip.php whereever you want. it needs to be reacheable via web.
* Copy the http-open-proxy-anon.nse to the nmap script path.


Configure
---------

Edit the http-open-proxy-anon.nse to configure the correct URL where the ip.script is uploaded.

```
local test_url = "http://www.xxxx.net/ip.php"
local hostname = "www.xxxx.net"
```

Results
-------

The output is your new IP if it is anonymous. Example:

```
Nmap scan report for 192.192.192.130
Host is up (0.39s latency).
PORT     STATE    SERVICE
3128/tcp open     squid-http
| http-open-proxy-anon: Potentially OPEN proxy.
|_Methods supported:  GET CONNECTION IP:11.222.33.444
8080/tcp closed   http-proxy
8081/tcp filtered blackice-icecap
```

If you look at the output, your target host (192.192.192.130) is different to the answered host (11.222.33.444) ;)

What to know
------------

The NSE script has two key parameters: timeout and ports.

**Timeout**:

Configured to 10 seconds. This value is not affected with the --max-rtt-timeout  --initial-rtt-timeout  and -host-timeout from NMAP.
The timeout is defined in the *proxy.lua* nmap lib. With this line:

```
socket:set_timeout(10000)
```


**Ports**:

(Via source code)

```
portrule = shortport.port_or_service({8123,3128,8000,8080},{'polipo','squid-http','http-proxy'})
```

You can add the ports changing that line.


Environment
-----------

I am using this NSE script with:

* Linux Mint 13
* FreeBSD 9.2
* Nmap 6.25


Notes
-----

* I am not a developer. I am a sysadmin. Sorry for my bad programming practices etc etc ......
* Spanish post about this PoC: http://www.securityartwork.es/2013/05/14/a-vueltas-con-la-deteccion-de-proxys/
