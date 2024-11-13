# Recolección y almacenamiento de evidencias

![Portada](/Imágenes/Portada.jpg)

## Introducción

Se nos pide realizar una adquisición completa (en vivo) de una máquina comprometida Windows 7.

![VirtualBox](/Imágenes/VirtualBox.png)

En cuanto al orden de adquisición, realizaremos en primer lugar una adquisición de memoria RAM, luego llevaremos a cabo un proceso de triaje con el fin de recolectar evidencias digitales clave que posteriormente nos ayudarán a realizar una investigación más profunda, y por último realizaremos una adquisición de disco duro. 

Todos los detalles acerca de estas evidencias quedarán reflejados en las correspondientes actas de adquisición. Además, estableceremos una cadena de custodia en la que registraremos todos los pasos de recolección, incluyendo quién, cuándo y cómo se manipularon las evidencias.

## Acceso a la máquina comprometida
Antes de hacer nada deberemos introducir las credenciales de acceso a la máquina, y nada más entrar al escritorio nos aparecerán unas ventanas emergentes.

![Iniciando la máquina](/Imágenes/Credenciales.png)

![Iniciando la máquina](/Imágenes/inicio.png)

Deberemos cerrar estas ventanas para poder realizar nuestra adquisición.

## Preparación
Para realizar las adquisiciones haremos uso de una serie de herramientas externas que estarán instaladas en un disco duro externo, con el fin de no alterar la máquina comprometida.

Las herramientas utilizadas serán las siguientes:
+ **FTKImager**: Es una herramienta forense digital gratuita desarrollada por AccessData. 
Sus principales características incluyen:
    + Creación de imágenes forenses exactas de dispositivos de almacenamiento como discos duros, memorias USB y tarjetas SD
    + Verificación de la integridad de las imágenes creadas
    + Búsqueda de palabras clave dentro de las imágenes forenses
    + Extracción de archivos específicos de las imágenes
    + Previsualización de archivos y carpetas sin modificar la evidencia original

![FTKImager](/Imágenes/FTK.png)

+ **IRTriage**: Es una herramienta de respuesta en vivo (live response) para Windows que
    + Extrae diversa información del sistema de forma rápida durante una investigación
    + Se utiliza para recopilar evidencias iniciales en modo "live response"
    + Permite obtener datos volátiles y otros artefactos forenses del sistema operativo

![IRTriage](/Imágenes/IRTriage.png)

![Herramientas](/Imágenes/CarpetaHerramientas.png)

## Adquisición de Memoria RAM
Para llevar a cabo esta tarea haremos uso de la herramienta **FTKImager**.

Una vez abramos la herramienta, seleccionaremos la opción **"Capture memory"**.

![Adquisicion RAM](/Imágenes/adquisicionRAM/CaptureMemory.png)

Una vez hecho esto indicaremos la ruta de destino para los archivos generados, el nombre del archivo **.mem** que contendrá la captura de RAM y también marcaremos la casilla **"Include pagefile"** para crear el archivo **.sys**, e iniciamos la captura de memoria.

Al terminar nos saldrá el siguiente mensaje en pantalla.

![Adquisicion RAM](/Imágenes/adquisicionRAM/finished.png)

Por último, para obtener los hashes del archivo **.mem** utilizaremos el comando **"Get-FileHash** en la Powershell.

![Adquisicion RAM](/Imágenes/adquisicionRAM/hashes.png)

### Acta de adquisición 1: Memoria RAM
A continuación se muestra el acta de adquisición en el que se ha realizado la documentación de la adquisición de la memoria RAM.
| Acta de Adquisición Forense |                                                              |
|-----------------------------|--------------------------------------------------------------|
| Fecha y hora                | 2024-11-12 14:42 PM
| Lugar                       | Oficina de Informática Forense, Calle Real 123, 11000 San Fernando, Cádiz        |
| Perito forense              | Santiago Domínguez Gómez               |
| Solicitante                 | Manuel Jesús Rivas Sández |
| **Descripción del dispositivo** |                                                          |
| Tipo de dispositivo         | Ordenador portátil        |
| Marca y modelo              | Lenovo Ideapad 3                                         |
| Número de serie             | PF9XB2309117                                   |
| Capacidad                   | Disco duro: 32 GB, Memoria RAM: 1 GB                      |
| Estado físico               | Sin daños aparentes                |
| **Procedimiento de adquisición** |                                                         |
| Método de adquisición       | Captura de memoria                   |
| Herramientas utilizadas     | FTKImager                     |
| Configuración               | Versión: 4.2.0.13, Formato imagen: mem                 |
| **Integridad de la evidencia** |                                                           |
| Valor hash original         | MD5: 0169AC813609E38D52032CC28FE86818, SHA1: 0CAC934DFF0E449A3FA3D4F01A31CAAB7426E1BB              |
| Algoritmo utilizado         | MD5, SHA1                                          |
| Valor hash de la copia      | MD5: 0169AC813609E38D52032CC28FE86818, SHA1: 0CAC934DFF0E449A3FA3D4F01A31CAAB7426E1BB                |
| **Observaciones**           |  |
| **Firmas**                  |                                                              |
| Perito forense              | Santiago Domínguez Gómez                                            |
| Testigo                     | Carlos Pérez Sagunto                                             |
| Responsable                 | Marina Suárez Sánchez      


