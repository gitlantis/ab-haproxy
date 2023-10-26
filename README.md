# AB-HAProxy
This role installs following packages and configures them
- ntp
- monit
- haproxy



## Role parameters
### NTP

ntp is configured using the crontab which updates system time on midnight.

There is ```time.google.com``` time server is set by default you can give another time server by parameter ```ntp_server```:

```yml
- role: ab-haproxy
  ntp_server: ntp.server.url
```

### ini file path
We can give id file and path and it's download path to local machine

```vmid_path``` path may be specified during vagrant image intalization

```vmid_fetch_dir```'s value is ```/tmp``` directory by default

```yml
- role: ab-haproxy
  vmid_path: /path/to/id/file # this maybe configured during vagrant initalization
  vmid_fetch_dir: /path/to/fetch/file.ini 
```

### servers list
and to set server list to haproxy we can use following method

```yml
- role: ab-haproxy
  listener_address:
    - name: srv1
      ip: 192.168.1.103
      port: 80
    - name: srv2
      ip: 192.168.1.102
      port: 80
```

### monit
monit is configured so, if there is any problem with ahproxy, then restarts the haproxy

```yml
check process haproxy with pidfile /var/run/haproxy.pid
  start program = "/etc/init.d/haproxy start"
  stop program = "/etc/init.d/haproxy stop"
  if failed host 127.0.0.1 port 80 then restart
  if 3 restarts within 5 cycles then timeout
```