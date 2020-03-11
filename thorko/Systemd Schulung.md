Systemd Schulung
========================
[toc]
### Targets
- targets sind wie früher die runlevels - in diesen werden unit files gruppiert.
- https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/System_Administrators_Guide/sect-Managing_Services_with_systemd-Targets.html

verfügbare Targets auflisten
```
systemctl list-units --type target
```
das default Target
```
systemctl get-default
systemctl set-default name.target
```
das derzeitige Target wechseln
```
systemctl isolate multi-user.target
```

##### Rescue und Emergency mode
```
systemctl rescue | emergency
```
[Target](https://www.freedesktop.org/software/systemd/man/systemd.target.html)

###Unit Files
```
systemctl list-unit-files -t service # list unit files
systemctl mask/unmask                # stronger disable - link to /dev/null
systemctl add-wants/add-requires
systemctl list-dependencies apache2  # dependencies for apache2
systemctl list-unit-files --state=enabled # all enabled units
systemd-analyze blame                # start times of units
```
##### Remote Aufruf
benötigt ssh Zugriff
```
systemctl --host root@x10734 status cobblerd
```
####Unit Files schreiben
- enden immer auf .service
- eigene Unit Files am besten unter /etc/systemd/system/
  - benötigt kein systemctl daemon-reload

```
[Unit]
Description=Vim

[Service]
Type=forking
...

[Install]
WantedBy=default.target
```
nach Erstellen oder Ändern immer ein
```
systemctl daemon-reload
```

Service überwachen
```
Restart=no,on-success,on-failure,on-abnormal,on-watchdog,on-abort,always
```
[Service](https://www.freedesktop.org/software/systemd/man/systemd.service.html)

### Socket Files
- enden immer .socket

```
[Unit]
Before=ssh.service
[Socket]
ListenStrem=22
Accept=yes
[Install]
WantedBy=sockets.target
```
[Socket](https://www.freedesktop.org/software/systemd/man/systemd.socket.html)

#### alle unit files visualisieren
```
# show dependency graph
systemd-analyze dot | dot -Tsvg > /tmp/systemd.svg
```

### Timers
- enden mit .timer
- können cronjobs ablösen

```
systemctl list-timers
```
#### Einen Timer einrichten
```
[Unit]
Description=Backup
[Timer]
OnCalendar=daily
# oder OnActiveSec=5s und OnUnitActiveSec=10m
[Install]
WantedBy=timers.target
```
```
systemctl [start|enable] snapper-cleanup.timer
```
[Timer](https://www.freedesktop.org/software/systemd/man/systemd.timer.html)

### Path
- enden mit .path
- überwachen das Filesystem und starten *Unit*

```
[Unit]
...
[Path]
Unit=name.service
PathModified=/tmp
[Install]
WantedBy=multi-user.target
```
[Path](https://www.freedesktop.org/software/systemd/man/systemd.path.html)


### Logmeldungen - journalctl basics
```
journalctl -u apache2
```
dem Logfile folgen  
```
journalctl -u apache2 -f
```

die letzten Zeilen anschauen  
```
journalctl -xn 20
```

alle Logmeldungen anzeigen  
```
journalctl -xa
```

Logmeldungen ab einem bestimmten Zeitpunkt  
```
journalctl -xa -S "2017-06-07 00:00:00"
journalctl -xa -S [today|yesterday|10 hours ago]
```

alle Errormeldungen  
```
journalctl -p 3
//alle debug meldungen
journalctl -p 7
```


### Files und Directories erstellen/löschen 
```
systemctl status systemd-tmpfiles-clean.service
/etc/tmpfiles.d/*.conf
man 5 tmpfiles.d
systemctl status systemd-tmpfiles-clean.timer
```

### Systemd User Manager
unter ~/.config/systemd/user  
```
systemctl --user
systemctl list-timers --user
```