# Minería de textos - Tema 4 - Custering y clasificación

Dentro del aprendizaje automático, la clasificación (aprendizaje supervisado) y el clustering (aprendizaje no supervisado) son técnicas que se utilizan para resolver numerosas tareas que pretenden extraer conocimiento del contenido textual. Cuando nos referimos a clustering o a clasificación de información textual no estructurada ambas tareas intentan realizar algún tipo de organización lógica de dicha información para facilitar posteriores análisis, o son las técnicas de base para dichos análisis. Ambas tareas tienen como fase inicial la representación del contenido que van a procesar.

Diferencias entre clasificación automática y clustering:
- Clustering (agrupamiento): las clases (clusters, grupos) no se conocen de antemano (aplicación de técnicas no supervisadas)
- Clasificación: las clases están previamente definidas (aplicación de técnicas supervisadas)

## Clusterig

Se parte de un conjunto de objetos (documentos) descritos por un conjunto de variables (rasgos, características). Trata de  organizar el conjunto de objetos en grupos (clusters) tales que los objetos pertenecientes al mismo cluster son más "similares" entre sí de lo que lo son con respecto a los de otros clusters. Las características de los grupos no se conocen a priori, hay que descubrir patrones, de ahí que sea una técnica de aprendizaje no supervisado.

Los objetos del mismo cluster deben ser similares entre si (**cohesión**) y marcadamente diferentes de los de las otras clases (**aislamiento**).

Los mismos documentos se pueden agrupar según diferentes criterios por lo que pueden dar lugar a diferentes agrupaciones (agrupación temática, por extensión, por estructura del contenido, grupos de granularidad alta - pocos grupos -, granularidad baja-muchos grupos con objetos muy cohesionados-…). Es importante seleccionar el conjunto de variables (rasgos) relevantes para el objetivo final del clustering.

El algoritmo recibe como **entrada** los n objetos que se quieren agrupar y dependiendo del algoritmo el valor del nº de clusters o grupos (k).La salida son los k clusters de objetos y/o una organización jerárquica de los objetos. El formato de entrada puede ser:

- Matriz de rasgo-documento (término-documento).

$$MR_{n \times p} | \text{n: nº de objetos (documentos), p: nº variables (rasgos)}$$
$$MR(i,j) \text{ donde valor de la jota-ésima variable del objeto i-ésimo}$$
- Matriz de distancias o similitudes entre objetos.
$$ MD_{n \times n} | \text{n: nº de objetos} $$
$$ MD(i,j) \text{ valor de la diferencia o similitud entre los objetos i y j} $$

En una agrupación temática los textos se suelen **caracterizar** por las palabras que contienen. También podrían utilizarse como rasgos otras unidades lingüísticas como: entidades nombradas, sintagmas,…Para la representación de un documento en esta tarea hace falta: 
- Método para calcular el peso (valor) de cada rasgo.
- Función de similitud o distancia.

Algunos algoritmos requieren unas medidas concretas de similitud o distancia, otros no.

### Tipos

Dependiendo del tipo de agrupamiento:

 - Agrupamiento duro (hard): cada objeto pertenece o no a un clúster, pero solo se asigna auno. También se denomina clustering exclusivo (exclusiveclustering)
 
- Agrupamiento suave(soft): en lugar de colocar cada objeto en un clúster separado, se asigna una probabilidad de que ese punto de datos esté en esos clústeres, o se indica su pertenencia a varios. También se denomina clustering solapado (overlapping clustering).

De acuerdo a la estructura de clusters que se obtiene:
- No jerárquicos: la salida son los n objetos organizados en k grupos sin existir ninguna relación entre los grupos.
- Jerárquicos: los n objetos se presentan organizados en una jerarquía de grupos. Se puede construir la jerarquía de arriba abajo (métodosdivisivos) o de abajo a arriba (métodosaglomerativos).
- Basados en modelos: los datos se modelan utilizando un modelo estadístico estándar para trabajar con diferentes distribuciones. La idea es encontrar el modelo que mejor se adapte a los datos.

Clasificación de los algoritmos:

