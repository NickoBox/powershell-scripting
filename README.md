# powershell-scripting
## Un repositorio con una introducción a scripting en Powershell

### 1. Verificar permisos de ejecución
El primer paso para empezar a crear scripts para powershell es verificar si la ejecucion de scripts esta habilitada en nuestro sistema, usualmente esta restringida por medidas de seguridad, si queremos conocer su estado podemos ejecutar el siguiente comando:

`Get-ExecutionPolicy`

Este comando nos devolvera el estado de la politica de ejecucion, para poder empezar a usar nuestros propios scripts el valor de esta propiedad debe ser `Unrestricted`, si en cambio este comando nos devuelve `Restricted`, debemos usar el siguiente comando para quitar esta restricción:

`Set-ExecutionPolicy Unrestricted`

Despues de esto podemos verificar su valor con `Get-ExecutionPolicy`
Ahora si podemos empezar a hacer scripts para ejecutarlos dentro de Powershell.

### 2. Comandos (CmdLets)
En PowerShell, los comandos reciben el nombre de `cmdlet`, y se puede utilizar la tecla de tabulación para que Powershell autocomplete el nombre del cmdlet que se quiere usar.

Windows PowerShell fue creado teniendo en cuenta su compatibilidad con versiones anteriores, por lo que es un recurso que funciona bien con los mismos comandos que utiliza el CMD. Sabiendo esto, se pueden utilizar los mismos comandos que se usaban en el Símbolo del Sistema, pero en una interfaz más avanzada y con muchos más comandos.

A continuacion, en este documento, hay una lista de cmdlets muy utiles y su sintaxis como tambien la funcion de cada uno.

Si quisieramos ver todos los comandos disponibles dentro de Powershell podemos usar el comando `Show-Command`, esto nos dara una lista extensa de todos los comandos utilizables en una ventana por fuera de la consola.
Por ahora vamos con los básicos.

#### Get-Command
En caso de que quieras conocer todos los cmdlets que ofrece PowerShell y verlos en consola, lo podrás hacer escribiendo este comando.

Windows PowerShell permite, a través de este comando, conocer todas las funciones y características que incluyen sus cmdlets, presentados en forma de lista en la que se describen las funciones de cada uno, así como sus parámetros y opciones especiales.

Escribiendo en la consola `Get-Command -Name <nombre>` obtenemos un conjunto de comandos que incluyen ese nombre en específico.
Si no recuerdas su nombre especifico, puedes usar asteriscos como comodines para buscar un comando que incluya cierta palabra, por ejemplo: 
`Get-Command -Name *Get*`
Con lo que podrías ver una lista de los cmdlets que incluyen el término `Get` en su nombre.

#### Get-Host
Con la ejecución de este comando se obtiene la versión de Windows PowerShell que está usando el sistema.

#### Get-History
Con este comando se obtiene un historial de todos los comandos que se ejecutaron bajo una sesión de PowerShell y que actualmente se encuentran ejecutándose.

#### Get-Random
Ejecutando este comando se obtiene un número aleatorio entre `0` y `2.147.483.646`.

#### Get-Service
En ciertas ocasiones, será necesario saber qué servicios se instalaron en el sistema, para lo que se puede usar el comando `Get-Service`, que brindará información acerca de los servicios que se están ejecutando y los que ya fueron detenidos.

Para usar este cmdlet, hay que ingresar `Get-Service` en la consola, usando al mismo tiempo alguna de las parámetros adicionales, en una sintaxis similar al siguiente ejemplo:

`Get-Service | Where-Object {$_.Status -eq "Running"}`

Como powershell es Orientado a Objetos podemos ejecutar la sintaxis de arriba y filtrar los servicios que tengan un estado "Running" (o corriendo) en el sistema en este momento.
Si usamos `Get-Service` sin ningun parametro, se presentará una lista de todos los servicios con sus respectivos estados (`"Running"` o `"Stopped"`)

#### Get-Help
Si estas empezando en Powershell este comando será tu mejor amigo.
Este `cmdlet` muestra la sintaxis y la funcion de cualquier `cmdlet` que uses como parametro.

Sintaxis:
`Get-Help <cmdlet>`

Si es la primera vez que lo utilizas Powershell te preguntara si quieres ejecutar primero Update-Help, ya que intentará de darte la ayuda actualizada.

#### Get-Date
Para saber de una forma rápida qué día fue en una determinada fecha del pasado, usando este comando se obtendrá el día exacto. Por ejemplo, para saber qué día fue el 11 de octubre de 2002, habría que escribir en Powershell:

`Get-Date 11.10.2002`

El formato de la fecha debe ser `dd.mm.aaaa`

Si ejecutamos Get-Date sin parametros nos devuelve la fecha y horas actuales

