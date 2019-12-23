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