## Proceso de Triaje
Para realizar el triaje forense de nuestra máquina comprometida emplearemos la herramienta **IRTriage**.

![Triaje](/Imágenes/triaje/IRTriajeInicio.png)

Al pulsar **OK** nos aparecerá un menú con varias pestañas en el que tendremos que seleccionar los archivos y datos a capturar. Una vez seleccionados clicaremos en **"Run"** para iniciar el proceso. 

![Triaje](/Imágenes/triaje/SeleccionArchivos.png)

![Triaje](/Imágenes/triaje/Haciendotriaje....png)

Los grupos de archivos capturados serán los siguientes:

### Registro de Windows
Este registro contiene información acerca de la configuración del sistema, los programas instalados y las actividades del usuario. Esta información nos servirá para detectar cambios sospechosos en el sistema o malware persistente.

### Logs del sistema
Los logs registran eventos importantes del sistema como errores, inicios de sesión o actividades de aplicaciones que nos serán útiles para reconstruir la línea de tiempo de eventos y detectar actividades anómalas.

### Archivos de paginación e hibernación
Estos archivos pueden contener datos de la memoria RAM que podrían revelar información sobre procesos en ejecución, además de malware que solo existe en memoria.

### Archivos de prefetch
Estos archivos proporcionan información sobre los programas que se han ejecutado en el sistema y cuándo se han ejecutado, lo cual nos será útil para detectar la ejecución de software malicioso.

### Metadatos MFT
El **MFT (Master File Table)** contiene metadatos de todos los archivos del sistema, incluidos timestamps y ubicaciones, que serán vitales para nuestro análisis de evidencias posterior.

### Datos de usuario
Los perfiles de usuario, archivos recientes e historiales de navegación pueden revelar actividades del usuario, posibles vectores de infección y datos comprometidos.

### Información de red
Estos datos ayudan a identificar conexiones sospechosas, posibles comunicaciones con servidores de comando y control, y alteraciones en la configuración de red.

### Procesos y servicios
La lista de procesos en ejecución y servicios activos nos servirá para identificar software malicioso o procesos anómalos que podrían estar causando la corrupción del sistema.

### Archivos de sistema críticos
Estos archivos contienen configuraciones críticas del sistema que podrían haber sido objetivo de modificaciones maliciosas.

Una vez finalizado el proceso de triaje se nos mostrará el siguiente mensaje:

![Triaje](/Imágenes/triaje/finished.png)

Por último obtendremos el hash de la carpeta donde se han almacenado todos los archivos generados. Para ello volveremos a utilizar la Powershell.

![Triaje](/Imágenes/triaje/HashCarpeta.png)

### Acta de adquisición 2: Triaje
A continuación se muestra el acta de adquisición en el que se ha realizado la documentación de la adquisición de disco.
| Acta de Adquisición Forense |                                                              |
|-----------------------------|--------------------------------------------------------------|
| Fecha y hora                | 2024-11-12 15:03 PM
| Lugar                       | Oficina de Informática Forense, Calle Real 123, 11000 San Fernando, Cádiz        |
| Perito forense              | Santiago Domínguez Gómez               |
| Solicitante                 | Manuel Jesús Rivas Sández |
| **Descripción del dispositivo** |                                                          |
| Tipo de dispositivo         | Ordenador portátil        |
| Marca y modelo              | Lenovo Ideapad 3                                         |
| Número de serie             | PF9XB2309117                                   |
| Capacidad                   | Disco duro: 32 GB, Memoria RAM: 1 GB                      |
| Estado físico               | Sin daños aparentes                |
| **Procedimiento de adquisición** |                                                         |
| Método de adquisición       | Extracción directa                 |
| Herramientas utilizadas     | IRTriage                     |
| Configuración               | Versión: 2.16.04.18, Modo de adquisición: Completo, Opciones seleccionadas: Recolección de archivos del sistema, análisis de registros, recopilación de logs del sistema, extracción de archivos temporales               |
| **Integridad de la evidencia** |                                                           |
| Valor hash original         | MD5: 54CE85771BCFD83E2DF465057B9ACB39, SHA1: EB170453A53D9259CCF86AEAD4C2095734907E58             |
| Algoritmo utilizado         | MD5, SHA1                                          |
| Valor hash de la copia      | MD5: 54CE85771BCFD83E2DF465057B9ACB39, SHA1: EB170453A53D9259CCF86AEAD4C2095734907E58                  |
| **Observaciones**           |  |
| **Firmas**                  |                                                              |
| Perito forense              | Santiago Domínguez Gómez                                            |
| Testigo                     | Carlos Pérez Sagunto                                             |
| Responsable                 | Marina Suárez Sánchez 

