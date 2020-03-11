### debugging
```
spamd -D -u spamd -q -C /etc/spamassassin/
```
```
echo -e "From: thorko@thorko.de\nTo:info@thorko.de\nSubject: Test\n\n" | spamc -u '$GLOBAL'
```

### errors
last time changed the following

```
allow_user_rules                 1
trusted_networks 167.86.80.54/32 2a02:c207:3003:6519::1/128
internal_networks !0/0
dns_available    1
dns_server       127.0.0.1:53
```

in database added
```
$GLOBAL report_safe 0
```

and also did run 
```
/usr/bin/sa-update --channelfile /etc/spamassassin/channels.text --gpgkeyfile /etc/spamassassin/keys.text -D --updatedir /etc/spamassassinp
```

## to many spam on webserver

check for permission problems on

```bash
/var/spool/bayes/bayes
```

## test spamassassin

```bash
spamd -D -q -u spamd -g spamd -C /etc/spamassassin/  >> /tmp/log 2>&1
echo -e "From: user\nTo:user\Subject: Test\n\n" | spamc -u 'dallase@nmgi.com'
```

## check learning

```bash
sa-learn --dbpath /var/spool/bayes/bayes  -C /etc/spamassassin/ --dump magic
0.000          0          3          0  non-token data: bayes db version
0.000          0       2126          0  non-token data: nspam
0.000          0      16120          0  non-token data: nham
0.000          0     150358          0  non-token data: ntokens
0.000          0 1565520251          0  non-token data: oldest atime
0.000          0 1580811599          0  non-token data: newest atime
0.000          0 1580806203          0  non-token data: last journal sync atime
0.000          0 1580806204          0  non-token data: last expiry atime
0.000          0   11059200          0  non-token data: last expire atime delta
0.000          0      15203          0  non-token data: last expire reduction count

sa-learn --spam <folder> --dbpath /var/spool/bayes/bayes
```

## disable auto expire bayes

this will disable the auto cleanup of bayes database

```bash
# set in local.cf
bayes_auto_expire 0
```

