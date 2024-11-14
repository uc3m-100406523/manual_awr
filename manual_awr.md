Guía de uso de AWR Design Environment
=====================================

# Orden de implementación de un circuito

Los parámetros físicos del circuito son definidos a priori por el usuario mediante un "esquemático" (*circuit schematics*). En él, se define el tipo de sustrato a emplear y las dimensiones físicas del diseño, tales como longitudes, anchuras y grosores de las líneas, así como la forma global del circuito o la disposición de las interconexiones entre los distintos componentes. Por ello es necesario, antes de crear el esquemático dentro del entorno de Microwave Office, realizar los cálculos previos que permiten conocer las dimensiones del circuito a partir de las impedancias deseadas de las líneas, la frecuencia de trabajo y el tipo de sustrato.

# Elementos de la interfaz

Los grupos de elementos más importantes del interfaz principal son:

- **Menús**: Menús desplegables donde aparecen todas las opciones y comandos. Tenemos:
	- *File*
	- *Edit*
	- *View*
	- *Project*
	- *Simulate*
	- *Options*
	- *Tools*
	- *Scripts*
	- *Window*
	- *Help*

	Pero destacamos el explorador de variables (`View->Variable Browser`), el cual, permite un acceso rápido a los valores de todos los parámetros del circuito definidos por el usuario (dimensiones de las líneas, parámetros eléctricos del sustrato, etc.) y la activación de las opciones *Tune* y *Optimize*, que se estudiarán más adelante.

- **Barra de herramientas**: Fila de botones donde se encuentran los comandos de uso más frecuente. Entre otros, por ejemplo, el botón en forma de "rayo" para ejecutar la simulación una vez se hayan definido todos los elementos necesarios.

- **Espacio de trabajo**: Área principal de la ventana, en la que se visualizan los diferentes elementos activos sobre los que se trabaja en cada instante (esquemáticos, gráficas, etc.).

- **Ventanas**: Lengüetas situadas en la parte inferior izquierda de la ventana, que permiten elegir el tipo de explorador, el cual, se visualizará en forma de columna en el área izquierda del interfaz. Los exploradores son:

	- **Explorador de proyecto (*Project*)**: Es el explorador que aparece por defecto. En esta columna se encuentran todos los datos y componentes que forman el proyecto. En particular, interesan las siguientes carpetas para comenzar:
		- *Project Options*: Para definir las frecuencias de simulación, las unidades globales y la impedancia de referencia, entre otras cosas.
		- *Circuit Schematics*: Página de diseño en la que se realiza el esquemático o implementación de los elementos que componen el circuito a simular.
		- *Graphs*: Carpeta de la que van a colgar los gráficos que se definirán cuando se desee simular el circuito.
		- *Optimizer Goals*: Para definir los objetivos del optimizador.

	- **Explorador de elementos (*Elements*)**: Despliega los elementos definidos en el entorno de trabajo del programa Microwave Office para implementar los circuitos, tanto en tecnologías stripline, microstrip y coplanar como a partir de elementos concentrados o bloques con matrices de parámetros. También aquí se encuentran definidos los diferentes tipos de sustratos. Cuando se accede a una categoría del explorador de elementos, aparece una lista de elementos junto con una breve descripción de los mismos. Si esta breve descripción no aparece, se puede pinchar con el botón derecho sobre cualquiera de los elementos y seleccionar en el menú desplegable la opción `Details`. Así, aparecerán todos los elementos en forma de listado, con una breve descripción a su lado. Para ver una descripción más detallada de un elemento en concreto, junto con el significado de cada uno de los parámetros eléctricos del mismo, se puede pinchar igualmente con el botón derecho sobre el icono de interés y seleccionar en el menú desplegable la opción `Element Help...` o pulsar el botón `Element Help` de la tabla del elemento (se verá más adelante). Esto abrirá la ventana de ayuda del programa en la página dedicada al elemento seleccionado.

	- **Explorador de diseño (*Layout*)**: Muestra las opciones para diseñar y representar el layout del circuito.

# Creación del circuito

En primer lugar abrimos un proyecto en blanco. Cada vez que se ejecuta el programa, se abre un proyecto en blanco, pero si estamos en uno y queremos abrir otro:
```
File->New Project
```
Se puede guardar el proyecto en cualquier momento, sea cual sea el estado en el que se encuentre su desarrollo. Para hacerlo:
- En el sistema de ficheros asignado: `File->Save Project`
- Con un nuevo nombre: `File->Save Project As...`

