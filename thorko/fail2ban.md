# fail2ban

```bash
# test regex
fail2ban-regex -c /etc/fail2ban/fail2ban.conf -vvv "Dec 07 13:12:36 auth-worker(2682807): Info: sql(edu@thorko.de,87.246.7.34): unknown user" "auth-worker\([0-9]+\): Info: sql\(\S*,<HOST>\): (?:unknown user)"
```

## sample filter conf

```
[INCLUDES]

before = common.conf

[Definition]

failregex = auth-worker\([0-9]+\): Info: sql\(\S*,<HOST>\): (?:unknown user)
            (?:imap-login: Info: Aborted login).*rip=<HOST>,.*


mode = normal

ignoreregex =

journalmatch = _SYSTEMD_UNIT=dovecot.service

datepattern = ^%%b %%d %%H:%%M:%%S
```

## sample jail.local

```
[INCLUDES]
before = paths-arch.conf

[DEFAULT]
bantime = 1200
findtime = 600
maxretry = 10

[dovecot]
enabled = true
port    = pop3,pop3s,imap,imaps,submission,465,sieve
logpath = /var/log/dovecot.log
maxretry = 2
action = %(action_)s

```

