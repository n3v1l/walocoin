# Instalación de Nodo Blockchain Walocoin

Parte importante del ecosistema de Walocoin, son los nodos, que no son más que réplicas de la blockchain
de forma distribuida. Aqui se explica como se instala en dos de las distribuciones más utilizadas en servidores:

## Ubuntu 18.04

Hay que tener varias librerias antes para poder ejecutar tanto las billeteras como los demonios del nodo de Walocoin

Estos son los pasos a seguir:

1. Activar repositorio de Bitcoin
```bash
echo deb http://ppa.launchpad.net/bitcoin/bitcoin/ubuntu xenial main >> /etc/apt/sources.list
apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv D46F45428842CE5E
apt-get update
```

2. Instalar librerias necesarias para compilar y/o ejecutar
```bash
apt-get -y install ccache git libboost-system1.65.1 libboost-filesystem1.65.1 libboost-program-options1.65.1 libboost-thread1.65.1 libboost-chrono1.65.1 libssl1.0.0 libevent-pthreads-2.1-6 libevent-2.1-6 build-essential libtool autotools-dev automake pkg-config libssl-dev libevent-dev bsdmainutils libboost-system-dev libboost-filesystem-dev libboost-chrono-dev libboost-program-options-dev libboost-test-dev libboost-thread-dev libdb4.8-dev libdb4.8++-dev libminiupnpc-dev libzmq3-dev libqt5gui5 libqt5core5a libqt5dbus5 qttools5-dev qttools5-dev-tools libprotobuf-dev protobuf-compiler libqrencode-dev
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
./configure --disable-tests --disable-bench --without-gui
make -j$n  (donde $n es la cantidad de hilos con los que vas a compilar)
```


## CentOS 8 (CentOS 7 también, solo reemplazar dnf por yum)
### Gracias a @r4d3006 por esta guía

```bash
sudo dnf install -y autoconf automake boost-devel gcc-c++ git libevent-devel libtool openssl-devel wget
wget http://download.oracle.com/berkeley-db/db-4.8.30.NC.tar.gz
tar zxvf db-4.8.30.NC.tar.gz
sed -i 's/__atomic_compare_exchange/__atomic_compare_exchange_db/g' db-4.8.30.NC/dbinc/atomic.h
cd db-4.8.30.NC
cd build_unix/
../dist/configure --prefix=/usr/local --enable-cxx
make
sudo make install
cd ..
cd walocoin-0.18.2
./configure --disable-test --disable-bench --without-gui
make
sudo make install
sudo echo "LD_LIBRARY_PATH=/usr/local/lib" | sudo tee /etc/profile.d/walocoin.sh
sudo echo "export LD_LIBRARY_PATH" | sudo tee -a /etc/profile.d/walocoin.sh
```
Hay que loguearse de nuevo para que tome la configuración de las librerías.


Ya estarán creados los binarios necesarios para ejecutar el nodo, vamos a hacer la parte final ahora:

[ ] Nota personal: Automatizar esta parte

5. Copiar los binarios a la ruta /usr/local/bin
```bash
cp src/walocoind /usr/local/bin
cp src/walocoin-cli /usr/local/bin
cp src/walocoin-wallet /usr/local/bin
cp src/walocoin-tx /usr/local/bin
```

6. Creación de usuario para ejecución del demonio
```bash
adduser walocoin
Adding user `walocoin' ...
Adding new group `walocoin' (1000) ...
Adding new user `walocoin' (1000) with group `walocoin' ...
Creating home directory `/home/walocoin' ...
Copying files from `/etc/skel' ...
Enter new UNIX password: 
Retype new UNIX password: 
passwd: password updated successfully
Changing the user information for walocoin
Enter the new value, or press ENTER for the default
        Full Name []: 
        Room Number []: 
        Work Phone []: 
        Home Phone []: 
        Other []: 
Is the information correct? [Y/n] y
```

En CentOS no va a pasar esto, sólo lo creará y nada más

7. Crear a priori el directorio para walocoin y archivo de configuración
```bash
su - walocoin
mkdir .walocoin
cat << EOF >.walocoin/walocoin.conf
listen=1 
connect=0
bind=0.0.0.0
addnode=labmor.duckdns.org:54323
addnode=labmor.duckdns.org:54324
addnode=labmor.duckdns.org:54325
addnode=labmor.duckdns.org:54326
addnode=labmor.duckdns.org:54327
addnode=labmor.duckdns.org:54328
addnode=nodo1.walocoin.xyz
addnode=nodo2.walocoin.xyz
addnode=nodo3.walocoin.xyz
addnode=nodo4.walocoin.xyx:31202
addnode=nodo5.walocoin.xyz
txindex=1
server=1
rpcuser=minero
rpcpassword=b4c4n0s0
rpcbind=0.0.0.0
EOF
```

8. Crear servicio para habilitación del demonio
```bash
cat << EOF >/etc/systemd/system/walocoind.service
[Service]
User=walocoin
Group=walocoin
Type=simple
ExecStart=/usr/local/bin/walocoind -datadir=/home/walocoin/.walocoin/
KillMode=process
Restart=always
TimeoutSec=120
RestartSec=30
Environment=LD_LIBRARY_PATH=/usr/local/lib #Solo si es Centos 8 - remover para otros

