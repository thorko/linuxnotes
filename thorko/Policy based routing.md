Policy based routing
========================
erstellen der Table

```
echo "200 haproxy" >> /etc/iproute2/rt_tables
```

eine Rule hinzufügen

```
ip rule add to 10.112.4.205 lookup haproxy
ip rule add from 10.112.4.1 lookup haproxy
ip route add default via 10.112.4.1 dev eth1 table haproxy
```

Liste anschauen

```
ip rule list
```

Routing Regeln anschauen

```
ip route list table haproxy
```

```
ip rule del table haproxy
```