## MacOS

#### Start sshd
`sudo /usr/bin/sshd - f /etc/sshd_config`

#### Put sshd in system service
`sudo launchctl load -w /System/Library/LaunchDaemons/ssh.plist`
`sudo launchctl unload -w /System/Library/LaunchDaemons/ssh.plist`

#### Check whether it is on:
`sudo launchctl list | grep sshd`
