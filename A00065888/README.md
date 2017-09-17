### Taller 3
**Universidad ICESI**  
**Curso:** Sistemas Operativos  
**Docente:** Daniel Barragán C.  
**Estudiante:** Nicolas Recalde M.  
**Tema:** Llamadas al sistema (syscalls)  
**Correo:** daniel.barragan at correo.icesi.edu.co


### Objetivos
* Conocer y explicar la función de las llamadas al sistema en un sistema operativo

### Prerrequisitos
* Virtualbox o WMWare
* Máquina virtual con sistema operativo CentOS7
* Aplicativo strace

### Descripción
El tercer taller del curso sistemas operativos trata sobre las llamadas al sistema y su importancia para el sistema operativo

### Actividades

1. Empleando el aplicativo **strace** obtenga 5 llamadas al sistema para uno o varios comandos de linux. Explique por qué los comandos seleccionados emplean las llamadas al sistema encontradas, para ello debe emplear los manuales de Linux en Internet o del sistema operativo (comando **man**). Debe incluir la explicación de los parámetros que reciben las llamadas al sistema encontradas. Consigne capturas de pantalla donde muestre las llamadas al sistema obtenidas (sugerencia: emplear -etrace para filtrar los resultados)

El comando para el cual deseo conocer las llamdas al sistema es 'chmod'
![][1]
* read(int fd, void buf, size_t count): intenta leer hasta contar los bytes desde el buffer *buf* hasta el descriptor de archivo *fd* en el buffer. El sistema emplea esta llamado en chmod para saber cuales son los permisos del archivo actualmente.
![][2]

  
*  write(int fd, const void buf, size_t count): el comando write, escribe hasta el contador de bytes desde el buffer *buf* hasta el contador señalado por *count*; caso de éxito, se devuelve el número de bytes escritos (cero indica que nada fue escrito). En este caso el retorno es 7 bytes. El sistema emplea este llamado en chmod para modificar posteriormente de forma escrita los permisos del archivo.
![][3]  

* open(const char pathname, int flags): dado el nombre de la ruta, open retorna un descriptor de archivo asociado a un integer pequeño no negativo para estas otras llamadas ( Un descriptor de archivo es una referencia a una descripción de archivo abierta; esta referencia no se ve afectada si la ruta de acceso se elimina o modifica para referirse a un archivo distinto). Una llamada a open () crea una nueva descripción de archivo, una entrada en la tabla de archivos abiertos de todo el sistema. La descripción del archivo abierto registra el desplazamiento del archivo y los indicadores de estado del archivo. El sistema emplea este llamado para obtener una forma de abrir el archivo evitando conflictos de ruta de archivo.
![][4]  

* mprotect(void addr, size_t len, int prot): cambia las protecciones de acceso para las páginas de memoria del proceso de llamada que contienen cualquier parte del rango de direcciones en el intervalo [addr, addr + len-1].  *prot* es una combinación de banderas de acceso. Aqui es donde, por medio de esta llamada al sistema, se logra cambiar los permisos del archivo
![][5]


2. Realice la compilación del código fuente adjunto y su ejecución empleando el aplicativo **strace**. Identifique las llamadas al sistema encargadas de enviar y recibir datos a través de la red. A partir de los manuales de Linux en Internet o del sistema operativo explique las llamadas al sistema encontradas y sus parámetros. 

* Descargamos las librerias según la nota al pie del documento y ejecutamos el programa. Mediante el comando strace -c ./curl obtenemos las siguientes llamas al sistema 
![][6]
![][7]
 las llamadas al sistema encargadas de enviar y recibir datos en la red son *sendto* and *recvfrom*
 | Syscall | Parámetros | Descripción | 
|---|---|---|
| recvfrom | **int sockfd** descriptor de archivo, **void buf** mensaje ,**size_t len** tamaño del mensaje, **int flags** tipo de mensaje, **struct sockaddr src_addr, socklen_t addrlen** direccion de origen y tamaño de la misma; | recive el mensaje proveniente de otro socket |
| sendto | **int sockfd** descriptor de archivo,  **const void buff** mensaje, **size_t len** tamaño del mensaje, **int flags** tipo de mensaje, **const struct sockaddr dest_addr ,socklen_t addrlen** direccion de destino y tamaño de la misma | transmite un mensaje a otro socket |

**Nota:** Cuando compile programas tenga en cuenta que estos pueden necesitar la instalación de librerías para su compilación y ejecución.

**Debian**
```
# apt-get install libcurl4-openssl-dev
$ gcc -o curl curl.c -lcurl
```
**CentOS**
```
# yum install libcurl-devel
$ gcc -o curl curl.c -lcurl
```

### Nota

El informe debe ser entregado en formato README.md y debe ser subido a un repositorio de github. El repositorio de github debe ser un fork de https://github.com/ICESI-Training/so-workshop3 y para la entrega deberá hacer un Pull Request (PR) respetando la estructura definida. El código fuente y la url de github deben incluirse en el informe.  

## Referencias

* http://man7.org/linux/man-pages/man2/syscalls.2.html  
* https://jvns.ca/blog/2014/09/18/you-can-be-a-kernel-hacker/
* http://man7.org/linux/man-pages/man2/write.2.html
* http://man7.org/linux/man-pages/man2/read.2.html
* http://man7.org/linux/man-pages/man2/open.2.html 
* http://man7.org/linux/man-pages/man2/mprotect.2.html

[1]: images/chmod.JPG
[2]: images/read.JPG
[3]: images/write.JPG
[4]: images/open.JPG
[5]: images/mprotect.JPG
[6]: images/curl1.JPG
[7]: images/curl2.JPG