`Get-Date`
`Wednesday, June 15, 2021 4:06:13 PM`
#### Copy-Item
Con este comando se pueden copiar carpetas o archivos.

Este comando es muy similar a `cp` de CMD pero con ciertas mejoras.

Sintaxis:
`Copy-Item <ruta\nombreArchivo> -Destination <ruta\nuevoNombreArchivo>`
Este `cmdlet puede copiar y modificar el nombre de elementos usando el mismo comando, con el que se puede establecer un nuevo nombre para dicho en el parametro.

Ejemplo:

`Copy-Item "C:\Desarrollo\web.html -Destination C:\Produccion\index.html"`
#### Invoke-Command
En el momento en que quieras ejecutar un script o un comando PowerShell (de forma local o remota, en uno o varios ordenadores), `Invoke-Command` va a ser tu mejor opción. Es simple de utilizar y te ayudará a gestionar ordenadores por lotes.

Sintaxis:
`Invoke-Command <script o cmdlet>`

Este comando tiene muchas opciones para mas informacion usa `Get-Help Invoke-Command`

#### Invoke-Expression
Con Invoke-Expression se ejecuta otra expresión o comando. Si te encuentras ingresando una cadena de entrada o una expresión, en primer lugar este comando la va a analizar y a continuación la ejecutará. Sin este comando, la cadena no devuelve ninguna acción. Invoke-Expression solo trabaja a nivel local, a diferencia de Invoke-Command.

Sintaxis
`Invoke-Expression <expresion o cmdlet>`

Para usar este comando, se debe escribir `Invoke-Expression` junto con una expresión o comando. Por ejemplo, se podría fijar una variable `$Command` con una orden que señale el cmdlet `Get-Process`. Mediante la ejecución del comando `Invoke-Expression $Command`, `Get-Process` va a actuar del mismo modo que un cmdlet en el equipo local.

Del mismo modo, se puede ejecutar una función en un script con el uso de una variable, lo que resulta muy útil si se trabaja con scripts dinámicos.

#### Invoke-WebRequest
Este cmdlet es muy similar a una herramienta de transferencia de datos llamada `cURL`, se puede hacer un inicio de sesión, un scraping y la descarga de información relacionada a servicios y páginas web, mientras se trabaja desde la interfaz de PowerShell haciendo el monitoreo de algún sitio web del que se desee obtener esta información.

Para llevar a cabo estas tareas, hay que utilizarlo como `Invoke-WebRequest` junto a sus parámetros. Con esto, es posible conseguir los enlaces que tiene un sitio web específico con la siguiente sintaxis de ejemplo:

Sintaxis:
`Invoke-WebRequest -Uri http://dominio.com/`

para descargar archivos:
`Invoke-WebRequest -Uri http://dominio.com/ -OutFile C:\ImagenDescargada.jpg`

Ejemplo:
`(Invoke-WebRequest –Uri ‘https://wwww.google.com’).Links`

En el ejemplo mostrado se obtendrian todos los enlaces de la pagina de google.

#### Set-ExecutionPolicy

Este `cmdlet` nos permite modificar el valor de las politicas de ejecucion de scripts(con extension .ps1)
desde PowerShell

Al principio de este documento usamos este `cmdlet` para quitar las restricciones de seguridad.

Las politicas de seguridad tienen cuatro opciones:

`Restricted`
`All Signed`
`Remote Signed`
`Unrestricted`

Por ejemplo, si queremos establecer el nivel de seguridad a restringida otravez, tendríamos que usar:

`Set-ExecutionPolicy -ExecutionPolicy Restricted`
#### Get-Item
En caso de que estés buscando información acerca de un elemento con una ubicación concreta, como podría ser un directorio en el disco duro, el comando `Get-Item` resulta el indicado para esta tarea.

Sintaxis:
`Get-Item <ruta\archivo-o-directorio>`

Ejemplo:
`Get-Item "C:\Desarrollo\"`

Este comando no obtiene el contenido del elemento (subdirectorios y/o archivos) a menos que lo solicites de forma explícita

#### Remove-Item
En caso de que desees borrar elementos como carpetas, archivos, funciones y variables y claves del registro, Remove-Item será el mejor cmdlet.

Lo importante es que ofrece parámetros para introducir y expulsar elementos.

Con el cmdlet Remove-Item puedes remover elementos de localizaciones específicas con el uso de ciertos parámetros.

Sintaxis:
`Remove-Item <ruta\archivo>`

Ejemplo:
`Remove-Item "C:\Desarrollo\web.html"`
#### Get-Content
Este cmdlet puede examinar lo que contiene un archivo y mostrarlo en la consola sin necesidad de abrirlo.

Por ejemplo, es posible mostrar en pantalla las primeras 20 líneas incluidas en un archivo `index.html`, para lo que puedes escribir:

`Get-Content "C:\Produccion\index.html" -TotalCount 20`