Los proyectos Microwave Office se guardan con la extensión `.emp`. Se puede recuperar cualquier proyecto guardado con anterioridad con:
```
File->Open Project...
```
Una vez abierto un proyecto en blanco, los pasos básicos recomendados para implementar un circuito o esquemático son los descritos en los siguientes puntos.

## Definición de los puntos de frecuencia y unidades globales: Project Options

Al seleccionar la opción `Options->Project Options...` ó, en el explorador de proyecto, al hacer doble click con el ratón sobre la opción `Project->Project Options`, aparece un cuadro de diálogo con diversas pestañas.

Con este cuadro de diálogo, y dentro de la pestaña `Frequencies`, se puede definir el barrido de frecuencias para la simulación posterior. Este conjunto de frecuencias definirá, por tanto, el eje de abcisas en los gráficos resultantes de las simulaciones. Para ello se define la frecuencia inicial (`Start`), la frecuencia final (`Stop`), y el salto entre puntos de frecuencia consecutivos (`Step`) o se añaden frecuencias específicas seleccionando `Single Point`. Una vez se han indicado las frecuencias a las que trabajar, se selecciona `Add` (para añadir nuevas frecuencias con las que experimentar) ó `Replace` (para usar solo las frecuencias que acabamos de indicar) y pulsamos el botón `Apply`, que actualizará la lista de frecuencias de prueba (`Current Range`). Si se selecciona la opción `Delete` y se pulsa `Apply`, la lista de frecuencias se borrará.

En ese mismo cuadro de diálogo, pero dentro de la pestaña `Global Units`, se pueden definir las unidades que, por defecto, aparecerán, por ejemplo, en los cuadros de diálogo que definen las dimensiones de cada elemento del esquemático. Para cambiarlas, pulsa el botón `Edit Units` y, en la nueva ventana que aparezca, en la sección `General->Units`, hay una tabla en la que se pueden modificar estas medidas. Especialmente hay que tener en cuenta que las dimensiones relativas a longitudes aparecen, por defecto, en micrómetros, por lo que puede ser conveniente cambiarlas a milímetros.

## Construcción del esquemático

El esquemático define el sustrato del circuito, los parámetros físicos o dimensiones del mismo (longitudes, anchuras y grosores de las líneas), los elementos que lo componen y los puntos de interconexión entre ellos. Un mismo proyecto puede tener varios esquemáticos, todos los cuales, a medida que vayan siendo creados, colgarán de `Project->Circuit Schematics` en el explorador de proyecto. Su construcción se puede descomponer en los siguientes puntos:

1. Creación del esquemático.

	Para realizar un esquemático, es necesario primero crear uno nuevo y darle nombre. Para ello, hacemos click derecho en `Project->Circuit Schematics`. En el menú desplegable que aparece hay que seleccionar la opción `New Schematic...`. Entonces aparece un cuadro de diálogo en el cual hay que introducir el nombre con el que identificar al circuito que vamos a crear. Otra opción es hacer click derecho en `Project`, a continuación, seleccionar `Add New->Schematic...` en el menú desplegable que aparece y darle nombre.

	Una vez que el esquemático ha sido creado, aparece en el espacio de trabajo una ventana con puntos que definen una rejilla rectangular. Sobre esta rejilla se define el esquemático, es decir, aquí se van a ir añadiendo y conectando los distintos elementos del circuito.

