# UseSSROnRaspbian
How to use SSR on Raspbian

**Please make sure you are using user root. Installed Python and added it in your environment.** 

***


### Install the git:
    apt-get install git
### Clone the SSR-Backup Project From Github
    git clone https://github.com/shadowsocksrr/shadowsocksr.git
### Create your SSR Config File
    vim /etc/ssr.json
Your config looks like this:
```
{
    "server" : "yoursever",
    "server_port" : xxx,
    "server_udp_port" : 0,
    "password" : "xxxxxx",
    "method" : "chacha20",
    "protocol" : "auth_chain_a",
    "protocolparam" : "",
    "obfs" : "tls1.2_ticket_auth",
    "obfsparam" : "xxxxxx",
    "udp_over_tcp" : false,
    "local_address": "127.0.0.1",
    "local_port": 1080
}
```
### Create a start script
    vim ssr.sh
It looks like this:
```bash
#!/bin/bash

screen python /usr/bin/ssr/shadowsocksr/shadowsocks/local.py -c /etc/ssr.json >> ssr.log
```
### Download Chromium Extension Named SwitchyOmega on Its GitHub Project Release
> URL: https://github.com/FelisCatus/SwitchyOmega/releases

Then try to install it on your Chromium. Set the proxy config.

    sockv5 127.0.0.1 1080

### Also,We Can Use Privoxy To Set Global Proxy
  
    apt-get install privoxy
    
Then,edit its config `vim /etc/privoxy/config` , It looks like this:
 
```
  forward-socks5t / 127.0.0.1:1080 .
  listen-address  127.0.0.1:8118    
  listen-address  [::1]:8118  
  accept-intercepted-requests 1
```

The please restart its service `service privoxy restart`, if the service gives your some error info, recheck your privoxy config and try aggin.

Then add the global proxy in your system environment.

First, edit `vim /etc/environment`, add some thing like this:

```
  export http_proxy=http://127.0.0.1:8118
  export https_proxy=http://127.0.0.1:8118
```

Then, edit `vim /etc/profile` , add some thing like this too:

```
  export http_proxy=http://127.0.0.1:8118
  export https_proxy=http://127.0.0.1:8118
```

At the last, reboot your system and enjoy!

### Install the Screen
    apt-get install screen

### How to use
    chmod a+x ssr.sh
    ./ssr.sh
    service privoxy start