## Adquisición de Disco
Para esta última tarea volveremos a utilizar **FTKImager**.

Esta vez seleccionaremos la opción **"Create Disk Image"**.

![Adquisicion Disco](/Imágenes/adquisicionDisco/creatediskimage.png)

Luego seleccionaremos **"Logical Drive"**.

![Adquisicion Disco](/Imágenes/adquisicionDisco/logicaldrive.png)

Por último indicaremos el destino de la imagen, el nombre de la imagen y pincharemos en **Start**.

![Adquisicion Disco](/Imágenes/adquisicionDisco/destinoimagen1.png)

![Adquisicion Disco](/Imágenes/adquisicionDisco/destinoimagen2.png)

**Nota**: El formato de los archivos generados será **E01**, ya que es el formato más compatible con la mayoría de programas.

Al finalizar el proceso se nos mostrará una ventana emergente donde se verifica la coincidencia de hashes.

![Adquisicion Disco](/Imágenes/adquisicionDisco/coincidenciahashes(100%).png)

### Acta de adquisición 3: Disco duro
A continuación se muestra el acta de adquisición en el que se ha realizado la documentación de la adquisición de disco.
| Acta de Adquisición Forense |                                                              |
|-----------------------------|--------------------------------------------------------------|
| Fecha y hora                | 2024-11-12 16:24 PM
| Lugar                       | Oficina de Informática Forense, Calle Real 123, 11000 San Fernando, Cádiz        |
| Perito forense              | Santiago Domínguez Gómez               |
| Solicitante                 | Manuel Jesús Rivas Sández |
| **Descripción del dispositivo** |                                                          |
| Tipo de dispositivo         | Ordenador portátil        |
| Marca y modelo              | Lenovo Ideapad 3                                         |
| Número de serie             | PF9XB2309117                                   |
| Capacidad                   | Disco duro: 32 GB, Memoria RAM: 1 GB                      |
| Estado físico               | Sin daños aparentes                |
| **Procedimiento de adquisición** |                                                         |
| Método de adquisición       | Imagen forense                   |
| Herramientas utilizadas     | FTKImager                     |
| Configuración               | Versión: 4.2.0.13, Formato imagen: E01                |
| **Integridad de la evidencia** |                                                           |
| Valor hash original         | MD5: 4e84ca27b01fb68f4860f4935d31c7b2, SHA1: c480d614328e952ece19cdf99cb16d44810908b1              |
| Algoritmo utilizado         | MD5, SHA1                                          |
| Valor hash de la copia      | MD5: 4e84ca27b01fb68f4860f4935d31c7b2, SHA1: c480d614328e952ece19cdf99cb16d44810908b1                  |
| **Observaciones**           |  |
| **Firmas**                  |                                                              |
| Perito forense              | Santiago Domínguez Gómez                                            |
| Testigo                     | Carlos Pérez Sagunto                                             |
| Responsable                 | Marina Suárez Sánchez 

## Cadena de custodia
Finalmente se muestra la cadena de custodia en la que se registran todos los pasos de recolección y donde se incluye quién, cuándo y cómo se manipularon las evidencias.
| Tipo de evidencia  | Fecha y hora de recolección | Recolectado por     | Hash de la evidencia                       | Método de adquisición | Estado de la evidencia |
|------------------- |----------------------------|---------------------|-------------------------------------------|-----------------------|------------------------|
| Memoria RAM    | 2024-11-12 14:42 PM        | Santiago Domínguez Gómez          | MD5: 0169AC813609E38D52032CC28FE86818, SHA1: 0CAC934DFF0E449A3FA3D4F01A31CAAB7426E1BB  |    Captura de memoria     | Sellado y etiquetado    |
| Archivos de triaje       |   2024-11-12 15:03 PM      | Santiago Domínguez Gómez          |  MD5: 54CE85771BCFD83E2DF465057B9ACB39, SHA1: EB170453A53D9259CCF86AEAD4C2095734907E58 |  Extracción directa   | En formato digital    |
| Disco duro (C:) |   2024-11-12 16:24 PM       | Santiago Domínguez Gómez       |  MD5: 4e84ca27b01fb68f4860f4935d31c7b2, SHA1: c480d614328e952ece19cdf99cb16d44810908b1   | Imagen forense     |  Sellado y etiquetado     |