2. Definición del sustrato

	El primer elemento del circuito que es necesario definir es el sustrato, aunque si vamos a trabajar con solo elementos concentrados no sería necesario.

	Una vez hemos creado un nuevo esquemático, hay que ir al `Elements->Circuit Elements->Substrates`. En la parte inferior izquierda de la pantalla aparecen, entonces, varios tipos de sustratos en diferentes tecnologías.

	Se recomienda utilizar el sustrato más sencillo posible en cada caso.

	Una vez seleccionado el sustrato que mejor se ajusta a nuestro diseño y necesidades, basta con pinchar sobre él y arrastrar con el ratón el elemento hasta situarlo en el espacio de trabajo o parte central de la pantalla, donde está la ventana con la página de diseño del esquemático que se acaba de generar. El sustrato se puede situar en cualquier parte del espacio de trabajo.

	Al hacer doble click sobre el icono del elemento, una vez esté situado sobre el esquemático, o seleccionar dicho icono y pinchar con el botón derecho del ratón sobre él y seleccionar `Properties...` en el menú desplegable, aparecerá en pantalla una tabla con todos los parámetros y sus valores asignados, a la que nos referiremos como "tabla del esquemático".

	El último paso es dar valores a los parámetros eléctricos del sustrato. Una vez situado el icono del sustrato en la página de diseño del esquemático dentro del espacio de trabajo, aparece sobre este icono una lista de parámetros (Er, Rho, ...) con unos valores numéricos asignados. Cada sustrato, así como cada elemento, tiene definidos unos valores por defecto para todos sus parámetros eléctricos y geométricos, pero será necesario redefinir la mayoría de ellos para adecuarlos a las especificaciones de cada diseño particular. Para ello, se puede hacer doble click sobre cada uno de los valores numéricos y rellenar el campo correspondiente de forma directa, sobre el espacio de trabajo. Si el esquemático fuera demasiado pequeño, se recomienda utilizar la lupa (situada en la barra de herramientas) o el atajo de teclado asignado (Control + Rueda del ratón) para ayudarse a visualizar mejor los valores. También se puede usar la tabla del esquemático, donde los valores pueden cambiarse pinchando con el ratón sobre cada uno de ellos.

	En dicha tabla también se pueden seleccionar aquellos parámetros elegidos para el ajuste manual (columna Tune), o para la optimización automática (columna Optimize), con posibilidad de elegir los límites superior e inferior para la optimización, como se explica más adelante.

	Los valores que hay que definir normalmente son:
	- La constante dieléctrica. La constante dieléctrica normalizada no es necesario redefinirla normalmente.
	- La tangente de pérdidas.
	- El grosor del material dieléctrico que compone el sustrato.
	- El grosor de la línea metálica. La anchura de la línea metálica se define posteriormente para cada elemento o tramo de línea que se vaya a incorporar al esquemático, por lo que no se puede definir en este punto todavía.
	- La resistividad de la línea metálica. Hay que tener cuidado al definir el valor de la resistividad del metal, porque se pide el valor de la resistividad normalizada respecto a la resistividad del oro, cuyo valor es $2.35 · 10^{-8} \text{ W/m}$.
	
	No es necesario renombrar el sustrato.

	Una vez definidos los parámetros del sustrato, en el explorador de variables aparece un listado con los valores numéricos correspondientes. Desde este listado también se pueden modificar estos valores en cualquier momento. En él también están incluidas las opciones `Tune` y `Optimize`.

3. Añadir elementos

	Una vez definido el sustrato con los valores eléctricos y geométricos de acuerdo a las especificaciones de diseño, ya se pueden añadir al esquemático los diferentes elementos y tramos de línea que componen el circuito, e interconectarlos entre sí.

	Este proceso es muy similar al seguido para definir el sustrato. En el explorador de elementos se selecciona primero el grupo de componentes de interés. Normalmente los grupos de interés, a parte de los sustratos, suelen ser *microstrip* o *stripline*, según se esté empleando una u otra tecnología. Cada uno de estos grupos posee un submenú que se despliega automáticamente al hacer doble click con el ratón sobre cada uno de ellos, o al pinchar en la cruz que aparece a su lado. Dentro del submenú se elige el subgrupo al cual corresponda el elemento que se desee insertar, y entonces aparece en la parte inferior izquierda del interfaz el conjunto de todos los elementos pertenecientes al subgrupo elegido.

	Se recomienda emplear, a priori, el elemento más sencillo de entre todos los posibles, o aquel que requiera definir un menor número de parámetros, siempre que cumpla los requerimientos de diseño.

	Una vez se ha elegido el elemento que se va a insertar en el circuito, se pincha sobre él y se arrastra hasta situarlo en la página de diseño del esquemático, dentro del espacio de trabajo, igual que se hizo cuando se incluyó el sustrato.

	A continuación, se definen los parámetros físicos o dimensiones del elemento, de manera idéntica, de nuevo, a como se hizo al definir los parámetros del sustrato. Según el tipo de componente, será necesario definir unos parámetros u otros. Las dimensiones básicas a definir son casi siempre la longitud de la línea metálica y la anchura de la misma. El grosor de la línea, así como las características del metal y del dieléctrico están ya definidas en el sustrato, por lo que no forman parte de los parámetros del elemento propiamente dicho. De hecho, cada elemento lleva asociado un sustrato (el tipo de sustrato sí es, por tanto, uno más de los parámetros a definir para cada componente). No obstante, si se ha implementado un único sustrato en el esquemático, éste será el que, por defecto, lleven asociados todos los elementos que se incorporen al circuito y no es necesario especificarlo para cada componente. Si se han definido varios sustratos, habrá que indicar sobre cuál de los sustratos definidos irá montado el elemento. Tampoco es necesario renombrar a cada elemento. Todos los parámetros definidos aparecen en el explorador de variables.

	Para interconectar los tramos de línea y elementos entre sí, basta con acercar los extremos de cada elemento lo suficiente, el programa se encarga de conectarlos directamente. Los extremos de cada elemento están marcados con cruces si el elemento está sin conectar. Al conectarlos entre sí, las cruces se vuelven un cuadrado de color verde.

	Para realizar un mismo circuito, el esquemático puede realizarse mediante diversas soluciones geométricas y utilizando diferentes tipos de elementos. Se recomienda sintetizar aquella forma que más se adecúe a la realidad física del circuito. Por ejemplo, se recomienda incluir las líneas a las que luego se van a conectar los puertos físicos de entrada y salida del circuito.

