## Hide files
Rename file to  '.' or '.."
or
Rename front of file to either '.' or '.."


## Shell Clearing
Clear the shell
`~/.bash`

`kill [pid]`
or
`killall bash`

## Log Files to clear
/var/log/messages
/var/log/secure


## Log Clearing
`logclean-ng` [logclean-ng](https://packetstormsecurity.com/search/files/?q=logclean-ng) is a useful tool for cleaning logs.  Use the man page to find out.

The first place to start is to check the syslog.conf file to see where the logs are being put in.

After that use `logcleaner-ng` to clean what ever you need to.

If an attacker wants to they can disable the bash history file like the following.

`unset HISTFILE; unset SAVEHIST`

Or an intruder may link `.bash_history` to `/dev/null`
`ln -s /dev/null ~/.bash_history`