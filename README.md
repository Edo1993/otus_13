# otus_13
Мониторинг и алертинг

*Настроить дашборд с 4-мя графиками: память, процессор, диск, сеть*

Здесь была использована связка prometheus - grafana.
Графиков чуть больше, чем нужно, зато красивенько)

![Image alt](https://github.com/Edo1993/otus_13/raw/master/131.png)
![Image alt](https://github.com/Edo1993/otus_13/raw/master/132.png)
![Image alt](https://github.com/Edo1993/otus_13/raw/master/133.png)
![Image alt](https://github.com/Edo1993/otus_13/raw/master/134.png)
![Image alt](https://github.com/Edo1993/otus_13/raw/master/135.png)
![Image alt](https://github.com/Edo1993/otus_13/raw/master/136.png)

Кулстори о том, как запустить такую красотищу.

На Google Cloud Platform создала вм для выполнения дз (Ubuntu 18.04 в самой нищей конфигурации - 300$ тянем как можем).Подрубаемся по ssh и погнали.

Начнем с Node Exporter,чтобы сразу добавить его в Prometheus, как только до него дойдет очередь.
```
sudo useradd --no-create-home --shell /bin/false node_exporter
# Последняя версия:
# https://github.com/prometheus/node_exporter/releases
wget {скопипастченная ссылочка}
tar -xvzf node_exporter-{загруженная версия}.tar.gz
sudo cp node_exporter-{загруженная версия}/node_exporter /usr/local/bin/
```
Создаем [node_exporter.service](https://github.com/Edo1993/otus_13/blob/master/node_exporter.service) в /etc/systemd/system/

Запускаем :
```
sudo systemctl daemon-reload
sudo systemctl start node_exporter
sudo systemctl status node_exporter
sudo systemctl enable node_exporter
```
Проверяем, что метрики отдаются:
```
curl 'localhost:9100/metrics'
```

Prometheus
```
sudo useradd --no-create-home --shell /bin/false prometheus
# Последняя версия:
# https://github.com/prometheus/prometheus/releases
wget {скопипастченная ссылочка}
tar -xvzf prometheus-{загруженная версия}.tar.gz
sudo cp prometheus-{загруженная версия}/prometheus /usr/local/bin/
sudo cp prometheus-{загруженная версия}/promtool /usr/local/bin/

sudo mkdir /etc/prometheus
sudo cp -r prometheus-{загруженная версия}/consoles/ /etc/prometheus/consoles
sudo cp -r prometheus-{загруженная версия}/console_libraries/ /etc/prometheus/console_libraries
sudo cp prometheus-{загруженная версия}/prometheus.yml /etc/prometheus/
sudo chown -R prometheus:prometheus /etc/prometheus

sudo mkdir /var/lib/prometheus
sudo chown prometheus:prometheus /var/lib/prometheus
```
Добавить строки в /etc/prometheus/prometheus.yml, указывая, где Node Exporter:
```
  - job_name: 'node_localhost'
    static_configs:
    - targets: ['localhost:9100']
```
Создать [prometheus.service](https://github.com/Edo1993/otus_13/blob/master/prometheus.service) в /etc/systemd/system/

Запустим Prometheus:
```
sudo systemctl daemon-reload
sudo systemctl start prometheus
sudo systemctl status prometheus
sudo systemctl enable prometheus
```
Проверяем, что метрики отдаются промом:
```
curl 'localhost:9090/metrics'
```
