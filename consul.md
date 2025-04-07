
Главная фишка HashiCorp Consul — свой DNS, позволяющий резолвить сервисы в домене .consul.
Но для этого надо настроить consul. По дефолту, в Linux используется в качестве DNS `systemd-resolved`
Поэтому, по пути `/etc/systemd/resolved.conf.d/consul.conf` с таким вот содержимым:
```
[Resolve]
DNS=92.53.105.243:8600
Domains=~consul
```
и перезапусти с помощью `systemctl restart systemd-resolved`

Если все еще фигня какая то, чекни статус и есть ли еще один, конфликтующий DNS сервер(dnsmasq например) с помощью `sudo netstat -tuln | grep 53` 