- Modelos de conectividad: se basan en la noción de que los puntos de datos más cercanos en el espacio de datos presentan más similitudes entre sí que los puntos de datos más lejanos. A este grupo pertenecen los algoritmos jerárquicos. Estos modelos pueden seguir dos enfoques:
    - Enfoque de abajo a arriba: comienzan con la clasificación de todos los puntos de datos en grupos separados y luego los agregan a medida que disminuye la distancia.
    
    - Enfoque de arriba abajo: todos los puntos de datos se clasifican como un solo grupo y luego se dividen a medida que aumenta la distancia.
    
La elección de la función de distancia es subjetiva. Son fáciles de interpretar pero carecen de escalabilidad para manejar grandes con juntos de datos.

- Modelos de centroides: son algoritmos iterativos de agrupación en los que la noción de similitud se deriva de la proximidad de un punto de datos al centroide (elemento representativo) de los clusters. Se debe proporcionar de antemano el número de clusters, lo que hace que sea importante tener un conocimiento previo del conjunto de datos. Estos modelos se ejecutan de forma iterativa para encontrar el óptimo local.

- Modelos distribucionales: se basan en la noción de cuán probable es que todos los puntos de datos del cluster pertenezcan a la misma distribución. Estos modelos pueden sufrir de overfitting (sobreentrenamiento). Ejemplo: algoritmo Expectation-maximization utiliza distribuciones normales multivariantes.

- Modelos basados en densidad: buscan en el espacio de datos áreas de densidad variada. Aísla varias regiones de densidades diferentes y asigna los puntos de datos dentro de estas regiones en el mismo cluster.

#### Algoritmos no jerárquicos

Suponemos conocido el valor de k (nºdeclusters).Trata de encontrar una partición en la que los objetos sean similares a los de su mismo cluster y lo sean menos de los de otros clusters. Requieren definir una medida de competencia de una partición y buscar una partición de objetos que optimice dicha medida. Hay que definir medidas de:
- Su heterogeneidad o falta de cohesión (H(Cr)).
- Su separación del resto de clusters (I(Cr)).

Dadas las medidas de heterogeneidad y separación de cada cluster serán particiones óptimas aquellas que minimizan las medidas de heterogeneidad y maximizan medidas de separación o combinan ambas. El problema de encontrar una partición óptima de n objetos en c clusters puede requerir un alto coste computacional dependiendo del criterio que se utilice. Algunos planteamientos cuando no se conoce el número de clusters son problemas NP-completos.

##### Algoritmos iterativos de reubicación
 
Dado un criterio de competencia de una partición de n objetos en c clusters:

1. Seleccionar C representantes (centroides) generando una partición inicial en c grupos. Existen varios métodos para generar la partición inicial.
2. Para cada documento $D_i$, asignarlo al cluster con el centroide más similar
3. Recalcular todos los centroides
4. Repetir 2 y 3 hasta que no haya modificaciones en la partición en toda una iteración o hasta que se itere un determinado número de veces.

La partición se modifica hasta que ninguna de las posibles transformaciones mejora el criterio de competencia de una partición.

Los algoritmos que involucran la identificación de un centroide para cada cluster reciben el nombre genérico de k-means (k-medias) o c-means.

Un centroide es la posición media de todos los puntos en un cluster: puede ser teórico, si es la posición exacta, o real si es objeto del cluster más próximo al centroide teorico.

#### Algoritmo jerárquicos

La salida del clustering es una jerarquía con forma de árbol de clusters (dendograma). Permite explorar los clusters a diferente niveles de granularidad o de abstracción. En el eje X se representan los objetos, el eje Y corresponde a la distancia/similitud en el espacio de datos, por lo que la altura en el dendograma es significativa.

