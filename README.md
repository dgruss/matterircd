我是光年实验室高级招聘经理。
我在github上访问了你的开源项目，你的代码超赞。你最近有没有在看工作机会，我们在招软件开发工程师，拉钩和BOSS等招聘网站也发布了相关岗位，有公司和职位的详细信息。
我们公司在杭州，业务主要做流量增长，是很多大型互联网公司的流量顾问。公司弹性工作制，福利齐全，发展潜力大，良好的办公环境和学习氛围。
公司官网是http://www.gnlab.com,公司地址是杭州市西湖区古墩路紫金广场B座，若你感兴趣，欢迎与我联系，
电话是0571-88839161，手机号：18668131388，微信号：echo 'bGhsaGxoMTEyNAo='|base64 -D ,静待佳音。如有打扰，还请见谅，祝生活愉快工作顺利。

# matterircd
[![Join the IRC chat at https://webchat.freenode.net/?channels=matterircd](https://img.shields.io/badge/IRC-matterircd-green.svg)](https://webchat.freenode.net/?channels=matterircd)

Minimal IRC server which integrates with [mattermost](https://www.mattermost.org) and [slack](https://www.slack.com)
Tested on FreeBSD / Linux / Windows

# Docker
Run the irc server on port 6667. You'll need to specify -bind 0.0.0.0:6667 otherwise it only listens on 127.0.0.1 in the container.

```
docker run -p 6667:6667 42wim/matterircd:latest -bind 0.0.0.0:6667
```

Now you can connect with your IRC client to port 6667 on your docker host.

# FreeBSD
Install the port.
```
# pkg install matterircd
```
Or with a local ports tree.
```
$ cd /usr/ports/net-im/matterircd
# make install clean
```

Enable the service.
```
echo "matterircd_enable="YES" >> /etc/rc.conf
```
Copy the default configuration and modify to your needs.
```
# cp /usr/local/etc/matterircd/matterircd.toml.sample /usr/local/etc/matterircd/matterircd.toml
```
Start the service.
```
# service matterircd start
```

# Compatibility
* Matterircd works with slack and mattermost 5.x

Master branch of matterircd should always work against latest STABLE mattermost release.

# Features

* support direct messages / private channels / edited messages
* auto-join/leave to same channels as on mattermost
* reconnects with backoff on mattermost restarts
* support multiple users
* support channel/direct message backlog (messages when you're disconnected from IRC/mattermost)
* search messages (/msg mattermost search query)
* scrollback support (/msg mattermost scrollback #channel limit)
* restrict to specified mattermost instances
* set default team/server
* WHOIS, WHO, JOIN, LEAVE, NICK, LIST, ISON, PRIVMSG, MODE, TOPIC, LUSERS, AWAY, KICK, INVITE support
* support TLS (ssl)
* support LDAP logins (mattermost enterprise) (use your ldap account/pass to login)
* &users channel that contains members of all teams (if mattermost is so configured) for easy messaging
* supports mattermost roles (shows admins with @ status for now)
* gitlab auth hack by using mmtoken cookie (see https://github.com/42wim/matterircd/issues/29)
* mattermost personal token support

# Binaries

You can find the binaries [here](https://github.com/42wim/matterircd/releases/latest)

# Building

Go 1.8+ is required 
Make sure you have [Go](https://golang.org/doc/install) properly installed, including setting up your [GOPATH] (https://golang.org/doc/code.html#GOPATH)

```
cd $GOPATH
go get github.com/42wim/matterircd
```

You should now have matterircd binary in the bin directory:

```
$ ls bin/
matterircd
```

# Usage

```
Usage of ./matterircd:
  -bind string
        interface:port to bind to. (default "127.0.0.1:6667")
  -conf string
        config file
  -debug
        enable debug logging
  -interface string
        interface to bind to (deprecated: use -bind)
  -mminsecure
        use http connection to mattermost
  -mmserver string
        specify default mattermost server/instance
  -mmskiptlsverify
        skip verification of mattermost certificate chain and hostname
  -mmteam string
        specify default mattermost team
  -port int
        Port to bind to (deprecated: use -bind)
  -restrict string
        only allow connection to specified mattermost server/instances. Space delimited
  -tlsbind string
        interface:port to bind to. (e.g 127.0.0.1:6697)
  -tlsdir string
        directory to look for key.pem and cert.pem. (default ".")
  -version
        show version
```

Matterircd will listen by default on localhost port 6667.
Connect with your favorite irc-client to localhost:6667

For TLS support you'll need to generate certificates.   
You can use this program [generate_cert.go](https://golang.org/src/crypto/tls/generate_cert.go) to generate key.pem and cert.pem

## Config file
See [matterircd.toml.example](https://github.com/42wim/matterircd/blob/master/matterircd.toml.example)   
Run with `matterircd -conf matterircd.toml`

## Mattermost user commands

Login with user/pass

```
/msg mattermost login <server> <team> <username/email> <password>
```

Login with personal token

```
/msg mattermost login <server> <team> <username/email> token=<yourpersonaltoken>
```


Or if it is set up to only allow one host:

```
/msg mattermost login <username/email> <password>
```

Search
```
/msg mattermost search query
```

Scrollback
```
/msg mattermost scrollback <channel> <limit>
e.g. /msg mattermost scrollback #bugs 100 shows the last 100 messages of #bugs
```

Mark messages in a channel/from a user as read (when DisableAutoView is set).
```
/msg mattermost updatelastviewed <channel>
/msg mattermost updatelastviewed <username>
```
## Slack user commands
Get a slack token on https://api.slack.com/custom-integrations/legacy-tokens

Login

```
/msg slack login <token>
```

Or use team/login/pass to login
```
/msg slack login <team> <login> <password>
```
After login it'll show you a token you can use for the token login

## Docker

A docker image for easily setting up and running matterircd on a server is available at [docker hub](https://hub.docker.com/r/42wim/matterircd/).

## Examples

1. Login to your favorite mattermost service by sending a message to the mattermost user
![login](http://snag.gy/aAop5.jpg)

2. You'll be auto-joined to all the channels you're a member of
![channel](http://snag.gy/IzlXR.jpg)

3. Chat away
![chat](http://snag.gy/JyFd7.jpg)
![mmchat](http://snag.gy/3qMd1.jpg)

Also works with windows ;-)
![windows](http://snag.gy/cGSCA.jpg)

If you use chrome, you can easily test with [circ](https://chrome.google.com/webstore/detail/circ/bebigdkelppomhhjaaianniiifjbgocn?hl=en-US)

## Guides

Here are some external guides and documentation that might help you get up and
running more quickly:

* [Breaking out of the Slack walled garden](https://purpleidea.com/blog/2018/06/22/breaking-out-of-the-slack-walled-garden/)