## Empleo de la herramienta *TXLine*

Para calcular las longitudes físicas y los grosores de las líneas según la impedancia y longitud eléctrica deseadas de cada tramo, dados un tipo de sustrato, una conductividad del metal y una frecuenta determinados, existen en la literatura diversas fórmulas empíricas aproximadas, así como tablas, que pueden emplearse en una primera aproximación antes de abordar la realización del esquemático. No obstante, el software de AWR incluye una herramienta llamada TXLine que puede ayudar a obtener los parámetros físicos de diseño a partir de los parámetros eléctricos.

Se accede a la herramienta TXLine en:
```
Tools->TXLine...
```

Tiene varias pestañas (parte superior) para hacer los cálculos según se trate de una línea microstrip, una línea stripline, un slotline, diversos tipos de líneas acopladas, etc.

En este tipo de "calculadora" hay que introducir como dato las características del dieléctrico empleado (constante dieléctrica y tangente de pérdidas), y la conductividad del metal. Estos parámetros, que deben ser conocidos a priori, se introducen en el cuadro de diálogo dispuesto a tal fin en la parte superior del interfaz TXLine (Material Parameters).

A continuación, se introducen, en la parte izquierda de la interfaz (Electrical Characteristics) las características eléctricas de la línea, tales como impedancia característica, frecuencia de trabajo, longitud eléctrica, etc. y, si se desea, el grosor del dieléctrico y el grosor de la tira metálica en la parte derecha de la interfaz (*Physical Characteristics*). Entonces, se obtienen la longitud física y la anchura necesaria para obtener las características eléctricas deseadas.

En muchos casos, también pueden introducirse los parámetros físicos y obtener los parámetros eléctricos equivalentes, es decir, también permite realizar los cálculos inversos, excepto en el caso de líneas acopladas. En cualquier caso, los parámetros calculados para unos datos de entrada determinados, aparecen subrayados en negro.

# Simulación del circuito

Una vez diseñado el esquemático con todos los parámetros definidos, y una vez implementado el circuito mediante la unión en el esquemático de todos los elementos que lo componen, ya se puede pasar a realizar una simulación del mismo. Para ello, es necesario cubrir los siguientes puntos:

## 1) Creación de los puertos

Hay que situar los puertos de entrada/salida en aquellos puntos del circuito que deseemos utilizar para medir la respuesta del mismo. Para añadir estos puertos, igual que se hizo para incluir el sustrato y cualquier otro elemento del circuito, basta con ir al explorador de elementos, seleccionar el conjunto de elementos `Circuit Elements->Ports`, y arrastrar hasta el esquemático el puerto más adecuado, conectándolo al punto deseado. Se recomienda utilizar el tipo de puerto simple `Circuit Elements->Ports->PORT`.

Los parámetros de los puertos (nombre, número e impedancia de referencia) no es necesario modificarlos a priori, salvo que la impedancia de referencia sea distinta de $50 \Omega$. Sí es conveniente tomar nota del número que el programa asigna por defecto a cada uno de los puertos. Así, por ejemplo, al especificar que se desea medir el parámetro *S12*, se medirá el parámetro que realmente se desea medir, entre los puertos adecuados. Por otro lado, siempre se puede modificar el número del puerto según la convención que al usuario le parezca más lógica o intuitiva para evitar ese tipo de errores en la simulación.

No es necesario emplear un puerto que permita variar la señal de excitación a la entrada. El programa realizará la simulación con la única condición de indicarle claramente en el gráfico la medida que especifique el parámetro a medir. Basta, como se ha dicho, con seleccionar el puerto simple `Circuit Elements->Ports->PORT`.

## 2) Definición de gráficos