Este cmdlet es similar al cmdlet Get-Item anterior, pero con el cual podemos obtener lo que incluye el archivo que has indicado.
Si ejecutas este comando para un archivo de extensión txt, te revelará íntegramente el texto que incluye dicho archivo pero si lo utilizas en un archivo de imagen png, vas a obtener gran cantidad de datos binarios ilegibles y sin sentido.

Es util para leer archivos de texto plano

Usar Get-Content sin parametros no ofrece mucha utilidad. Pero combinado con otros cmdlets puede ser una herramienta muy útil.

#### Set-Content
Con este cmdlet es posible almacenar texto en un archivo, algo parecido a lo que se puede hacer con «echo» en el Bash. Si se usa en combinación con el cmdlet Get-Content, se puede ver primero qué es lo que contiene un determinado archivo para posteriormente hacer la copia a otro archivo a través de Set-Content.

Por ejemplo, se puede usar el cmdlet Set-Content para añadir o sustituir lo que contiene un archivo por otro contenido. Por último, se puede combinar con el comando antes mencionado para guardarlo con un nuevo nombre (ejemplo.txt) de la siguiente manera:

Ejemplo:
`Get-Content "C:\Produccion\index.html" -TotalCount 20 | Set-Content "primerasLineas.txt"`

#### Get-Variable
Este cmdlet obtiene las variables de Windows PowerShell en la consola actual. Puede recuperar los valores de las variables especificando el parámetro ValueOnly y filtrar las variables devueltas por nombre.

Para utilizarlo solo debes escribir `Get-Variable` acompañado de sus parámetros y demás opciones. Por ejemplo, si te gustaría conocer el valor de la variable `variableDeEjemplo` escribe lo siguiente:

Ejemplo:
`Get-Variable -Name "variableDeEjemplo"`

#### Set-Variable
El valor de una variable puede ser establecido, modificado o reinicializado con este `cmdlet`. Para fijar el valor de la variable del caso anterior, habría que escribir lo siguiente:

Ejemplo:

`Set-Variable -Name "variableDeEjemplo" -Value "Nuevo valor"`
#### Get-Process
A menudo, utilizamos el Administrador de Tareas con el fin de descubrir exactamente qué procesos se están ejecutando en nuestro PC. En PowerShell, cualquier usuario puede saber esto ejecutando este `cmdlet`, con el que obtendrá la lista de procesos activos en ese momento.

El cmdlet `Get-Process` tiene cierta semejanza con `Get-Service`, aunque en este caso ofrece información acerca de los procesos.

#### Start-Process
Con este `cmdlet`, Windows PowerShell hace que sea mucho más fácil ejecutar procesos en el equipo.

Ejemplo:
Para abrir la calculadora de forma rapida y simple.

`Start-Process calc`

#### Stop-Process
Con este cmdlet puedes detener un proceso, ya sea que haya sido iniciado por ti o por otro usuario.

Siguiendo con el ejemplo de la Calculadora, si deseas interrumpir íntegramente sus procesos en ejecución, escribe lo indicado abajo en PowerShell:

`Stop-Process -Name "Calculator"`

#### Start-Service
Si necesitas comenzar un servicio en el PC, el cmdlet `Start-Service` es el indicado en este caso, sirviendo de igual modo aunque dicho servicio esté deshabilitado en el PC.

Para iniciar el servicio Windows Search, se usa esta sintaxis:

`Start-Service -Name "WSearch"`

#### Stop-Service

Con este comando detienes los servicios que se encuentran en ejecución en el equipo.

`Stop-Service -Name "Wsearch"`
Con esta orden detendrás el servicio «Windows Search».

#### Get-Location
El cmdlet `Get-Location` es similar al comando `pwd` del shell BASH.

Se usa sin parametros

Con este `cmdlet` puedes obtener la ubicacion del directorio deonde estas trabajando actualmente
El cmdlet `Get-Location` obtiene un objeto que representa el directorio actual, al igual que el comando pwd (imprimir directorio de trabajo).

Al moverse entre unidades de Windows PowerShell, Windows PowerShell conserva la ubicación en cada unidad. Puede utilizar `Get-Location` para buscar la ubicación en cada unidad.

También puede utilizar `Get-Location` para obtener el directorio actual en tiempo de ejecución y utilizarlo en funciones y scripts, como en una función que muestra el directorio actual en el símbolo del sistema de Windows PowerShell.

`Get-Location`


#### Set-location

El cmdlet `Set-Location` establece la ubicación de trabajo en la ubicación especificada. Esa ubicación puede ser un directorio, un subdirectorio, una ubicación del Registro u otra pila de ubicaciones.

Sintaxis:

`Set-Location <ruta>`


#### Exit

Puedes salir de PowerShell utilizando el comando `Exit`.