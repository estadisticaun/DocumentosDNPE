# **INSTALACIÓN DEL PAQUETE `UnalR` DESDE LINUX**

## *Instalación de `R`* :computer:

### :round_pushpin: Preliminares

El siguiente laboratorio se desarrolla bajo la distribución de Linux basada en Debian GNU/Linux <span style="color:#F20034; font-weight:bold;">Ubuntu 18.04.6 LTS (cLong Term Support*)</span>.

Para lo cual se usó [Oracle VirtualBox](https://www.virtualbox.org), descargando la imagen ISO de la página oficial de [Ubuntu]([https://releases.ubuntu.com/18.04.6/](https://releases.ubuntu.com/18.04.6/)) y procedemos a instalarla.

Como uno de los requisitos se requiere acceso privilegiado al sistema ya sea como `root` o mediante el comando `sudo` (*si es `root`, remueva de los siguientes comandos `sudo`*).

Verificamos la información del sistema con el que estamos trabajando, mediante el comando:

```
cat /etc/os-release
```

Obteniendo así:

```
    NAME="Ubuntu"
    VERSION="18.04.6 LTS (Bionic Beaver)"
    ID=ubuntu
    ID_LIKE=debian
    PRETTY_NAME="Ubuntu 18.04.6 LTS"
    VERSION_ID="18.04"
    HOME_URL="[https://www.ubuntu.com/](https://www.ubuntu.com/)"
    SUPPORT_URL="[https://help.ubuntu.com/](https://help.ubuntu.com/)"
    BUG_REPORT_URL="[https://bugs.launchpad.net/ubuntu/](https://bugs.launchpad.net/ubuntu/)"
    PRIVACY_POLICY_URL="[https://www.ubuntu.com/legal/terms-and-policies/privacy-policy](https://www.ubuntu.com/legal/terms-and-policies/privacy-policy)"
    VERSION_CODENAME=bionic
    UBUNTU_CODENAME=bionic
```

Hay dos comandos que estaremos ejecutando varias veces durante el laboratorio, los cuales son:

+ `$ apt-get update`: Actualiza la lista de paquetes.
+ `$ apt-get upgrade`: Actualiza el sistema de Linux, incluyendo los paquetes de seguridad.

Por lo tanto, comenzaremos con ellos:

```
sudo apt-get update
sudo apt-get upgrade
```

Ahora ejecutaremos el siguiente chunk de código necesario extraído de la documentación oficial de [R](https://cran.r-project.org/bin/linux/ubuntu/):

```
# Actualizar Índices
sudo apt update -qq
# Instalar dos Paquetes de Ayuda que Necesitamos
sudo apt install --no-install-recommends software-properties-common dirmngr
# Agregue la Clave de Firma para estos Repositorios
wget -qO- https://cloud.r-project.org/bin/linux/ubuntu/marutter_pubkey.asc | sudo tee -a /etc/apt/trusted.gpg.d/cran_ubuntu_key.asc
# Agregue el Repositorio R 4.0 de CRAN
sudo add-apt-repository "deb https://cloud.r-project.org/bin/linux/ubuntu $(lsb_release -cs)-cran40/"
```

Finalmente, estamos listos para ejecutar en sí la instalación:

```
sudo apt-get install r-base
sudo apt-get install r-base-dev
```

Ahora obtendremos el listado de direcciones de +5.000 paquetes del `CRAN`, para agregar el repositorio 'c2d4u' actual de R 4.0 o posterior (*tenga en cuenta que el repositorio 'c2d4u' solo está disponible para versiones LTS*):

```
sudo add-apt-repository ppa:c2d4u.team/c2d4u4.0+
sudo apt install --no-install-recommends r-cran-tidyverse
```

Para realizar el mantenimiento de paquetes en `R`, se pueden actualizar usando `apt-get` ya que los paquetes que forman parte de `r-base` y `r-recommended` de Ubuntu se instalan en el directorio `/usr/lib/R/library`. Por lo que volveremos a ejecutar:

```
sudo apt-get update
sudo apt-get upgrade
```

Para corroborar que todo nos haya quedado correctamente ejecutaremos:

```
sudo -i R
```

Obteniendo así:

```
R version 4.2.2 Patched (2022-11-10 r83330) -- "Innocent and Trusting"
Copyright (C) 2022 The R Foundation for Statistical Computing
Platform: x86_64-pc-linux-gnu (64-bit)

R es un software libre y viene sin GARANTIA ALGUNA.
Usted puede redistribuirlo bajo ciertas circunstancias.
Escriba 'license()' o 'licence()' para detalles de distribucion.

R es un proyecto colaborativo con muchos contribuyentes.
Escriba 'contributors()' para obtener más información y
'citation()' para saber cómo citar R o paquetes de R en publicaciones.

Escriba 'demo()' para demostraciones, 'help()' para el sistema on-line de ayuda,
o 'help.start()' para abrir el sistema de ayuda HTML con su navegador.
Escriba 'q()' para salir de R.
```

___
## :bar_chart: *Instalación del Paquete* :muscle:

Antes de iniciar con la instalación de `UnalR` en sí, es necesario instalar ciertos paquetes/compiladores/librerías, a continuación, se detalla los posibles errores a obtener, de dónde se extrae su solución y las líneas de código a ejecutar para que no nos aparezcan.

Comenzaremos con instalar una librería indispensable para disponer de paquetes desde GitHub.

```
sudo su - -c "R -e \"install.packages('remotes')\""
```

Por defecto nos hacen falta varias herramientas de compilación, como, por ejemplo, el compilador `g++`.

```
ERROR: dependencies 'leaflet', 'leaflet.extras', 'hrbrthemes', 'sf' are not available for package 'UnalR'
```

Para solventarlos es necesario ejecutar las siguientes líneas de código (*soluciones extraídas de [1](https://stackoverflow.com/questions/51146468/installation-of-package-raster-had-non-zero-exit-status) y [2](https://github.com/rspatial/terra/issues/363))*:

```
sudo apt-get install libxml2
sudo apt-get install libxml2-dev
sudo apt-get install build-essential
sudo apt install libgdal* -y

sudo su - -c "R -e \"remotes::install_github('rspatial/terra')\""
```

Ahora, continuando con los issues (*solución extraída de [1](https://stackoverflow.com/questions/66881963/trying-to-load-hrbrthemes-package-but-fail)*):

```
ERROR: dependencies 'hrbrthemes', 'sf' are not available for package 'UnalR'
ERROR: dependency 'gdtools' is not available for package 'hrbrthemes'
```

```
sudo apt install libcairo2-dev
sudo su - -c "R -e \"remotes::install_github('hrbrmstr/hrbrthemes')\""
```

El penúltimo inconveniente a solventar es (*solución extraída de [1](https://community.rstudio.com/t/configuration-failed-for-package-units/76417/4)*):

```
ERROR: configuration failed for package 'units'
```

```
sudo apt install libudunits2-dev
sudo su - -c "R -e \"install.packages('sf')\""
```

:eyes: ***Consideración Importante*** :collision: :

> Durante la ejecución de los comandos, anteriormente mencionados, puede que le aparezca el siguiente error, ¡no se preocupe! la solución es no ejecutar más peticiones de R por 6 horas :watch: (*para instalar dependencias*), es decir, **no use** la consola por 6 horas para instalar paquetes desde GitHub.

> A nosotros nos pasó :sleepy:, por lo que sí ha llegado a esta línea espere 6 horas para ejecutar las siguientes líneas en la guía :hourglass_flowing_sand:.

```
User_Name@ubuntu:~$ sudo usermod -a -G staff User_Name
```

> Si en algún momento obtiene un error por los privilegios de administrador o temas relacionados con ellos, ejecute el anterior comando :point_up: (*a nosotros nos funcionó*) donde `User_Name` es el nombre del usuario con privilegios.

```
Downloading GitHub repo estadisticaun/UnalR@HEAD
Error: Failed to install 'unknown package' from GitHub:
  HTTP error 403.
  API rate limit exceeded for 186.155.47.98. (But here's the good news: Authenticated requests get a higher rate limit. Check out the documentation for more details.)

  Rate limit remaining: 0/60
  Rate limit reset at: 2023-03-08 03:36:53 UTC

  To increase your GitHub API rate limit
  - Use `usethis::create_github_token()` to create a Personal Access Token.
  - Use `usethis::edit_r_environ()` and add the token as `GITHUB_PAT`.
Ejecución interrumpida
```

Una vez esperado el tiempo recomendado para que no le aparezca el error anteriormente mencionado, ejecutaremos por fin la línea que instala el paquete :star: `UnalR` :star:, y si siguió el paso a paso no debería obtener ningún error :tada:.

```
sudo su - -c "R -e \"remotes::install_github('estadisticaun/UnalR')\""
```

> :exclamation: ***OJO*** :warning:: Si le salen mensajes de confirmación siempre escriba por consola `Y/S` ó `1: ALL` en el caso de `R`.

Si todo salió bien debería obtener como últimas líneas las siguientes, que indican que el paquete `UnalR` se ha instalado correctamente :heavy_check_mark:.

```
** testing if installed package keeps a record of temporary installation path
* DONE (UnalR)
```

:white_check_mark: Para poner a prueba ejecute (*obsérvese que se obtienen varios Warnings, ¡no pasa nada! el paquete funciona con ellos, son debidos a una dependencia*):

```
sudo R
library('UnalR')
Registered S3 method overwritten by 'quantmod':
  method            from
  as.zoo.data.frame zoo 
Warning messages:
1: In CPL_gdal_init() :
  GDAL Error 1: libgrass_vector.7.4.0.so: no se puede abrir el archivo del objeto compartido: No existe el archivo o el directorio
2: In CPL_gdal_init() :
  GDAL Error 1: libgrass_vector.7.4.0.so: no se puede abrir el archivo del objeto compartido: No existe el archivo o el directorio
3: In CPL_gdal_init() :
  GDAL Error 1: libgrass_dgl.7.4.0.so: no se puede abrir el archivo del objeto compartido: No existe el archivo o el directorio
4: In CPL_gdal_init() :
  GDAL Error 1: libgrass_dgl.7.4.0.so: no se puede abrir el archivo del objeto compartido: No existe el archivo o el directorio
5: In CPL_gdal_init() :
  GDAL Error 1: libgrass_vector.7.4.0.so: no se puede abrir el archivo del objeto compartido: No existe el archivo o el directorio
6: In CPL_gdal_init() :
  GDAL Error 1: libgrass_vector.7.4.0.so: no se puede abrir el archivo del objeto compartido: No existe el archivo o el directorio
7: In CPL_gdal_init() :
  GDAL Error 1: libgrass_dgl.7.4.0.so: no se puede abrir el archivo del objeto compartido: No existe el archivo o el directorio
8: In CPL_gdal_init() :
  GDAL Error 1: libgrass_dgl.7.4.0.so: no se puede abrir el archivo del objeto compartido: No existe el archivo o el directorio
...
```

Por último, si desea instalar (**NO** *es obligatorio*) uno de los paquetes más usados para la instalación de otros `devtools`, deberá ejecutar (*solución extraída de [1](https://github.com/r-lib/textshaping/issues/21)*):

```
sudo apt install libcurl4-openssl-dev
sudo apt libbz2-dev liblzma-dev
sudo apt-get install libharfbuzz-dev libfribidi-dev
sudo su - -c "R -e \"install.packages('devtools')\""
```

___
## :file_folder: *Instalación de `UnalData`* :books:

> :closed_lock_with_key: Para la instalación de este paquete, es necesario que cuente con el `auth_token`, pues este es un paquete privado y no liberado al público, reemplaza dicho valor cuando más adelante aparezca `YOUR_ACCESS_TOKEN` :key:.

:red_circle: Tenga en cuenta adicionalmente que al tratar de instalar el paquete le puede aparecer el siguiente error:

```
> remotes::install_github("estadisticaun/UnalData", auth_token = "YOUR_ACCESS_TOKEN")
Downloading GitHub repo estadisticaun/UnalData@HEAD
── R CMD build ────────────────────────────────────────────────────────────────
   checking for file '/tmp/RtmpOyprV6/remotes86c3ddc43f2/estadisticaun-UnalData✔  checking for file '/tmp/RtmpOyprV6/remotes86c3ddc43f2/estadisticaun-UnalData-5fdfb68e9e876725599041db37cc6b635caaa20e/DESCRIPTION'
─  preparing 'UnalData':
✔  checking DESCRIPTION meta-information ...
─  checking for LF line-endings in source and make files and shell scripts
─  checking for empty or unneeded directories
─  building 'UnalData_0.0.0.9000.tar.gz'

Installing package into '/home/jeisonalarcon/R/x86_64-pc-linux-gnu-library/4.2'
(as 'lib' is unspecified)
* installing *source* package 'UnalData' ...
** using staged installation
** R
** data
*** moving datasets to lazyload DB
Killed
Warning message:
In i.p(...) :
  installation of package '/tmp/RtmpOyprV6/file86c7ee9c25d/UnalData_0.0.0.9000.tar.gz' had non-zero exit status
```

Cuando aparezca el error sin mayor detalle categorizado como `Killed` es debido a problemas de memoria :anger:. Más específicamente hace pensar que el proceso fue eliminado por un supervisor de proceso (*por ejemplo, un `out-of-memory killer`*), ¿eso quiere decir que el paquete es demasiado pesado? No y sí, es decir, comparado con otros paquetes si es pesado.

* :mag: El siguiente comando analiza el tamaño de algunos de los paquetes más usados y conocidos, ahí vemos que si bien el paquete `UnalData` es el más pesado de los comparados no llega a un tamaño extraordinario en GB.

```
sudo R

  system.file("DESCRIPTION", package = "UnalData") |>
    dirname() |> fs::dir_info(all = TRUE, recurse = TRUE) |>
    getElement("size") |> sum()
  q()
```

Obteniendo como salida del código anterior:

|   Paquete    | Tamaño (MG)  |
|:-----------: |:-----------: |
|   UnalData   |     77.5     |
|    plotly    |     6.45     |
|    UnalR     |     5.18     |
|   ggplot2    |     4.97     |
|   leaflet    |     4.16     |
| highcharter  |     2.98     |
|  tidyverse   |    0.609     |

Por lo tanto, asegúrese de contar con un espacio de memoria normal y una memoria RAM considerable, pues la descompresión de los datos sí consume gran cantidad de recursos. Utilice el siguiente comando de `R` para observar el estado de uso de memoria, núcleos y celdas utilizados en la sesión `gc()`.

:floppy_disk: Todo lo anterior para garantizar que la máquina no tenga limitaciones de memoria y que la instalación del paquete cuente con la RAM necesaria al ser usado :vhs:.

:large_blue_circle: Una vez asegurado que se cumpla con los requisitos mínimos y que no hay restricciones por parte del sistema, otro de los posibles errores que le puede surgir es el siguiente:

```
> remotes::install_github("estadisticaun/UnalData", auth_token = "YOUR_ACCESS_TOKEN")
Downloading GitHub repo estadisticaun/UnalData@HEAD
── R CMD build ────────────────────────────────────────────────────────────────
   checking for file '/tmp/RtmpreVX5T/remotes9b77d7d9a43/estadisticaun-UnalData✔  checking for file '/tmp/RtmpreVX5T/remotes9b77d7d9a43/estadisticaun-UnalData-5fdfb68e9e876725599041db37cc6b635caaa20e/DESCRIPTION'
─  preparing 'UnalData':
✔  checking DESCRIPTION meta-information ...
─  checking for LF line-endings in source and make files and shell scripts
─  checking for empty or unneeded directories
─  building 'UnalData_0.0.0.9000.tar.gz'

Installing package into '/home/jeisonalarcon/R/x86_64-pc-linux-gnu-library/4.2'
(as 'lib' is unspecified)
ERROR: failed to lock directory '/home/jeisonalarcon/R/x86_64-pc-linux-gnu-library/4.2' for modifying
Try removing '/home/jeisonalarcon/R/x86_64-pc-linux-gnu-library/4.2/00LOCK-UnalData'
Warning message:
In i.p(...) :
  installation of package '/tmp/RtmpreVX5T/file9b748bfe18e/UnalData_0.0.0.9000.tar.gz' had non-zero exit status
```

:large_orange_diamond: Tal como lo indica el error es remover la carpeta de `UnalData`, creada temporalmente en una ejecución fallida, usualmente la encontrará en una ruta similar a `/home/jeisonalarcon/R/x86_64-pc-linux-gnu-library/4.2/` (*igualmente el error arroja la ruta que aplica en su caso, no borre solo ese archivo sino TODA la carpeta que lo contiene*) :open_file_folder:.

Finalmente ejecutaremos el comando de instalación, obteniendo el siguiente mensaje que indica que se realizó la descarga de forma correcta :heavy_check_mark:.

```
sudo su - -c "R -e \"remotes::install_github('estadisticaun/UnalData", auth_token = "YOUR_ACCESS_TOKEN')\""


Installing package into '/home/jeisonalarcon/R/x86_64-pc-linux-gnu-library/4.2'
(as 'lib' is unspecified)
* installing *source* package 'UnalData' ...
** using staged installation
** R
** data
*** moving datasets to lazyload DB
** byte-compile and prepare package for lazy loading
** help
*** installing help indices
** building package indices
** testing if installed package can be loaded from temporary location
** testing if installed package can be loaded from final location
** testing if installed package keeps a record of temporary installation path
* DONE (UnalData)
```