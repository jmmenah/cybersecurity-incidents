# Instalación del NOC
## 1. Descarga e instalación de pfsense
Descargamos la ISO de pfsense más estable de su [web](url "www.pfsense.org/download").

Importamos la iso al servidor de proxmox, en la siguiente ruta:

``/var/lib/vz/templates/iso ``

Creamos una nueva maquina virtual en nuestro SOC asignándole 4GB de memoria RAM y 10GB de memoria ROM.

Importante asignarle dos adaptadores de red, una en modo puente y otra para la red interna.

Agregamos la ISO a la maquina virtual creada y comenzamos la instalación del pfsense.

Cuando haya cargado la ISO nos aparecerá el menú de arranque de pfsense, seleccionamos la opción 1 para instalarlo desde cero.
Configuraremos el teclado eligiendo nuestro idioma y la distribución.
Luego elegiremos como instalar el SO, en nuestro caso usamos Auto[UFS]BIOS.
Comenzará la instalación y al final nos preguntará si queremos lanzar una consola, seleccionamos "NO" y el sistema se reiniciará.

Una vez reiniciado ya está instalado el sistema y podemos usarlo.

## 2. Configuración del pfsense

Primero tenemos que configurar las IP de los dos adaptadores de red.

El adaptador WAN cogerá la IP por DHCP, en caso contrario, la agregamos manualmente.

En el adaptador LAN le configuraremos una IP para poder configurarlo desde el navegador.

Al acceder al pfsense desde el navegador, entraremos con la contraseña y usuario por defecto y seguimos los pasos del asistente de configuración.

## 3. Instalación de pfBlocker-devel

Desde pfsense accedemos a la pestaña System y seleccionamos Package Manager, dentro pulsamos Available Packages y buscamos pfBlocker-devel.
Pulsamos instalar cuando lo encontremos.

Una vez instalado nos aparecerá en la pestaña Firewall donde podremos configurarlo.

Dentro de la configuración pinchamos en DNSBL y activamos su filtro. Luego vamos a general y activamos pfBlockerNG, dejando la configuración por defecto.

Con el filtro DNSBL activaremos los filtros de AD, Pornografía, Spyware y Trackers de Shallalist, dentro de la pestaña DNSBL Category.

Podemos activar el modo Phyton que es mas eficiente con listas grandes desde la pestaña DNSBL en DNSBL mode.

## 4. Instalación de snort

Desde pfsense accedemos a la pestaña System y seleccionamos Package Manager, dentro pulsamos Available Packages y buscamos snort.
Pulsamos instalar cuando lo encontremos.

Una vez instalado nos aparecerá en la pestaña Servicios donde podremos configurarlo.

Para configurarlo nos dirigimos la pestaña Global Settings y alli activamos las reglas de la comunidad, clickando en Enable Snort GPLv2.

Por ultimo señalamos Enable Snort VRT y guardamos los cambios.

