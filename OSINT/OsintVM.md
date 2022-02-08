# LinuxMint OSINT

Receta para crear una máquina para investigación OSINT.

Descargamos ISO de LinuxMint y con shasum -a 256 comprobamos la integridad de la imagen.

Actualizamos el sistema:

```bash
sudo apt update
sudo apt upgrade
sudo apt install kate git vim python3-pip
```

## Herramientas para Youtube

Herramientas OSINT para Youtube:

```bash
sudo apt install youtube-dl 
pip3 install yt-dlp
```

**IMPORTANTE**: Python instala sus *ejecutables* en la ruta *$HOME/.local/bin*, luego tenemos que añadir esta ruta a nuestra variable PATH.

1. Usamos youtube-dl para descargas normales
2. Usamos yt-dlp para descargas complejas como vídeos fragmentados, usando para ello la lista de vídeos (fichero m3u8)

Para automatizar esta tarea, podemos crearnos un script bash:

```bash
#!/bin/bash

DIR=$HOME/OSINT
VIDEO_DIR="$DIR/Youtube"
marcatiempo=$(date +%Y-%m-%d-%H:%M:%S)

if [[ ! -d $DIR ]]
then
    echo "No existía el directorio $DIR, creado"
    mkdir $DIR
fi 

if [[ ! -d $VIDEO_DIR ]]
then 
    echo "No existía el directorio $VIDEO_DIR, creado"
    mkdir $VIDEO_DIR
fi

url=$(zenity --entry --title "Descarga de Youtube" --text "URL:")
yt-dlp "$url" -o "$VIDEO_DIR/$marcatiempo%(title)s.%(ext)s" \
 --all-subs -i | zenity --progress --pulsate --no-cancel --auto-close \
 --title "Descargando vídeo..."
```

Ahora faltaría darle permisos de ejecución al archivo con:

```bash
chmod +x youtube.sh
```

## Herramientas para Instagram

Para esta red social tenemos herramientas tan interesantes como:

* [instaloader](https://instaloader.github.io/)
* [instalooter](https://github.com/althonos/InstaLooter)
* [toutatis](https://github.com/megadose/toutatis)
* [Osintgram](https://github.com/Datalux/Osintgram)

```bash
cd 
cd OSINT
git clone https://github.com/Datalux/Osintgram
cd Osintgram
pip3 install -r requirements.txt
python3 main.py -c photos elmonaguillo
python3 main.py -c fwersemail elmonaguillo
```

Para aprender más sobre Osintgram, en [este vídeo](https://www.youtube.com/watch?v=NWyqSbnsvGU) nos dan más pistas.

## Herramientas de usuario / correo electrónico

Existen multitud de herramientas como:

1. [Sherlock](https://github.com/sherlock-project/sherlock)
2. [Holene](https://github.com/megadose/holehe)
3. [SocialScan](https://github.com/iojw/socialscan)
4. [WhatsMyName](https://github.com/WebBreacher/WhatsMyName)
5. [Email2Phone](https://github.com/martinvigo/email2phonenumber)

Instalación de **Sherlock**:

```bash
# clone the repo
$ git clone https://github.com/sherlock-project/sherlock.git

# change the working directory to sherlock
$ cd sherlock

# install the requirements
$ python3 -m pip install -r requirements.txt
```

Instalación y uso de **Holehe**:

```bash
pip3 install holehe
holehe juan@sincorreo.com
```

## Herramientas para analizar redes (CLI)

1. [amass](https://github.com/OWASP/Amass): Busca subdominios dentro del indicado.
2. [TheHarvester](https://github.com/laramies/theHarvester):
3. [Photon](https://github.com/s0md3v/Photon)
4. [Carbon14](https://github.com/Lazza/Carbon14)
5. [EyeWhitness](https://github.com/FortyNorthSecurity/EyeWitness)

```bash
docker pull caffix/amass
docker run -v OUTPUT_DIR_PATH:/.config/amass/ caffix/amass enum -share -d iesvirgendelcarmen.com
```

## Herramientas para investigar redes (Web)

* WiFi: [Wigle](https://wigle.net): Permite buscar por SSID, BSSID, área, etc.
* Por IP: [Shodan](https://shodan.io): La cuenta gratuita permite 100 búsquedas al mes. Nos permite ver puertos abiertos sin hacer escaneo activo. También se puede hacer consultas sencillas sin login así: [http://https://beta.shodan.io/host/88.26.231.151](https://beta.shodan.io/host/88.26.231.151) -en este momento esta es la IP del IES-.
* Reverse DNS: [ViewDNS](https://viewdns.info), ejemplo: [https://viewdns.info/reverseip/?host=2.139.173.60](https://viewdns.info/reverseip/?host=2.139.173.60)
* Geolocalización: [iplocation.net](https://iplocation.net). Consulta varios servicios de geolocalización. Si lo probamos con la IP del centro, no termina de llegar con precisión a nuestra localización, pero se acerca bastante: [https://www.iplocation.net/ip-lookup?query=88.26.231.151&submitIP+Lookup](https://www.iplocation.net/ip-lookup?query=88.26.231.151&submitIP+Lookup).

## Herramientas de Telegram

* [Fuente: Awesome Telegram OSINT](https://github.com/ItIsMeCall911/Awesome-Telegram-OSINT)

## Herramientas de Office 365

* [PWC - Office365 Extractor](https://github.com/PwC-IR/Office-365-Extractor)

## Herramientas búsqueda de código

* [Grep.APP](https://grep.app/)
* [SourceGraph](https://sourcegraph.com)

## Herramientas para investigar Proton Mail y VPN

* [ProtOSINT](https://github.com/pixelbubble/ProtOSINT)


