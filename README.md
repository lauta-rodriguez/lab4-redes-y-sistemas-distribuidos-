# Análisis de enrutamiento en redes de topología de anillo

# Grupo 29

## Integrantes:

- Lara Kurtz, lara.kurtz@mi.unc.edu.ar
- Lautaro Rodríguez, lautaro.rodriguez@mi.unc.edu.ar

# Abstract

Este informe analiza el comportamiento del tráfico de red en una topología de anillo utilizando diferentes algoritmos de enrutamiento. El objetivo es comparar el rendimiento de estos algoritmos en términos del `retardo` de los paquetes. Para lograrlo, se implementaron dos algoritmos: uno **inicial** proporcionado por la cátedra y otro **modificado** llamado "Hop count modificado".

# Introducción

La red sobre la que se va a trabajar es una red con una topología en forma de anillo compuesta por 8 nodos numerados del 0 al 7.

![topologia](/images/General_Network.png)

Cada nodo posee dos interfaces de comunicación full-duplex, una para establecer conexión con el nodo de la izquierda y otra para comunicarse con el nodo de la derecha.

![node](/images/Node.png)

El algoritmo proporcionado por la cátedra para el enrutamiento de paquetes solo los envía en una dirección, siguiendo el sentido de las agujas del reloj (a través de la Interfaz `0`). Esto implica que no se aprovecha la otra interfaz de comunicación disponible.

Esta limitación conlleva un `retardo` en la entrega de los paquetes:

- Si el nodo de destino se encuentra a la izquierda del nodo emisor, el paquete debe realizar una vuelta completa a la red antes de llegar a su destino.

- Dado que todos los paquetes se envían en el mismo sentido, se crea un cuello de botella en los nodos ubicados a la derecha del nodo emisor, ya que todos los paquetes deben pasar por ellos. Este problema se acentúa cuando múltiples nodos envían paquetes al mismo tiempo.

# Algoritmo de enrutamiento modificado

Se presenta un nuevo algoritmo que tiene como objetivo principal disminuir el retardo de los paquetes. Este algoritmo permite enviar los paquetes en ambos sentidos, aprovechando las dos interfaces de comunicación disponibles en cada nodo.

El nuevo algoritmo utiliza el método de contar saltos (hop count) para determinar la interfaz por la cual enviar cada paquete. Se busca seleccionar la interfaz que requiera la menor cantidad de saltos para llegar al nodo destino, con el fin de reducir el retardo en la entrega de los paquetes.

Además, este algoritmo busca mitigar el problema del cuello de botella que se genera en los nodos ubicados a la derecha del nodo emisor. Al distribuir de manera más equitativa la carga de tráfico, se logra una mejor utilización de los recursos de la red.

### Suposiciones:

- Se asume que la red de topología de anillo es simétrica.

- Se considera que la topología de la red es estática, lo que implica que no hay cambios en la conexión entre los nodos durante el funcionamiento del algoritmo.

### Algoritmo

El algoritmo de enrutamiento modificado sigue los siguientes pasos:

1. Al iniciar la simulación, cada nodo envía un paquete de **reconocimiento** llamado `Hello` al nodo destino establecido en el archivo de configuración `omnetpp.ini`. El paquete se envía por la interfaz `0`.

2. Una vez que el paquete de reconocimiento `Hello` llega al nodo destino, este envía un paquete de **información** llamado `Info` en el mismo sentido que el paquete de reconocimiento. El paquete `Info` se envía por la interfaz `0` al nodo emisor, indicando la cantidad de saltos que ha realizado el paquete de **reconocimiento**.

3. El nodo emisor recibe el paquete de **información** `Info` y determina la interfaz por la cual debe enviar los paquetes. Se elige la interfaz que requiera la menor cantidad de saltos para llegar al nodo destino. Para tomar esta decisión, se compara la cantidad de saltos informada en el paquete `Info` con la mitad de la cantidad de nodos en la red (`NET_HALF_LENGTH`). Si la cantidad de saltos informada es menor a la mitad de la cantidad de nodos, se prefiere la interfaz `0`; de lo contrario, se prefiere la interfaz `1`.

# Análisis de las redes bajo los algoritmos de enrutamiento

Se utiliza una distribución exponencial con un parámetro de 1, lo que significa que los paquetes se generan en promedio cada 1 segundo. Se plantean dos casos de estudio:

## Caso 1: Dos nodos envían paquetes al mismo nodo destino

Los nodos **0** y **2** envían paquetes al nodo **5**.

### Comparación del retardo de los paquetes en ambos algoritmos

gráficos de retardo de paquetes en función del tiempo

### Comparación del tamaño de la cola de paquetes en ambos algoritmos

gráficos de tamaño de la cola de paquetes en función del tiempo

## Caso 2: Todos los nodos envían paquetes al mismo nodo destino

Los nodos **0**, **1**, **2**, **3**, **4**, **5**, **6** y **7** envían paquetes al nodo **5**.

### Comparación del retardo de los paquetes en ambos algoritmos

gráficos de retardo de paquetes en función del tiempo

### Comparación del tamaño de la cola de paquetes en ambos algoritmos

gráficos de tamaño de la cola de paquetes en función del tiempo

# Resultados

Acá comparamos los resultados obtenidos en ambos algoritmos

tablas de escalares

info relevante
