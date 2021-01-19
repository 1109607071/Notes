# 有用的资源

* [linux挑战：linuxupskillchallenge](https://github.com/snori74/linuxupskillchallenge)
收录了通过命令行对远程linux服务器进行系统管理所需要的技能
收录了商业在线Linux服务器管理课程的20个课程的所有资料，free

* 高性能博客模板：[eleventy-high-performance-blog](https://github.com/google/eleventy-high-performance-blog)
Google开源的为11ty静态博客收录的高性能博客模板。11ty.dev

* 破解小能手：[Ciphey](https://github.com/Ciphey/Ciphey)
在你不知道密钥或者密码的情况下自动解密加密、解码编码和破解哈希，只要输入加密的文本，即可获得解密的文本。
Ciphey可以在3秒或者更短时间内解决大部分加密问题。







# Command Line





## Linux









## MacOS





### Network



#### sshd

- Start sshd :

    `sudo /usr/bin/sshd - f /etc/sshd_config`

- Put sshd in system service :

    `sudo launchctl load -w /System/Library/LaunchDaemons/ssh.plist`
    `sudo launchctl unload -w /System/Library/LaunchDaemons/ssh.plist`

- Check whether it is on :

    `sudo launchctl list | grep sshd`



#### ssh

- Usage

    `ssh username@address`

- Example

    `ssh inkfinite@10.17.11.191`



#### ifconfig

查看本机地址