Una vez definidos los puertos, se pueden crear tantos gráficos como se desee. En el explorador de proyecto, se pincha con el botón derecho sobre la opción `Project->Graph` y, a continuación, se selecciona la opción `New Graph...`. Entonces aparece un cuadro de diálogo en el cual hay que introducir un nombre para el gráfico que vamos a crear, y un tipo de representación (polar, rectangular, carta de Smith, etc). Normalmente, para gráficos convencionales, y en una primera toma de contacto, interesa la representación rectangular, salvo que se indique expresamente lo contrario.

Al crear un gráfico nuevo, éste colgará del árbol `Project->Graph` en el explorador de proyecto. Colgarán tantos gráficos como creemos, y en cada gráfico podemos incluir tantas medidas como se necesiten, superponiéndose unas a otras. Cada una de las medidas también colgará, a su vez, del gráfico correspondiente en el explorador de proyecto. Los gráficos como tal aparecen en la parte central de la pantalla, o espacio de trabajo.

Una vez creado el gráfico, y antes de ejecutar la simulación, hay que definir qué medidas deseamos que aparezcan en dicho gráfico. Para ello, una vez definido el gráfico, se pincha sobre él con el botón derecho del ratón y, en el menú desplegable que aparece, se selecciona la opción `Add Measurement...`. Entonces aparece un cuadro de diálogo en el que hay que especificar:
- El esquemático del que se desea extraer la medida. Se especifica en el campo `Data Source Name`. Por defecto, en dicho campo aparece el valor `All Sources`, pero hay que cambiar esta opción por el nombre del esquemático creado.
- El tipo de medida. Se especifica en el campo `Measurement Type`. Para un circuito es `Linear->Port Parameters`.
- El formato de la medida. Se especifica en el campo `Complex Modifier`, y se puede indicar una de las siguientes opciones:
	- Magnitud (`Mag.`)
	- Fase (`Angle`)
	- En parte real (`Real`)
	- En parte imaginaria (`Imag.`)
También se puede indicar si de desea en dB seleccionando la casilla `dB`.
- Los parámetros que se desean medir (S, Y, Z, ABCD, etc.). Se selecciona en el campo `Measurement` con `Linear->Port Parameters` seleccionado.
- Los puertos involucrados en la medida. Se especifican en los campos `To Port Index` y `From Port Index`.

Se podrán incluir tantas medidas como se deseen en un mismo gráfico. Para ello, una vez realizadas las acciones anteriores, se puede volver a pinchar con el botón derecho del ratón sobre el mismo gráfico y volver a seleccionar `Add Measurement...`, volviendo a repetir el proceso tantas veces como se desee o como medidas se quieran incluir en una misma gráfica. Si dos medidas distintas no deben aparecer en el mismo gráfico, puede crearse un nuevo gráfico.

## 3) Simulación

Una vez creado un gráfico y definida la medida que deseamos aparezca en él, para obtener dicha medida, es decir, simular el circuito con los parámetros definidos anteriormente, basta con ejecutar la simulación apretando el botón en forma de rayo que aparece en los menús (parte superior de la pantalla) o seleccionando `Simulate->Analyze`.

En el espacio de trabajo se genera un sistema de ventanas con el esquemático y tantos gráficos como se hayan creado.

Los puntos de frecuencia pueden modificarse tantas veces como se desee y volver a ejecutar la simulación. A su vez, los parámetros del circuito, tanto del sustrato como de los elementos que lo componen, también pueden modificarse tantas veces como sea necesario, y actualizar a continuación las simulaciones.

Pueden ser de mucha utilidad las herramientas *Tune* para el ajuste manual, y *Optimize* para la optimización automática a la hora de modificar los parámetros del circuito, como se verá a continuación.

# Optimización del circuito

## Optimización manual (*Tune*)

La herramienta tune permite observar el efecto que tiene la variación de uno o más parámetros del circuito sobre la respuesta de éste. Su uso comprende los siguientes puntos:

