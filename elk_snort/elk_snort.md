# Instalacion de pila ELK e integracíon con Snort

## Instalación de Snort

Primero hacemos la instalacion en la maquina del paquete de snort.
```bash
sudo apt-get install snort
```
![instalacion de snort](/images/instalacion%20snort.png)

Durante la instalación se pregunta por el intervalo de direcciones para la red local (256): 192.168.1.0/24

Una vez instalado comprobamos que se encuentra activo.

```bash
sudo systemctl status snort
```
![estado de snort](/images/status%20snort.PNG)

Detenemos snort:
```bash
sudo systemctl stop snort
```

Editamos el fichero de configuración de Snort:

```bash
sudo nano /etc/snort/snort.conf
```

![configuracion de snort](/images/config%20snort.PNG)

Localizamos el apartado “syslog” y cambiamos las facilidades de alerta

![configuracion de rsyslog](/images/config%20syslog.PNG)

Rearrancamos rsyslog: 

```bash
sudo systemctl restart rsyslog
```

Comprobamos su estado: 

```bash
sudo systemctl status rsyslog
```

![status de rsyslog](/images/status%20rsyslog.png)

Ahora ya podemos arrancar snort: 

```bash
sudo systemctl start snort
```

IMPORTANTE: Nos debemos asegurar de que el fichero snort_alerts.log tenga todos los permisos:


```bash
sudo chmod 777 /var/log/snort_alerts.log
```

Verificamos que se ha creado el fichero snort_alerts.log y que se está llenando.

Incluimos las reglas de detección de protocolos ICMP y TCP en la configuración de Snort:

```bash
# Regla para detectar un ping (ICMP)
alert icmp any any -> $HOME_NET any (msg:"¡Trafico ICMP!"; sid:3000001;)

# Regla para detectar un SSH (TCP)
alert tcp any any -> any 22 (msg:"Acceso SSH"; sid:3000002;)
```

Abrimos un tail al fichero de alertas y atacamos con ping y ssh desde otra máquina,
para ver si se detectan:

```bash
tail -f /var/log/snort_alerts.log
```

![tail de snort_alerts.log](/images/tail_rsyslog.png)

Si todo ha sido satisfactorio, detenemos Snort y comprobamos que se ha parado antes de proseguir:

```bash
sudo systemctl stop snort
sudo systemctl status snort
```

## Instalación pila ELK

Previamente a la instalación hay que instalar el JDK

```bash
sudo apt install openjdk-8-jdk libjffi-java libjffi-jni
```

### Instalación de Elasticsearch

Descargamos con wget el paquete Elasticsearch y lo instalamos con dpkg:

```bash
sudo wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.0.0.deb
sudo dpkg -i elasticsearch-8.0.0.deb
```

Editamos el fichero de configuración de elasticsearch:

```bash
sudo nano /etc/elasticsearch/elasticsearch.yml
```

![configuracion de elasticsearch](/images/config%20elastic.PNG)

Arrancamos elasticsearch y consultamos su estado para comprobar si todo ha ido correctamente:

```bash
sudo systemctl start elasticsearch
sudo systemctl status elasticsearch
```

![estado de elasticsearch](/images/status%20elasticsearch.PNG)

Antes de proseguir, detenemos elasticsearch y comprobamos que está parado:

```bash
sudo systemctl stop elasticsearch
sudo systemctl status elasticsearch
```

### Instalación de Logstash

Se instalará de una forma similar a elasticsearch:

```bash
sudo wget https://artifacts.elastic.co/downloads/logstash/logstash-8.0.0.deb
sudo dpkg -i logstash-8.0.0.deb
```

Editamos el fichero de configuración de logstash:

```bash
sudo nano /etc/logstash/logstash.yml
```

![configuracion de logstash](/images/config%20logstash.PNG)

Arrancamos logstash y chequeamos su status:

```bash
sudo systemctl start logstash
sudo systemctl status logstash
```

![configuracion de logstash](/images/status%20logstash.PNG)

En nuestro caso, con nuestra solución no tenemos activo logstash

### Instalación de Kibana

Instalar Kibana mediante el siguiente procedimiento:

```bash
sudo wget https://artifacts.elastic.co/downloads/kibana/kibana-8.0.0-linux-x86_64.tar.gz
sudo mkdir /etc/kibana/
sudo tar -xvf kibana-8.0.0-linux-x86_64.tar.gz --strip 1 --directory /etc/kibana/
```

Configurar Kibana editando el fichero siguiente:

```bash
sudo nano /etc/kibana/kibana.yml
```

![configuracion de kibana](/images/config%20kibana.PNG)

Cambiamos el server.host:

```bash
server.host: 0.0.0.0
```

Arrancar Kibana y comprobar su status:

```bash
sudo systemctl start kibana
sudo systemctl status kibana
```

![estado de kibana](/images/status%20kibana.PNG)


### Últimos pasos

Reiniciar el host:

```bash
sudo reboot
```

Arrancar elasticsearch siempre ANTES que kibana:

```bash
sudo systemctl start elasticsearch
sudo systemctl status elasticsearch
```

Arrancar Kibana:

```bash
sudo systemctl start kibana
sudo systemctl status kibana
```

Arrancamos snort:

```bash
sudo systemctl start snort
sudo systemctl status snort
```

Por ultimo accedemos a Kibana via web.

Añadimos la integracion de snort

![intregracion de snort](/images/integracion_snort.PNG)

Instalamos el agente de Elastic

```bash
curl -L -O https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-8.2.0-linux-x86_64.tar.gz
tar xzvf elastic-agent-8.2.0-linux-x86_64.tar.gz
cd elastic-agent-8.2.0-linux-x86_64
sudo ./elastic-agent install --url=https://192.168.1.101:8220 --enrollment-token={token}FNUdWt1bi0xclpoUQ==
```

El archivo de configuración queda asi: 

![configuracion de agent](/images/config%20agent.PNG)

Después de un rato podremos ver como empiezan a aparecer datos en las distintas graficas del servicio.

### Host overview
![configuracion de agent](/images/panel1.png)
![configuracion de agent](/images/panel5.png)
![configuracion de agent](/images/panel6.png)

### Agent metrics
![configuracion de agent](/images/panel2.png)

### Discover
![configuracion de agent](/images/panel3.png)

### Syslog Dashboard
![configuracion de agent](/images/panel4.png)