![DEndograma](https://upload.wikimedia.org/wikipedia/commons/thumb/8/83/Global-Diversity-of-Sponges-%28Porifera%29-pone.0035105.s008.tif/lossy-page1-1280px-Global-Diversity-of-Sponges-%28Porifera%29-pone.0035105.s008.tif.jpg) 

La decisión del número de clusters se puede elegir observando el dendrograma. Puede calcularse como el número de líneas verticales en el dendrograma cortadas por una línea horizontal que puede cruzar la distancia máxima verticalmente sin intersectar una agrupación.


Pueden ser aglomerativos o divisivos. 

##### Aglomerativos

Comienzan con tantos clusters como objetos. En cada paso, los clusters más similares se unen. La unión de clusters continúa hasta que todos los objetos pertenecen a un único cluster. Los algoritmos más utilizados unen pares de clusters dando lugar a árboles binarios. La distancia entre objetos individuales se generaliza a la distancia entre subconjuntos. Esta medidade proximidad se denomina: **métrica de enlace** o de conexión (linkage)

$$d(C_1, C_2) = op(d(x \in C_1, y \in C_2)$$

Las principales métricas de enlace:
- Enlacesimple (singlelink): op=mínimo
- Enlace promedio (averagelink): op=promedio
- Enlace completo(completelink): op=máximo

**Esquema general**:
1. Calcular la matriz de distancias y tratar cada objeto como un cluster
2. Encontrar el par de clusters más similares, los une en uno y actualiza la matriz de distancias para reflejar esta operación.
3. Repetir paso 2 hasta que solo quede 1 cluster.

##### Divisivos

Comienzan con un único cluster que contiene a todos los objetos. En cada paso, se divide un cluster. La división de clusters continúa hasta que todos los clusters contienen un sólo objeto. Los algoritmos más utilizados dividen un cluster en dos dando lugar a árboles binarios.

La agrupación de arriba hacia abajo es conceptualmente más compleja que la agrupación de abajo hacia arriba, ya que ne cesita un segundo algoritmo de agrupación para llevar a cabo las divisiones.

Tiene la ventaja de ser más eficiente si no generamos una jerarquía completa hasta las hojas de documento individuales. Los aglomerativos toman decisiones de agrupación basadas en patrones locales sin tener en cuenta inicialmente la distribución global. Los divisivos disponen de la información completa sobre la distribución global cuando se toman decisiones de partición de alto nivel.

**Esquema general**:
1. Se selecciona un cluster para dividir. Posibilidades:
    - Seleccionar el cluster con más objetos.
    - Seleccionar el cluster que al dividir lo genere una solución que optimice una función criterio.
   
2. Se realiza la partición de dicho cluster normalmente en 2
3. Repetir desde 1 hasta que no queden clusters de más de un elemento.

#### TopicModel (Donde narices meto esto)
 Es un modelo estadístico para descubrirlos "temas" sobre los que versa una colección de documentos y permite realizar clustering. Se asume que si un documento trata sobre un tema habrá palabras que aparecerán con mayor o menor frecuencia según estén relacionadas o no con dicho tema. Como un documento puede tratar varios temas, podrá versar un $X\%$ sobre un tema, un $Y\%$ sobre otro.
 
Dada una colección de documentos, este modelo permite determinar los temas de los que tratan y para cada documento su concreta proporción de dichos temas.

**Latent Dirichlet Allocation*(LDA)[Bleietal.2003] es un modelo estadístico generativo. En LDA cada documento se ve como una mixtura de varios temas y el modelo es capaz de asignárselos. Una palabra puede aparecer en varios temas con diferente probabilidad y con diferentes palabras vecinas. Requiere que el número del grupo s sea conocido.

### Medidas de evaluación de clustering

Hay dos tiposde medidas de evaluación:
- Internas o intrínsecas: se basan en los propios objetos agrupados. Estos métodos suelen asignar mejor puntuación al algoritmo que produce clústeres con alta similitud dentro de un clúster y baja similitud entre clústeres.
- Externas o extrínsecas: se basan en determinar cómo se ajustan los clusters creados por el algoritmo a un conjunto de clusters referencia de evaluación, benchmark o goldstandard. Este gold standard idealmente lo realizan jueces humanos, más de uno, con un buen nivel de acuerdo entre ellos. Entonces podemos calcular un criterio externo que evalúa cómo de bien la agrupación se ajusta a las clases del goldstandard.

#### Internas 
Permiten escoger el mejor algoritmo de clustering a partir de la información de los propios datos, así como el número de clúster óptimo sin ningún tipo de información adicional. Una desventaja de este tipo de evaluación es que las puntuaciones altas en una medida interna no necesariamente resultan en aplicaciones efectivas para el usuario. Esta evaluación está sesgada hacia algoritmos que utilizan el mismo modelo o función de optimización

Ejemplo de medidas:
- Silhouette(https://en.wikipedia.org/wiki/Silhouette_(clustering))
- Índice Davies–Bouldin(https://en.wikipedia.org/wiki/Davies%E2%80%93Bouldin_index)

#### Externas

Las colecciones de referencia o gold estándar son escasas y no cubren todos los campos de aplicación del clustering debido al esfuerzo que supone realizar una agrupación manual.

Algunas medidas son:

- Precisión, cobertura(recall) y medida-F. El cálculo de la precisión y la cobertura se puede realizar de dos maneras diferentes:
    - Microaveraging: los cálculos se basan en la suma de todos los valores individuales
    - Macroaveraging: los cálculos se basan en la suma de los valores por categoría, obteniendo después la media total
- Índice de Rand ajustado (Adjusted Rand index): es una función que mide la similitud de las dos asignaciones, ignorando las permutaciones(https://en.wikipedia.org/wiki/Rand_index).

### Herramientas

Algunas herramientas disponibles para clustering:
- CLUTO (http://glaros.dtc.umn.edu/gkhome/views/cluto/)
- Text-Garden (http://ailab.ijs.si/dunja/textgarden/)
- Weka (https://www.cs.waikato.ac.nz/ml/weka/)
- jLDADMM (https://sourceforge.net/projects/jldadmm/)
- Twitter-LDA (https://github.com/minghui/Twitter-LDA)

## Clasificación

La clasificación automática de documentos es una tarea en la que un documento, o una parte del mismo, se etiqueta como perteneciente a un determinado conjunto, grupo o categoría predeterminada.

Un sistema de clasificación debe asignar un valor booleano T (true) o F (false) para cada par ($d_j$, $c_i$), siendo djun documento de la colección y ci una de las categorías entre las que se desee clasificar $C =\{c_1, c_2, ... , c_j\}$.

La clasificación desde los años 90 utiliza el paradigma del Aprendizaje Automático Supervisado que trata de obtener las características que debe cumplir un documento para que sea clasificado en una u otra categoría, basándose para ello en una colección inicial de documentos preclasificados.

Un **descriptor de clase** contiene las características que debe cumplir un documento para ser clasificado en una determinada clase. Una vez que se han representado los documentos y se han obtenido los descriptores de clase para cada una de las clases consideradas (por medio del proceso de aprendizaje), el sistema utilizará estos descriptores para crear un clasificador, siendo capaz de clasificar nuevos documentos. Se debe disponer de un conjunto de documentos de ejemplo para cada una de las categorías consideradas. Cuanto mayor sea el conjunto de datos etiquetados mayor será la información potencial disponible y, previsiblemente, mejor resultará el aprendizaje.

En el aprendizaje semisupervisado se utilizan datos no etiquetados para
refinar una estimación inicial obtenida a partir de los datos etiquetados con
la categoría correspondiente de los que se disponga.

Las técnicas de aprendizaje supervisado y semisupervisado centran su
aprendizaje en el corpus del que se dispone, que se divide al menos en dos
subcolecciones:
- Subcolección de entrenamiento: documentos utilizados para analizar las características de cada clase (hallar los descriptores de clase) y poder crear el clasificador.
- Subcolección de prueba o test: para comprobar la calidad de los clasificadores generados.

Dentro del aprendizaje semisupervisado se podría ubicar la supervisión
distante (distant supervision). En este tipo de supervisión se dispone de un conjunto de entrenamiento etiquetado generalmente pequeño, y un conjunto más grande de documentos no etiquetados. Se dispone además de un operador (regla, heurística, base de conocimiento, …) que permite etiquetar una muestra de la colección no etiquetada asumiendo que habrá ruido (errores) en el etiquetado. El algoritmo de clasificación con ambos conjuntos etiquetados decide la etiqueta para un nuevo ejemplo.

Aplicaciones de la clasificación automática supervisada en minería de textos:
- Reconocimiento y clasificación de entidades nombradas
- Etiquetado morfosintáctico y sintático
- Desambiguación del sentido de la palabras
- Análisis de sentimientos (identificación de la polaridad)
- Clasificación temática de documentos
- Detección de spam
- Filtrado de documentos

### Tipos

Dependiendo de las categorías:
- single label cuando cada uno de los documentos de la colección debe tener una y sólo una categoría asignada.
- multilabel cuando el número de categorías puede variar desde 0 hasta el número total de clases.

Un caso concreto de clasificación single label es la clasificación binaria, asignando un T o F a cada documento (una aplicación de análisis de polaridad positiva o negativa asignaría T a un documento si es positiva y F en caso de que no lo sea). La clasificación binaria se puede utilizar también en multilabel, asignando un T o F para cada par documento-categoría.

Según la forma de asignar una categoría:
- Hard classification: Se decide directamente si pertenece o no a una categoría  obteniéndose un conjunto no ordenado de los documentos que pertenecen a cada categoría.
- Ranking Classification: Se puede generar un ranking de categorías según la adecuación de cada documento a cada categoría.

Dependiendo del tipo de información utilizada en el proceso de clasificación se distingue entre:
- Fast feature: cuando no se tiene en cuenta información externa al documento
- Full feature: cuando se emplean características adicionales, externas al documento, como puede ser el texto de los enlaces salientes.

### Algoritmos

Hay numerosos algoritmos de clasificación pero vamos a presentar solo los más conocidos.

#### NaïveBayes
Está basado en la teoría de la decisión de Bayes: la teoría de las probabilidades condicionadas. El problema de la clasificación se reduce al cálculo de las probabilidades a posteriori de una clase dado un documento.
$$ P(c_k, d_j) = \frac{P(d_j|c_k)P(c_k)}{P(d_j)}$$

La tarea de un clasificador NaïveBayes es elegir la clase que haga mayor esta probabilidad. Deben estimarse primero otras cantidades como son:
- La probabilidad a priori de cada clase, $P(c_k)$,
- La probabilidad a priori del documento, $P(d_j)$ ;
- La probabilidad condicionada de cada documento a una clase dada, $P(dj|ck)$.

La asunción del principio de independencia entre rasgos implica que la probabilidad de un documento dada una clase sea el producto de las probabilidades condicionada de cada rasgo presente en el documento donde $N_j$ es el número de rasgos presentes en $d_j$.

$$ P(d_j|c_k)=\prod_{i=1}^{N_j}P(t_i, c_k)$$
donde $N_j$ es el número de rasgos presentes en $d_j$.

La diferencia entre dos algoritmos NaïveBayesvendrá por la diferente función que se emplee para la estimación de la probabilidad de un rasgo a una clase: $P(t_i|c_k)$. Algunas de las funciones más usadas en la literatura son las gaussianas y multinomiales.

#### Árboles de decisión
Un árbol de decisión es un árbol cuyos nodos internos representan términos (rasgos), las ramas salientes los pesos de éstos, y las hojas se corresponden con las categorías. Se recorre el árbol de arriba a abajo para cada uno de los documentos, hasta llegar a una de las hojas, es decir, hasta asignar una categoría. En estas estructuras de árbol las hojas representan etiquetas de clase y las ramas representan conjunciones de características que conducen aesas etiquetas de clase. Los árboles de decisión en los que la variable objetivo puede tomar valores continuos (normalmente números reales) se denominan árboles de regresión. Algunos algoritmos para construirlos son ID3(Iterative Dichotomiser3), C4.5(sucesor de ID3), CART(Classification And Regression Tree), Chi-square automatic interaction detection(CHAID), MARS...

#### Máquinas de vectores de soporte (SupportVector Machine - SVM)

Thorstem Joachims utilizó los SVM por primera vez en clasificación automática. Se construyen en espacios de dimensión T, siendo $∣T∣$ el número de términos existentes en nuestra colección. El objetivo es obtener una hipersuperficie que separe el conjunto de ejemplos positivos de los negativos para cada categoría y en ese espacio de dimensión $∣T∣$. Para establecer estas hipersuperficiesde decisión se utiliza una colección de ejemplos de entrenamiento, denominados vectores de soporte. De todas las posibles hipersuperficiesde decisión se escoge aquella que tenga mayor margen sobre las clases.

Las máquinas de vectores de soporte pueden utilizarse también cuando las categorías no son linealmente separables utilizando una **función de kernel** que transforme el espacio $|T|$ dimensional en un espacio con dimensión $|T'| > |T|$ donde el problema sí resulte ser linealmente separable.

#### Basados en redes neuronales artificiales

Son una clase de algoritmos de reconocimiento de patrones que se utilizan comúnmente para problemas de regresión y clasificación. Están formadas por nodos, neuronas, que toman un valor real, lo multiplican por un peso y lo hacen funcionar mediante una función de activación no lineal. Al construir múltiples capas de neuronas, cada una de las cuales recibe parte de las variables de entrada, y luego transmite sus resultados a las capas siguientes, la red puede aprender funciones muy complejas. Teóricamente, una red neuronal es capaz de aprender la forma de cualquier función, dada la suficiente potencia computacional. 

Las redes neuronales son un modelo computacional que consiste en un conjunto de unidades, llamadas neuronas, conectadas entre sí para transmitirse señales. La información de entrada atraviesa la red neuronal, se opera con ella y se producen unos valores de salida. Cada neurona está conectada con otras a través de unos enlaces en los que el valor de salida de la neurona anterior es multiplicado por un valor de peso que pueden incrementar o inhibir el estado de activación de las neuronas adyacentes. A la salida de la neurona, puede existir una función limitadora o umbral, que modifica el valor resultado o impone un límite que se debe sobrepasar antes de propagarse a otra neurona. Esta función se conoce como función de activación.

Teóricamente, una red neuronal es capaz de aprender la forma de cualquier función, dada la suficiente potencia computacional.

#### Basados en aprendizaje profundo (deeplearning)
Los métodos de Aprendizaje Profundo son una actualización moderna de las Redes Neurales Artificiales que explotan la facilidades de computación existentes en la actualidad. Consisten en redes neuronales mucho más grandes y complejas de varias capas. Algoritmos populares de este tipo son:
- ConvolutionalNeural Network (CNN)
- Recurrent Neural Networks (RNNs)
- Long Short-TermMemoryNetworks (LSTMs)
- StackedAuto-Encoders
- Deep BoltzmannMachine (DBM)
- Deep BeliefNetworks (DBN)

#### Transformers

En el año 2017 apareció el modelo Transformer[Polosukhinet al. 2017] basado en redes profundas mucho más potentes y flexibles que las arquitecturas de redes profundas anteriores. Un transformeres básicamente un modelo que recibe un texto y produce otro texto. La aplicación más natural de esta idea es la traducción automática, pero se puede generalizar para utilizarse en muchas otras tareas de PLN. La arquitectura consiste en una secuencia de encoders y decoders, a través de los cuales van fluyendo representaciones intermedias del texto de entrada, hasta producir el texto de salida. Los encoders y decoders son redes neuronales con una capa especial, denominada de auto-atención, que permite determinar qué partes de la frase son las más relevantes y aprender relaciones de contexto entre las palabras.

Gracias a las diferentes capas de auto-atención, el procesamiento del texto se realiza de forma independiente al orden de las palabras, lo que supone un mecanismo muy potente para resolver fenómenos lingüísticos complejos como, por ejemplo, la correferencia. A finales de 2019 Google publicó T5, acrónimo de "Text-to-Text Transfer Transformer", un frameworkbasado en transformerspreparado para adaptar cualquier tarea de PLN y resolverla desde la perspectiva text-to-textde los transformers. Además de la traducción automática (que de forma natural es text-to-text) pueden beneficiarse de la potencia de estos modelos pre-entrenados casi cualquier tarea de procesamiento de textos, incluidas las basadas en clasificación.

Bert (Bidirectional Encoder Representations from Transformers) y GPT(Generative Pre-trained Transformer) son sistemasprentrenadosbasadosentransformerscon corpora enormesque se puedenajustara tareasespecíficasde PLN. Los modelos GPT-n se encuentran basados en la arquitectura de transformers: GPT-2 (Radforset al. 2019); GPR-3 (Brown et al. 2020).

### Evaluación

Se compara la salida del sistema con un conjunto de referencia o gold standard. A través de una tabla de contingencia se pueden definir las siguientes funciones de evaluación para todo sistema de clasificación. TP (verdaderos positivos); TN (verdaderos negativos); FP (falsos positivos): FN (falsos negativos)

La exactitud –accuracy-(Â) y el error (Ê):
$$Â = \frac{TP+TN}{TP+TN+FP+FN}$$
$$Ê = 1-Â$$


También se utilizan la Precisión, Cobertura y medida-F