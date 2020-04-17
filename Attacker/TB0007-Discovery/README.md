### User Enumeration
`finger` displays information about system users. Port 79

`finger 0@192.168.0.5`

## LDAP Discovery
TCP/UDP 389 3268

`ldp.exe` can query AD
[MS Article](http://support.microsoft.com/kb/224543)


## Netbios Session Enumeration
TCP 139/445
SMB essentially provides things like file and printer sharing.  It can dish out wealths of information even to unauthenticated users.

### Null Sessions
Connecting to a null session can be performed by doing the following.

This connects to the hidden interprocess communications 'share'
`net use \\x.x.x.x\IPC$ "" /u:""`

This will enumerate all shares on the computer
`net view \\computername`


`Dumpsec` can perform the same task.

To check to see what applications run on startup do the following.

reg query \\x.x.x.x\HKLM\SOFTWARE\MICROSOFT\Windows\CurrentVersion\Run

To pull user information with dumpsec on remote pc do the following.
dumpsec /computer\\x.x.x.x /rpt=useronly /saveas=tsv /outfile=c:\temp\users.txt

### ENUM
`enum` is another great SMB tool.  Use as follows (by default it uses null sessions.
`enum -U x.x.x.x lists users`
`enum -G x.x.x.x lists groups`
`enum -P x.x.x.x pulls password policies.`

To use username and password with enum do the following
`enum -u [username] -p [password] -G x.x.x.x`

password guessing with enum
`enum -D -u [username] -f [wordfile] x.x.x.x`

Establish SMB Session from Linux
`smbclient -L [winipaddr] -U [username] -p 445`

Connect to SMB Share / Windows Share on Linux
`smbclient //[Win IP Addr]/test -U [username] -p 445`
use `ls` to list folders, `get` to download, `cd` to change dir.

Pull wealth of info from SMB on Linux
`rpcclient -U [username] [WinIpAddr]`
then type `password`.  Some commands are as follows
`enumdomusers`
`enumalsgroups`
`lsaenumsid`
`lookupnames`
`lookupsids`
`srvinfo`

## NFS Enumeration
NFS TCP/UDP 2049
Network File System.
Linux has a simple tool called showmount that can enumerate through shares.

`showmount -e 192.168.1.5`

## RPC Enumeration
`rwho` UDP 513 and rusers (RPC Program 100002)

`rwho` returns users currently logged onto a remote host running the rwho deamon

`rwho x.x.x.x`

`rusers` returns similar output with a little more info if you use the '-l' switch
`rusers -l x.x.x.x`

### Unix RPC Enumeration
TCP/UDP 111 and 32771

RPC employs a service called portmapper (now known as rpcbind) to abitrate between client requests and ports that it dynamically assigns to listening applications.

RPCINFO is the equivalent of finger for enumerating RPC apps.
`rpcinfo -p x.x.x.x`

If port 111 was blocked you can try the following.  The 100083 refers to the program number.  In this case 'Tooltalk Database server'

`rpcinfo -n 32776 -t x.x.x.x 100083`

Nmap also has a command line tool for this.
`nmap -sS -sR x.x.x.x`

### Network Information System
RPC Program 100004

Network information system.

Was originally implemeted as as distributed database of network info.  Unfortuantely has no security.

Once you know the NIS domain name of a server, you can get any of its NIS maps by using a simple RPC query.  They have information such as passwd file contents. Scanner below.
[NMAP PScan](http://nmap.org/scanners/pscan.c)

### Net View
UDP 137
Serves as a distributed naming system for Microsoft NT
`net view /domain`
`Domain`
`---------------------------------`
`CORLEONE`
`BARZINI_DOMAIN`
`TTAGGLIA_DOMAIN`
`BRAZZI`
`The command completed successfully`

`net view /domain:corleone`
`\\computername`

You can dump the whole NBT Name table all at once by using nbtsat and nbtscan

`nbtstat -a x.x.x.x`

### SMTP Enumeration

Comes built in with two commands that allow for enumeration of users

`VRFY`: Which confirms names of valid users
`EXPN`: which reveals the actual delivery address of aliases and mailing lists.

`jeremy.kister.net/code/perl/vrfy.pl‎`  is a tool to speed up the vrfy process.

### SNMP Enumeratioin
UDP 161
Most commonly implmented password is 'public' and 'private'

`snmputil` can be used to enumerate SNMP
`snmpwalk` is another tool that can be used.

### TFTP Enumeration
UDP 69

`tftp 192.168.0.6`
`connect 192.168.0.6`
`get /etc/passwd /tmp/passwd.cracklater`
`quit`

Or to get cisco gear config
`running-config`
`startup-config`
`.config`
`config`
`run`