[Install]
WantedBy=multi-user.target
EOF
```

9. Habilitación e inicio del demonio **walocoind**
```bash
systemctl daemon-reload
systemctl enable walocoind
systemctl start walocoind
```

Si todo ha salido bien, el nodo se inicializará y se conectará a la red para sincronizarse, se tiene que ver algo como esto
```bash
tail /home/walocoin/.walocoin/debug.log
2020-09-05T03:25:01Z UpdateTip: new best=5ba61892dbad24ebe9ad91d447685018b49b4607e950aa73920f01e46add1488 height=14391 version=0x20000000 log2_work=38.811245 tx=14431 date='2020-09-05T03:24:58Z' progress=1.000000 cache=0.1MiB(566txo)
2020-09-05T03:30:23Z UpdateTip: new best=c0a8b076b58c482af08f5d1466713c769a4d1d331b2a1d2b502665135d1143ec height=14392 version=0x20000000 log2_work=38.81144 tx=14432 date='2020-09-05T03:30:21Z' progress=1.000000 cache=0.1MiB(567txo)
2020-09-05T03:31:32Z UpdateTip: new best=f77eaea482927e4dcb69c561011c76dd00a9642d39045b8e6c5ce2d0e8685415 height=14393 version=0x20000000 log2_work=38.811634 tx=14433 date='2020-09-05T03:31:27Z' progress=1.000000 cache=0.1MiB(568txo)
2020-09-05T03:37:38Z UpdateTip: new best=8d560bf28b7aa6702f12f474b476f686f53fa9195ee7982435c1820db15ad2d2 height=14394 version=0x20000000 log2_work=38.811829 tx=14434 date='2020-09-05T03:37:37Z' progress=1.000000 cache=0.1MiB(569txo)
2020-09-05T03:38:19Z UpdateTip: new best=0950502c76bec41ca3855401bb9d083308c81ebb2e43d4205d914dc6298398ed height=14395 version=0x20000000 log2_work=38.812024 tx=14435 date='2020-09-05T03:38:18Z' progress=1.000000 cache=0.1MiB(570txo)
2020-09-05T03:38:33Z UpdateTip: new best=07c4929baa1fb006cf276760626f76c090578976c8a98bbec7ee56b61b55a161 height=14396 version=0x20000000 log2_work=38.812219 tx=14436 date='2020-09-05T03:38:32Z' progress=1.000000 cache=0.1MiB(571txo)
2020-09-05T03:42:20Z UpdateTip: new best=20e227c4549fb001af0c5412a6e84c8c00e292d2d3ab9b1330c6bee9c7da543b height=14397 version=0x20000000 log2_work=38.812413 tx=14437 date='2020-09-05T03:42:19Z' progress=1.000000 cache=0.1MiB(572txo)
2020-09-05T03:49:09Z UpdateTip: new best=08de55300cad67d40fdf3e9fc376d8ee23605f12910c6ebf356901f8340a0feb height=14398 version=0x20000000 log2_work=38.812608 tx=14438 date='2020-09-05T03:49:04Z' progress=1.000000 cache=0.1MiB(573txo)
2020-09-05T03:50:47Z UpdateTip: new best=f86b26ffd14e513d2516f826752d6fcbb41e4bed36830525abebb165b286cd7b height=14399 version=0x20000000 log2_work=38.812802 tx=14439 date='2020-09-05T03:50:43Z' progress=1.000000 cache=0.1MiB(574txo)
2020-09-05T03:53:20Z UpdateTip: new best=157f2c8118d8845620333ce3ca9effd0f54b036faf0ffbb1a82e9acf6e83305f height=14400 version=0x20000000 log2_work=38.812997 tx=14440 date='2020-09-05T03:53:19Z' progress=1.000000 cache=0.1MiB(575txo)
```

Obviamente tendrá que ser una fecha cercana (+/- 10 a 15 minutos máximo de tu hora.)

**Enhorabuena! Ya tienes un nodo operativo y eres parte de la red P2P blockchain de Walocoin**





