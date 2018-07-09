# vps_1

OS: Ubuntu Server 16.04

## ssh

``` Bash shell
vi /etc/ssh/sshd_config
```

``` vi
:%s/PermitRootLogin yes/PermitRootLogin no
:wq
```

## [vlmcsd](https://github.com/Wind4/vlmcsd)

1. Download the Source Code package from [the latest release](https://github.com/Wind4/vlmcsd/releases/latest)
2. Read [vlmcsd/README.compile-and-pre-built-binaries](https://github.com/Wind4/vlmcsd/blob/HEAD/README.compile-and-pre-built-binaries) and do compiling
3. Copy ```./bin/vlmcsd``` to ```/usr/local/bin/```
4. Create ```vlmcsd.service```, like this:

``` ini
[Unit]
Description=Vlmcsd Service

[Service]
ExecStart=/usr/local/bin/vlmcsd -D
Restart=always

[Install]
WantedBy=multi-user.target
```

``` Bash shell
cp vlmcsd.service /etc/systemd/system/

systemctl enable vlmcsd.service
systemctl start vlmcsd.service
```

## [shadowsocks](https://github.com/shadowsocks/shadowsocks)

1. ```pip install shadowsocks```
2. Create ```shadowsocks.json``` in ```/etc/```, like this:

``` JSON
{
  "server":"0.0.0.0",
  "local_address":"127.0.0.1",
  "local_port":1080,
  "port_password":{
    "*10001*":"*password_for_first_port*",
    "*20002*":"*password_for_second_port*"
   },
   "timeout":300,
   "method":"aes-256-cfb",
   "fast_open":true
}
```

3. Create ```ssserver.service```, like this:

``` ini
[Unit]
Description=Shadowsock Server Service
ConditionPathExists=/etc/shadowsocks.json

[Service]
ExecStart=/usr/local/bin/ssserver -c /etc/shadowsocks.json
Restart=always

[Install]
WantedBy=multi-user.target
```

``` Bash shell
cp ssserver.service /etc/systemd/system/

systemctl enable ssserver.service
systemctl start ssserver.service
```
