# Instalación de Billeteras Walocoin

## Windows 

1. Descargar el [Instalador de Walocoin](https://github.com/n3v1l/walocoin/releases/download/0.18.2/walocoin-0.18.2-win64-setup.exe)

2. Ejecutarlo para instalarlo, dejar todo por defecto.

3. Cuando se ejecute, cerrar la aplicación

4. Ir a la ruta c:\users\%username%\appdata\roaming\walocoin\

5. Copiar el contenido del archivo walocoin.conf ubicado en config/walocoin.conf al directorio

6. Ejecutar de nuevo la billetera 

7. Ahora se sincronizará con la blockchain.


## Ubuntu 18.04 - Kali RR - LinuxMint - Debian

Hay que tener varias librerias antes para poder ejecutar tanto las billeteras como los demonios del nodo de Walocoin. 

La única diferencia, es la versión de las librerías libboost. hay que adaptarlas de acuerdo a la distribución que se está usando

Estos son los pasos a seguir:

1. Activar repositorio de Bitcoin
```bash
echo deb http://ppa.launchpad.net/bitcoin/bitcoin/ubuntu xenial main >> /etc/apt/sources.list
apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv D46F45428842CE5E
apt-get update
```

2. Instalar librerias necesarias para compilar y/o ejecutar

###Librerias Ubuntu 18.04
```bash
apt-get -y install ccache git libboost-system1.65.1 libboost-filesystem1.65.1 libboost-program-options1.65.1 libboost-thread1.65.1 libboost-chrono1.65.1 libssl1.0.0 libevent-pthreads-2.1-6 libevent-2.1-6 build-essential libtool autotool$```
```

###Librerias Kali RR
```bash
apt -y install ccache git libboost-system1.71.0 libboost-filesystem1.71.0 libboost-program-options1.71.0 libboost-thread1.71.0 libboost-chrono1.71.0 libssl1.0.2 libevent-pthreads-2.1-7 libevent-2.1-7 build-essential libtool autotools-dev automake pkg-config libssl-dev libevent-dev bsdmainutils libboost-system-dev libboost-filesystem-dev libboost-chrono-dev libboost-program-options-dev libboost-test-dev libboost-thread-dev libdb4.8-dev libdb4.8++-dev libminiupnpc-dev libzmq3-dev libqt5gui5 libqt5core5a libqt5dbus5 qttools5-dev qttools5-dev-tools libprotobuf-dev protobuf-compiler libqrencode-dev
```


3. Descargar el tar con el código fuente cualquiera de las dos opciones son validas

#### Opción Git

```bash
git clone https://github.com/n3v1l/walocoin.git
```

#### Opción Código Fuente en Release
```bash
wget https://github.com/n3v1l/walocoin/releases/download/0.18.2/walocoin-0.18.2.tar.gz
tar xvf walocoin-0.18.2.tar.gz
```

4. Compilado
```bash
cd walocoin-0.18.2
./autogen.sh
./configure --disable-tests --disable-bench
make -j$n  (donde $n es la cantidad de hilos con los que vas a compilar)
```

5. Copiar el archivo del binario a la ubicación /usr/local/bin

```bash
cp src/walocoin-qt /usr/local/bin
chmod +x /usr/local/bin/walocoin-qt
```

6. Ejecutar el binario:
```bash
walocoin-qt
```
Esperar a que se ejecute y luego cerrarlo.

7. Ir a la carpeta del walocoin. Esta carpeta estará en home de tu usuario
```bash
cd /home/usuario/.walocoin
```

8. Copiar desde walocoin-0.18.2/config/walocoin.conf a la carpeta .walocoin

9. Ejecutar la billetera de nuevo, ahora se sincronizará.


## Fedora 

WIP

 
