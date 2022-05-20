# Instalacion de la pila ELK

* Instalamos Java y Nginx

```bash
sudo apt-get install openjdk-8-jdk
sudo apt-get install nginx
```

* Añadimos el repositorio de elasticsearch

```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -

sudo apt-get install apt-transport-https

echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee –a /etc/apt/sources.list.d/elastic-7.x.list
```

## Instalacion de Elasticsearch

```bash
sudo apt-get update

sudo apt-get install elasticsearch
```

* Configuramos el puerto y la ip del servicio

```bash
sudo nano /etc/elasticsearch/elasticsearch.yml
```
* Cambiamos la ip a localhost y reiniciamos el servicio

```bash
network.host: localhost
sudo systemctl enable elasticsearch.service
```
* Cambiamos la configuracion de JavaVM

```bash
sudo nano /etc/elasticsearch/jvm.options

-Xms512m
-Xmx512m
```
## Instalacion de Kibana

```bash
sudo apt-get install kibana
```

* Configuramos Kibana

```bash
sudo nano /etc/kibana/kibana.yml

server.port: 5601
server.host: "localhost"
elasticsearch.hosts: ["http://localhost:9200"]
```

* Iniciamos kibana y permitimos el trafico al puerto 5001

```bash
sudo systemctl start kibana
sudo systemctl enable kibana

sudo ufw allow 5601/tcp
```

## Instalacion de Longstash

```bash
sudo apt-get install logstash

sudo systemctl start logstash
sudo systemctl enable logstash
```
* Instalamos y configuramos Filebeat

```bash
sudo apt-get install filebeat

sudo nano /etc/filebeat/filebeat.yml

output.logstash
     hosts: ["localhost:5044"]

sudo filebeat modules enable system
sudo filebeat setup --index-management -E output.logstash.enabled=false -E 'output.elasticsearch.hosts=["localhost:9200"]'
```

* Encendemos el servicio de Filebeat

```bash
sudo systemctl start filebeat
sudo systemctl enable filebeat
```
