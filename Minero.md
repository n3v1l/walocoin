# Minado con CPU

Se debe usar un minero de otra criptomoneda que está habilitada para el uso de lo necesario.
Paso previo, debes haber creado un nodo y tener billetera, ya que los datos necesarios son:

1. IP del nodo. El puerto necesario es el 9772/TCP del protocolo RPC

2. Nombre de usuario: Definido en el archivo walocoin.conf en la directiva **rpcuser=**

3. Clave del usuario: Definido en el archivo walocoin.conf en la directiva **rpcpassword=**

4. La dirección de tu billetera en formato bech32, es decir, debe comenzar con wlc
 

## Ubuntu 18.04
```bash
sudo apt-get install -y \
build-essential libssl-dev libcurl4-openssl-dev libjansson-dev libgmp-dev automake zlib1g-dev && \
git clone https://github.com/cryptozeny/cpuminer-opt-sugarchain.git && \
cd cpuminer-opt-sugarchain && \
./build-yespower.sh 
```

Luego se debe ejecutar de la siguiente forma:
```bash
./cpuminer -a scrypt -o http://IP_DEL_NODO:9772 -u ususario_del_walocoin.conf  -p password_del_walocoin.conf --coinbase-addr=TU_BILLETERA_BECH32 -D --no-stratum --no-longpoll
```

## Windows 

Descargar el minero desde la siguiente URL  https://github.com/cryptozeny/cpuminer-opt-sugarchain/releases/download/v3.8.8.1.7rc1/cpuminer-opt-sugarchain-v3.8.8.1.7-w64.zip

Luego se debe ejecutar de la siguiente forma:
```cmd
cpuminer.exe -a scrypt -o http://IP_DEL_NODO:9772 -u usuario_del_walocoin.conf  -p password_del_walocoin.conf --coinbase-addr=TU_BILLETERA_BECH32 -D --no-stratum --no-longpoll
```



# Minado en Pool con GPU y CPU
### Puedes ahora minar con tu CPU y GPU en el pool de Walocoin

## Para tarjetas GPU Nvidia
Usar el siguiente minero en Windows:

https://github.com/tpruvot/ccminer

Se puede compilar para Linux también.


## Cómo conectar al pool
### GPU
Se usa de la siguiente manera:

```cmd
ccminer -a scrypt -o stratum+tcp://blockexplorer.walocoin.xyz:3053 -u tu-direccion-de-billetera - p x
```

### CPU

Con elmismo minero usado para minar con CPU solo, se puede usar también con el pool:

```cmd
cpuminer -a scrypt -o stratum+tcp://blockexplorer.walocoin.xyz:3052 -u tu-direccion-de-billetera - p x
```

Nota: Puedes usar tu dirección legacy como la bech32 para minar en el pool.