1. Selección de los parámetros a ajustar

	El primer paso es seleccionar aquellos parámetros que de deseen variar o sintonizar. Estos parámetros pueden ser cualesquiera de entre todos aquellos susceptibles de ser definidos o modificados dentro de cada elemento del esquemático. Normalmente, suelen escogerse longitudes y anchuras de las líneas, pero también podemos escoger la conductividad del metal o la constante dieléctrica definidas en el sustrato. Hay varias opciones para seleccionarlos:
	- Con la página de diseño del esquemático activa en el espacio de trabajo, se puede seleccionar la herramienta pinchando en el icono con forma de destornillador en la barra de herramientas. También se puede acceder a ella en `Simulate->Tune Tool`. Si pinchamos, dentro del esquemático, sobre aquel o aquellos valores de los parámetros que deseemos variar, éstos cambian a color azul. Eso indica que ya pueden ser incluidos en la herramienta *Tune*. Para deseleccionarlos, basta con volver a sobre ellos con la herramienta *Tune* seleccionada.
	- En el explorador de variables, aparecen con sus valores todas las variables de diseño, o parámetros definidos por el usuario para cada uno de los elementos del circuito. Pinchando, al lado de cada elemento que desee sintonizarse, en la columna `Tune`, se seleccionan los parámetros a incluir en la herramienta *Tune*, de la misma forma que con el destornillador de la barra de herramientas.
	- En la tabla del esquemático también se puede elegir, pinchando la columna `Tune`, aquellos parámetros a sintonizar.

2. Ajuste o sintonía mediante *Tune*

	Una vez seleccionados los parámetros a ajustar, se selecciona el icono `Tune` en la barra de herramientas, situado al lado del icono en forma de destornillador o se accede con `Simulate->Tune`. A continuación, aparece una interfaz con tantos mandos o ajustes variables como parámetros se hayan elegido para el ajuste.

	Si debajo de la interfaz de *Tune* aparece una ventana con una de las gráficas de simulación, se puede observar de forma directa cómo se modifica la gráfica a medida que variamos mediante los mandos de ajuste de la herramienta los valores de los parámetros. Esto puede ser de gran utilidad para conocer cómo afecta, cualitativa y cuantitativamente, la variación de los parámetros físicos del circuito a la respuesta del mismo.

	Cada vez que se desee, se puede guardar un conjunto de valores, con la opción `Save` dentro de la interfaz, y seguir modificando los valores a partir de ese punto.

	Cada estado guardado recibe un nombre y, para volver a uno de los estados anteriores guardados, incluido el inicial, existe la opción `Restore` dentro del mismo interfaz.

	Para que sean más sencillas las comparaciones, se puede congelar un estado de la gráfica con la opción `Freeze`, que mantiene en pantalla los valores de los gráficos para un conjunto de valores dados, y seguir variando los valores de los parámetros mientras se modifica la gráfica, pero superponiéndose a los valores de la gráfica que existían en el estado en el que se ejecutó `Freeze`. Esta herramienta es extremadamente útil para conocer cómo varía la respuesta de un circuito respecto a cada uno de los parámetros que entran en juego, y cuál es su sensibilidad respecto a las variaciones de cada uno de ellos.

## Optimización automática (*Optimize*)

Esta herramienta permite, una vez establecido un objetivo a alcanzar en lo que a la respuesta del circuito se refiere, optimizar las variables de diseño seleccionadas (longitudes, grosores de las líneas, etc.) para mejorar la respuesta del dispositivo, es decir, para alcanzar los objetivos de diseño hasta donde sea posible.

Algunos valores, en teoría, podrían ser obtenidos en las simulaciones si todos los cálculos respecto a los grosores y longitudes de las líneas fueran exactos y el comportamiento de los dispositivos fuera ideal. Sin embargo, se verá que, en una primera fase de diseño, normalmente no se alcanzan los objetivos planteados. Ello es debido a la no idealidad de los componentes como las pérdidas y otros fenómenos y efectos parásitos que no se tienen en cuenta en los cálculos previos y que el programa sí es capaz de simular.

A partir del diseño inicial, llevado a cabo según se ha descrito en los apartados precedentes, se puede llevar a cabo una optimización siguiendo los siguientes pasos:

1. Selección de los parámetros a optimizar

	El primer paso es seleccionar aquellos parámetros que de deseen variar en el proceso de optimización, dando un valor máximo y un valor mínimo, que será el rango de variación dentro del cual se le permite al optimizador variar estos parámetros. Estos parámetros pueden ser cualesquiera de entre todos aquellos susceptibles de ser definidos o modificados dentro de cada elemento del esquemático. Normalmente, suelen escogerse longitudes y anchuras de las líneas, igual que en el caso de la optimización manual. Hay varias opciones para seleccionarlos:
	- En el explorador de variables, aparecen con sus valores todas las variables de diseño, o parámetros definidos por el usuario para cada uno de los elementos del circuito. Pinchando, al lado de cada elemento que desee optimizarse, en la columna `Optimize`, se seleccionan los parámetros a incluir en la herramienta de optimización, de la misma forma que con la herramienta de optimización automática.
	- En la tabla del esquemático también se puede elegir, pinchando la columna `Optimize`, aquellos parámetros a optimizar.

