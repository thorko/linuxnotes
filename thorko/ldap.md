
```
yum -y install nss-pam-ldapd
```

/etc/pam.d/password-auth
```
#%PAM-1.0
# This file is auto-generated.
# User changes will be destroyed the next time authconfig is run.
auth        required      pam_env.so
auth        required      pam_faildelay.so delay=2000000
auth        sufficient    pam_unix.so nullok try_first_pass
auth        requisite     pam_succeed_if.so uid >= 1000 quiet_success
auth        sufficient    pam_ldap.so use_first_pass
auth        required      pam_deny.so

account     required      pam_unix.so
account     sufficient    pam_localuser.so
account     sufficient    pam_succeed_if.so uid < 1000 quiet
account     [default=bad success=ok user_unknown=ignore] pam_ldap.so
account     required      pam_permit.so

password    requisite     pam_pwquality.so try_first_pass local_users_only retry=3 authtok_type=
password    sufficient    pam_unix.so sha512 shadow nullok try_first_pass use_authtok
password    sufficient    pam_ldap.so use_authtok

password    required      pam_deny.so

session     optional      pam_keyinit.so revoke
session     required      pam_limits.so
-session     optional      pam_systemd.so
session     [success=1 default=ignore] pam_succeed_if.so service in crond quiet use_uid
session    required    pam_mkhomedir.so skel=/etc/skel/ umask=0022
session     optional      pam_ldap.so
session     required      pam_unix.so
```

/etc/pam.d/system-auth
```
#%PAM-1.0
# This file is auto-generated.
# User changes will be destroyed the next time authconfig is run.
auth        required      pam_env.so
auth        required      pam_faildelay.so delay=2000000
auth        sufficient    pam_unix.so nullok try_first_pass
auth        requisite     pam_succeed_if.so uid >= 1000 quiet_success
auth        sufficient    pam_ldap.so use_first_pass
auth        required      pam_deny.so

account     required      pam_unix.so
account     sufficient    pam_localuser.so
account     sufficient    pam_succeed_if.so uid < 1000 quiet
account     [default=bad success=ok user_unknown=ignore] pam_ldap.so
account     required      pam_permit.so

password    requisite     pam_pwquality.so try_first_pass local_users_only retry=3 authtok_type=
password    sufficient    pam_unix.so sha512 shadow nullok try_first_pass use_authtok
password    sufficient    pam_ldap.so use_authtok
password    required      pam_deny.so

session     optional      pam_keyinit.so revoke
session     required      pam_limits.so
-session     optional      pam_systemd.so
session     [success=1 default=ignore] pam_succeed_if.so service in crond quiet use_uid
session    required    pam_mkhomedir.so skel=/etc/skel/ umask=0022
session     optional      pam_ldap.so
session     required      pam_unix.so
```

/etc/nslcd.conf
```
uid nslcd
gid ldap
uri ldap://ldap-master.test-integration.rzneo
base o=denic
scope sub
base   group  ou=Groups,o=denic
base   passwd ou=Users,o=denic
ssl no
tls_cacertdir /etc/openldap/cacerts
```
---
## sudo
/etc/sudo-ldap.conf
```
TLS_CACERTDIR /etc/openldap/cacerts
SASL_NOCANON    on
URI ldap://ldap-master.test-integration.rzneo
BASE ou=Users,o=denic
sudoers_base ou=Users,o=denic
sudoers_debug 0
```

---
## sshkey ldap

```
dn: cn=sshkey,cn=schema,cn=config
objectClass: olcSchemaConfig
cn: sshkey
olcAttributeTypes: ( 1.3.6.1.4.1.24552.500.1.1.1.13 NAME 'sshPublicKey' DESC 'OpenSSH Public key' EQUALITY octetStringMatch SYNTAX 1.3.6.1.4.1.1466.115.121.1.40 )
olcObjectClasses: ( 1.3.6.1.4.1.24552.500.1.1.2.0 NAME 'ldapPublicKey' DESC 'OpenSSH LPK objectclass' SUP top AUXILIARY MUST uid MAY sshPublicKey )
```


```
ldapmodify -Y EXTERNAL -H ldapi:///.
ldapadd -Y EXTERNAL -H ldapi:/// -f sshpubkey.ldif
```


in sshd_config
```
AuthorizedKeysCommand /usr/bin/ssh-ldap-pubkey-wrapper
AuthorizedKeysCommandUser nobody
```

ltb for user frontend
[link](https://github.com/ltb-project/self-service-password)
