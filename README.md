# :mag: Proyectos y Archivos Varios :open_file_folder:

El siguiente repositorio tiene como fin consolidar aquellos proyectos que se encuentran en una etapa inicial, almacenar aquellos archivos varios, instructivos de instalación y demás documentación útil y de valor.

## :computer: Shiny Server :outbox_tray:

Acá encontrará unos slides en donde se explica a groso modo el proceso de instalación desde 0 de Shiny Server usando una máquina virtual (`VirtualBox-6.1.36`) y el SO `CentOS-7-Minimal`. Dentro de las diapositivas hallará todos los enlaces, comandos y documentación necesaria para logra dicha instalación.

___

## :dart: Web Scraping :arrows_counterclockwise:

### :wrench: Instalación :battery:

Para la correcta ejecución del [Jupyter Notebook]() que contiene las clases para cada una de las páginas (*Easy, Home Center, Mercado Libre, AliExpress*) a las que se realiza el rastreo se debe contar con [Python]( https://www.python.org/downloads/) (*preferiblemente una versión* `>= 3.8`) y algún administrador de paquetes como [conda](https://docs.conda.io/en/latest/miniconda.html).

Una vez se cuenta con ello, basta con instalar los paquetes usados en el proyecto, teniendo presente siempre apuntar al archivo de `requirements.txt` (*en la ruta en donde dejo el proyecto clonado*). Siguiendo un esqueleto como el siguiente:

``` py
# conda install -c conda-forge --file requirements.txt
conda install pip
pip install -r requirements.txt
```

### :pencil2: Uso y Consideraciones :calendar:

Tenga en cuenta que cada clase goza de ciertas peculiaridades (*pues cada página web es diferente*), por lo cual considere que:

- **WebScrapingEasy()**
  - :clipboard: `path` es el parámetro con el que se buscará los productos en la categoría a la que pertenece, omita el prefijo `Inicio >`, conserve los espacios tal como quedan al pegar el texto directamente, y tenga presente que no es sensible a mayúsculas o minúsculas (*no se preocupe por eso* :sunglasses:).

- **WebScrapingHomeCenter()**
  - :arrow_up_small: Siga las mismas indicaciones/recomendaciones que arriba, pero tenga presente que para Home Center la categoría **NO** define la URL, por lo tanto, se es necesario de un archivo con las rutas (`CategoriesList_HomeCenter.pkl`), sin él (*o lo sí lo mueve de ubicación*) no se ejecutará el raspado web.
  - :eyes: Este pendiente de la consola, quizás se esté esperando una respuesta (*introducir algún valor*), no tiene necesidad de modificar manualmente el archivo con rutas, la clase se preocupa por todos los posibles escenarios :muscle:.

- **WebScrapingMercadoLibre()**
  - :ok: Acá (*a diferencia de los 2 casos anteriores*) basta con introducir el nombre del producto (*el cual no es sensible a mayúsculas o minúsculas*).
  - :top: Se requiere de un nuevo parámetro llamado `limit` para especificar el número de productos a rastrear (*filas a retornar*), esto con el fin de que usted se quede con los `n` primeros y no gastar tanto tiempo de ejecución (*no recorrer todas las páginas*).
  - :books: El parámetro `infoExtra` acepta un valor booleano (`True` o `False`), esto por si desea retornar información adicional del producto (*categoría a la que pertenece, rating, unidades vendidas y si cuenta con envío gratis*). :warning: Ojo :exclamation: recuerde que al habilitarlo tomará mucho más tiempo la ejecución, ya que ahora el script tendrá que ingresar a cada URL de cada producto.

- **WebScrapingAliExpress()**
  - :round_pushpin: Al igual que para Mercado Libre, use el parámetro `limit` pues son demasiados los productos que se encuentran con una sola consulta. Son productos no páginas a indagar, tenga presente eso.