2. Objetivos del optimizador

	Una vez seleccionados los parámetros, en el explorador de proyecto, se pincha con el botón derecho del ratón en la opción `Project->Optimizer Goals` y se selecciona `Add Optimizer Goal...` en el menú desplegable. Aparece entonces un cuadro de diálogo.

	Hay que especificar qué parámetro de los medidos se desea optimizar. Para ello, se pulsa el botón `New/Edit Meas...`, que abre otro cuadro de diálogo que permite seleccionar el parámetro.

	Hay que dar el valor del parámetro que se desea alcanzar en torno a un punto de frecuencia en concreto o dentro de un rango de frecuencias completo. Esto se hace en la sección `Range`.

	También se puede especificar que el objetivo a alcanzar no sea exacto, sino que la medida debe quedar por encima o por debajo de dicho valor. Esto se hace en la sección `Goal Type`.

3. Optimización

	Se selecciona la opción `Simulate->Optimize`. Se selecciona un número de iteraciones en el campo `Maximum Iterations` y se comienza la optimización pulsando el botón `Start`.

	El optimizador concluirá cuando se haya alcanzado el objetivo del optimizador o cuando se haya sobrepasado el número de iteraciones. Este proceso puede tardar unos minutos.

	Los valores finales del optimizador pasan directamente a los parámetros del esquemático, y la gráfica se actualiza automáticamente.

# Librería de AWR

Se accede en Elementos circuitales (`Elements->Circuit Elements`), donde se pueden ver varias categorías de elementos.

- `Coplanar`: Elementos en tecnología coplanar.
	- `Bends`: Codos.
	- `Components`: Componentes.
	- `Coupled Lines`: Líneas acopladas.
	- `Junctions`: Uniones.
	- `Lines`: Tramos de líneas.
	- `Other`: Otros elementos.
	- `_Obsolete`: Modelos obsoletos.
- `General`: Elementos básicos para la construcción de circuitos.
	- `Active`: Elementos activos.
	- `Filters`: Filtros.
	- `Lumped Elements`: Elementos concentrados convencionales.
		- `Coupled Inductor`: Bobinas acopladas.
			- `MUCN`: N bobinas mutuamente acompladas (forma cerrada).
	- `Network Blocks`: Bloques de parámetros.
	- `Passive`: Elementos pasivos.
		- `Other`: Otros elementos.
			- `IMPED`: Impedancia compleja, las partes real e imaginaria son especificadas.
			- `OST`: Equivalente a `TLOC`, para *stubs* en serie.
			- `SST`: Equivalente a `TLSC`, para *stubs* en serie.
	- `_Obsolete`: Modelos obsoletos.
- `Interconnects`
	- `GND`: Conexión a tierra.
- `Lumped Elements`: Elementos concentrados convencionales.
	- `Capacitor`: Condensadores.
		- `CAP`: Condensador (forma cerrada).
		- `CAPQ`: Condensador con carga (forma cerrada).
		- `CAPQP`: Condensador con capacitancia dependiente de la frecuencia y con pérdidas.
	- `Coupled Inductor`: Bobinas acopladas.
		- `MUC2`: Dos bobinas mutuamente acompladas (forma cerrada).
		- `MUC3`: Tres bobinas mutuamente acompladas (forma cerrada).
		- `MUC4`: Cuatro bobinas mutuamente acompladas (forma cerrada).
		- `MUC5`: Cinco bobinas mutuamente acompladas (forma cerrada).
		- `MUC6`: Seis bobinas mutuamente acompladas (forma cerrada).
		- `MUC7`: Siete bobinas mutuamente acompladas (forma cerrada).
		- `MUC8`: Ocho bobinas mutuamente acompladas (forma cerrada).
		- `MUC9`: Nueve bobinas mutuamente acompladas (forma cerrada).
		- `MUC10`: 10 bobinas mutuamente acompladas (forma cerrada).
	- `Inductor`: Bobinas.
	- `Inverter`
	- `Package`
	- `Resistor`: Resistencias.
		- `LOAD`: Resistencia conectada a tierra (forma cerrada).
		- `RES`: Resistencia (forma cerrada).
		- `REST`: Resistencia con temperatura de ruido (forma cerrada).
	- `Resonators`: Resonadores/Osciladores.
		- `CRYSTAL`: Resonador de cristal (forma cerrada).
		- `PLC`: Resonador LC en paralelo (forma cerrada).
		- `PLCQ`: Resonador LC en paralelo con carga (forma cerrada).
		- `PRLC`: Resonador RLC en paralelo (forma cerrada).
		- `SLC`: Resonador LC en serie (forma cerrada).
		- `SLCQ`: Resonador LC en serie con carga (forma cerrada).
		- `SRLC`: Resonador RLC en serie (forma cerrada).
	- `_Obsolete`: Modelos obsoletos.
