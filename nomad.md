1#devops #полезняшки

Конфиги лежат по пути `/etc/nomad.d`

отображение логов по сервису
```
journalctl -u {systemctl service name} -f
```

nomad **очень** капризный в сертификатах. Проверь их в первую очередь!

Если не приходят запросы, то скорее всего закрыты порты.
Проверь с помощью `ufw status` или `iptables -L`

Чтобы убрать порты, используй nginx в качестве прокси к сервисам.

Чтобы посмотреть активные ноды, используй:
`nomad node status --address https://nomad.achievify.ru:4646`

По дефолту, nomad использует mTLS.

gitlab.serega-pirat.ru:5050/my-graduate-work/infrastructure/minio:latest