- `MeasDevice`: Herramientas de medida.
	- `I_METER`: Amperímetro.
	- `P_METER3`: Potenciómetro de tres puertos.
	- `V_METER`: Voltímetro.
	- `V_NSMTR`: Voltímetro de ruido.
	- `Controls`
	- `Envelope`
	- `IV`
	- `Probes`: Sondeo.
		- `V_PROBE`: Sonda de voltaje.
- `Microstrip`: Elementos en tecnología microstrip.
	- `Bends`: Codos.
	- `Components`: Componentes.
	- `Coupled Lines`: Líneas acopladas.
	- `Junctions`: Uniones.
	- `Lines`: Tramos de líneas.
		- `MLIN`: Se recomienda para simular un tramo simple de línea microstrip siempre que no existan otras especificaciones al respecto o el diseño del circuito no lo exija.
		- `MTAPER`: Se emplea para interconectar líneas de diferentes grosores.
	- `Other`: Otros elementos.
	- `PwrDivider`: Divisores de potencia.
	- `_Obsolete`: Modelos obsoletos.
- `Sources`: Generadores.
	- `AC`: Fuentes de corriente alterna.
	- `DC`: Fuentes de corriente continua.
	- `Dependent`: Fuentes dependientes.
	- `Noise`: Generadores ruido.
	- `Signals (Tone 1)`: Generadores de señales.
	- `Signals (Tone 2)`: Generadores de señales.
- `Ports`: Puertos para la simulación.
	- `PORT`: Puerto simple.
- `Stripline`: Elementos en tecnología stripline.
	- `Bends`: Codos.
	- `Components`: Componentes.
	- `Coupled Lines`: Líneas acopladas.
	- `Junctions`: Uniones.
	- `Lines`: Tramos de líneas.
		- `SLIN`: Se recomienda para simular un tramo de línea stripline simple.
		- `STAPER`: Se emplea para interconectar líneas de diferentes grosores.
	- `Other`: Otros elementos.
	- `_Obsolete`: Modelos obsoletos.
- `Substrates`: Distintos tipos de sustratos disponibles en la librería de *Microwave Office*.
	- `CPW_SUB`: Sustrato coplanar simple.
	- `MSUB`: Sustrato microstrip simple.
	- `MSUB2`: Sustrato microstrip de dos capas.
	- `MSUB4`: Sustrato microstrip de cuatro capas.
	- `SSUB`: Sustrato stripline simple (balanceado).
	- `SSUB2`: Sustrato stripline no balanceado. Permite especificar diferente grosor para la capa superior e inferior del stripline.
	- `SSUB4`: Sustrato stripline de cuatro capas.
	- `SSUBL`: Sustrato stripline de dos capas. Permite especificar distintas constantes dieléctricas para cada una de las dos capas.
	- `SSUBT`: Sustrato stripline de tres capas.
	- `_Obsolete`: Modelos obsoletos.
- `Transmission Lines`: Líneas de transmisión.
	- `Phase`
		- `TLIN`: Línea de transmisión ideal y sin pérdidas, donde la impedancia característica y la longitud eléctrica (en grados) son especificadas (la longitud a una cierta frecuencia, que también es especificada). Solo hay un terminal en cada extremo ya que los otros se consideran comunes (conectados a tierra).
		- `TLIN4`: Parecido a `TLIN`, pero con cuatro terminaciones, apto para conexiones en serie.
		- `TLOC`: Sección de línea de transmisión cargada con un circuito abierto (stub conectada en paralelo), tiene los mismos parámetros que las anteriores.
		- `TLOC4`: Idéntica a `TLOC`, pero con cuatro terminales.
		- `TLSC`: Sección de línea de transmisión cargada con un cortocircuito (stub conectada en paralelo), tiene los mismos parámetros que las anteriores.
		- `TLSC4`: Idéntica a `TLSC`, pero con cuatro terminales.
- `Libraries`
	- `* AWR web site*`
		- `APLAC`
			- `APLAC`
				- `Switch_AP`: Interruptor de cuatro nodos.