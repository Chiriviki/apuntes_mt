# Minería de textos - Tema 3 - Representación de documentos.


Para que los algoritmos de minería de textos puedan procesar un texto se deben extraer sus características o rasgos. El contenido textual se puede representar de diferentes formas (cadenas de caracteres, palabras, estructuras sintácticas, grafos de relaciones, predicados, etc.) . a representación de un texto deberá ser fiel a su contenido, incluyendo la información necesaria para poder extraer el conocimiento útil que se espera obtener. Además, deberá ser adecuada a las especificaciones de los algoritmos que se empleen a continuación. Diferentes tareas de la minería de textos y diferentes tipos de textos pueden requerir diferentes representaciones. En [Hothoet al 2005] se presentan los principios de la minería de textos y se detallan los modelos de representación de textos más habituales

## Modelos de representación

Los métodos de representación más utilizados son:

- Modelodel espacio vectorial(Vector Space Model –VSM-)
- Modelo probabilístico(Probabilistic Topic Model)
- Modelo estadístico(Statistical Language Model)
- Modelos semánticos vectoriales (Vector SemanticModels)

Nos centraremos en el modelo del espacio vectorial y en el semántico vectorial pero se darán nociones del resto de modelos

### Espacio vectorial

Un modelo de representación vectorial de documentos se puede definir como una cuádrupla $<X, B, \mu, F>$ donde:

- Vocabulario X: conjunto de cadenas posibles.
- Álgebra B: generaliza las relaciones entre los objetos
- Función de medida o métrica $\mu$: establece la distancia (o similitud) entre objetos
- Función de pesado F: establece la proyección de cada objeto del espacio sobre la recta real

Los documentos se modelan como conjuntos de rasgos (cadenas de caracteres: tokens, palabras, n-gramas, …) que pueden ser individualmente tratados y pesados. Los documentos pasan a ser representados como vectores dentro de un espacio euclídeo, de forma que midiendo la distancia entre dos vectores se tratará de estimar su similitud como indicador de cercanía semántica. Se pueden aplicar diferentes métricas $\mu$ para el cálculo de la distancia: distancia euclídea y coseno son las más populares pero hay muchas otras.

Asume el **"principio de independencia"** entre rasgos: las cadenas (rasgos) aparecidas en un mismo texto no tienen relación entre sí y se pueden cuantificar individualmente. No tiene en cuenta el orden en el que aparecen los rasgos en el texto: la semántica de un documento queda reducida a la suma de los significados de los rasgos que contiene. Aunque estas suposiciones no son correctas desde el punto de vista lingüístico, reducen la complejidad sin penalizar en muchos casos de forma sustancial los resultados.

Un documento quedará representado como una combinación lineal de vectores base $t_i$ donde cada coeficiente $t_{ij}$ de la combinación representará la relevancia de cada rasgo en el contenido del documento, calculada con una función de pesado $F$.

Así, definir una representación dentro del VSM se reduce aencontrar una función de asignación de pesos $F: f(t_i) = t_{ij}$ para calcular el valor de cada componente del vector $d_j$. Dos representaciones de un mismo documento $di(d_i y d'_i)$ a partir del mismo vocabulario serán diferentes siempre que el conjunto de valores $t_{ij}$ que tomen las componentes de los vectores $d_i$ y $d'_i$ sean diferentes; es decir,

$$ d_j \neq d'_j \Leftrightarrow \{i| t_{ij} \neq t'_{ij}, \forall i \} \neq \emptyset $$

**Bolsa de palabras** (*Bag of Words, BoW*) es uno de los métodos más básicos de representarun documento. En la representación BoW se representa un documento utilizando un vector con tantas componentes como palabras/términos haya en el vocabulario y el valor de la componente se calcula utilizando una función de pesado.

#### Colecciones de documentos

Una colección de documentos se suele representar mediante una **matriz término-documento**, enla que cada fila representa una palabra en el vocabulario de la colección y cada columna representa un documento de la colección.

Este tipode matrices se utilizaron primeramente para determinar la similitud entre documentos en Recuperación de Información(Information Retrieval): dos documentos similares tendrán palabras similares y eso quedará patente en sus vectores que también serán similares.

Los tamaños de los vocabularios en colecciones extensas pueden ser de cientos de miles y el número de documentos también puede llegar a ser muy alto, lo que da lugara matrices dispersas (con muchos ceros).

#### Limitaciones

- **Alta dimensionalidad de la representación**: suele dar como resultado vectores muy largos y dispersos en colecciones extensas (contienen principalmente ceros) ya que muchas las palabras nunca ocurren en el contexto de otras. Las funciones de selección y reducción de rasgos ayudan a reducir la dimensionalidad.
- **Pérdida de correlación con las palabras (términos, rasgos) adyacentes**: algunas propuestas utilizan para representar un documento secuencias de símbolos (carácteres, palabras) denominadas n-gramas, donde n indica el tamaño de la secuencia, o utilizan términos multipalabra que previamente deben ser identificados.
- **Pérdida de posibles relaciones semánticas entre los términos de un documento**: algunas propuestas usan el Índice de Latencia Semántica (LSI) que es un método de análisis de coaparición entre rasgos y se basa en el análisis de latencia semántica (LatentSemanticAnalysis, LSA), otras utilizan representaciones basadas en ontologías [Hothoet al 2001].

### Modelo probabilístico de temas (Probabilistic Topic Model)

Los documentos se representan como una mezcla de temas, donde un **tema** es una distribución de probabilidad sobre palabras. Se asume que si un documento trata de un tema en particular, es de esperar que aparezcan palabras concretas en el documento con mayor o menor frecuencia: "carrocería", "llantas" o "volante" aparecerán más a menudo en documentos sobre coches que en documentos de otros temas, mientras que palabras de uso general como "el", "de" o "es" aparecerán igualmente en todos.

Un documento típicamente trata múltiples temas en diferentes proporciones; por lo tanto, en un documento que es 80% sobre coches probablemente habría sobre ocho veces más palabras relacionadas con el tema “coches” que de otros temas.

Los "temas" producidos por las técnicas de este modelado son grupos de palabras relacionadas. Un modelo temático examina un conjunto de documentos y descubre a partir de las estadísticas de las palabras de cada uno cuáles podrían ser los temas y cuál es el equilibrio temático de cada documento. Este modelo supera los problemas asociados con "el término” como tema. Un término puede ser una palabra o una frase. Sin embargo este modelo no puede representar temas complejos, capturar las variaciones de vocabulario y la ambigüedad del sentido de las palabras


La Indexación Semántica Latente Probabilística --Probabilistic Latent Semantic Indexing (PLSI)--y la Asignación Latente de Dirichlet--Latent DirichletAllocation (LDA) --son los dos métodos de modelización de temas más conocidos. Más información en https://en.wikipedia.org/wiki/Topic_model#Algorithms.

### Modelo de lenguaje estadístico de temas(Statistical Language Model)

Es una distribución de probabilidad sobre secuencias de palabras. Se aplica en muchas tareas relacionadas con procesamiento de lenguaje natural: reconocimiento de voz, traducción automática, clasificación y enrutamiento de documentos, reconocimiento óptico de caracteres, recuperación de información, reconocimiento de escritura a mano, corrección ortográfica ...

Uno de los modelos más utilizado es el **modelo de n-gramas**. El modelode n-gramas asume que la probabilidad de una palabra depende de las n palabras anteriores. Es decir, la probabilidad $P(w_1,w_2,w_3,...,w_m)$ de observar la oración $w_1$, $w_2$, ..., $w_m$ se aproxima de la siguiente manera:

$$ 𝑃( 𝑤_1,𝑤_2,…, 𝑤_𝑚)=\prod_{i=1}^m p(𝑤_𝑖|𝑤_1, ..., 𝑤_{𝑖−1}) $$


Cuando en este modelo n=1, es decir no hay dependencia de palabras previas, es equivalente a la bolsa de palabras.

Más información en https://en.wikipedia.org/wiki/Language_modely en el capítulo 3 de [Jurafskyand Martin 2019]

### Modelo semántico vectorial

Se basan en la hipótesis distribucional [Joos1950] que dice que las palabras que ocurren en contextos similares tienden a tener significados similares. Relacionan la similitud en la distribución de las palabras y la similitud en su significado: palabras sinónimas tienden a aparecer en contextos similares. Se hace corresponder la diferencia de significado entre dos palabras con la diferencia en los contextos en los que aparecen.

En este modelo a cada palabra se le asocia un vector y se representa como un punto en algún espacio semántico multidimensional.

En lugar de una matriz de término-documento como en el modelo del espacio vectorial, se utiliza una **matriz palabra-palabra** (término-término) que también se denomina matriz término-contexto dado que las columnas son también palabras, no documentos. La dimensionalidad de esta matriz para un vocabulario V sería $|V| \times |V|$ donde cada elemento o celda representaría el número de veces que la palabra de la fila (objetivo) y la palabra de la columna(contexto) coaparecen en un determinado contexto en un corpus.

Para determinar el tamaño del contexto de la palabra objetivo normalmente se define una ventana de varias palabras por delante y por detras de una considerada objetivo y ese sería el contexto a tener en cuenta. En[Jurafskyand Martin 2019], Cap. 6, pág. 9—10 se puede ver un ejemplo de matriz término-contexto de unos textos extraídos de Wikipedia.

Para determinar la **similitud** entre dos palabras v y w se suele utilizar una medida de similitud entre sus correspondientes vectores: el coseno.

$$ sim(v,w) = cos(v,w) = \frac{v · w}{|v||w|}$$

El valor del coseno varía desde 1 para vectores que apuntan en la misma dirección, a través de 0 para vectores que son ortogonales, a -1 para vectores que apuntan en direcciones opuestas. Como los valores de frecuencia que contienen los vectores no son negativos el coseno varía de 0 a 1.

#### Función de pesado

Utilizar la frecuencia como pesado en los vectores término-contexto no se considera la mejor opción para determinar asociaciones entre palabras ya que en el contexto de una palabra objetivo normalmente ocurren palabras frecuentes con todo tipo de palabras y no son informativas sobre ninguna palabra en particular(artículos, determinantes, verbos auxiliares, …). En lugar de la frecuencia se suele usar TF-IDF: en este modelo representa lo relevante que es una palabra con respecto a otra en una colección y permite determinar que algunas palabras son generalmente más comunes que otras en las colecciones.

Con este modelo se puede por ejemplo:
- encontrar las x palabras más similar es a una objetivo calculando los cosenos entre dicha palabra y el resto de las palabras del vocaculario, ordenar el resultado y seleccionar las x con valores más altos.
- decidir si dos documentos son similares considerando los vectores de todas las palabras de un documento y calculando el centroide de todos los vectores. Dados dos documentos podemos calcular sus vectores documento y estimar su similitud utilizando el coseno.

Una función de ponderación alternativa a TF-IDF es Información Mutua Puntual Positiva PPMI (Positive Pointwise Mutual Information).
PMI es una medida de la frecuencia con la que ocurren dos eventos x e y, comparados con lo que esperaríamos si fueran independientes. Como puede dar como resultado valores negativos, se utiliza PPMI, que en vez de valores negativos devuelve 0.

#### Embeddings

Si se representa una palabra como un vector con las dimensiones de todo el vocabulario muy probablemente el vector será disperso (muchos elementos con valor 0) pero se puede limitar el numero de las dimensiones de forma que la mayoría de los valores de los vectores sean distintos de cero, dando lugar a vectores densos. Dimensiones habituales son valores como 256, 512, o 1024.

La idea de la semántica vectorial es representar una palabra como un punto en algún espacio semántico multidimensional, las representaciones resultantes también reciben el nombre de representaciones distribuidas. Los vectores para representar palabras se llaman embeddings(embebidos) porque la palabra está embebida en un espacio vectorial en particular. Los modelos semánticos vectoriales son muy prácticos porque se pueden aprender automáticamente a partir de texto sin necesidad de etiquetado o supervisión. Actualmenteson la forma habitual de representación en muchas tareas de procesamiento de lenguaje natural: traducción automática, análisisde opinion, generación de textos, Chatbox(sistemas que responden preguntas de usuarios), ...

Para trabajar con representaciones distribuidas una opción esgenerar nuestro propio modelo de vectoreso bien se puede partir de un modelo ya entrenado. Estas representaciones permiten utilizar vectores densos, en los que se limita el tamaño de las dimensiones y la mayoría de los valores de los vectores son distintos de cero. Hay variosmodelosde word embedding disponibles: word2vec (Google); Glove (Stanford, [Pennington et al. 2014] ); fastTest(Facebook, https://fasttext.cc/); y másrecientementeel modelode Bert (Google, https://arxiv.org/pdf/1810.04805.pdf) o Elmo (https://allennlp.org/elmo).

#### Word2vec

Herramienta para entrenar/generar y usar wordembeddings que utiliza modelos predictivos. Los modelos predictivos intentan predecir directamente una palabra a partir de sus vecinos en términos de vectores pequeños y densos que se aprenden durante el entrenamiento a partir del corpus sin etiquetar. La idea detrás de estos métodos es que si podemos predecir en qué contexto aparece una palabra, entonces significa que entendemos el significado de la palabra en su contexto. Su objetivo es agrupar vectores de palabras similares en el espacio multidimensional. Word2vec utiliza una red neuronal de 2 capas que toma como entrada un corpus textual y genera sus correspondientes word embeddings que pueden ser la entrada de una red de aprendizaje profundo o servir para detectar relaciones entre palabras.

Word2Vec implementa dos modelos neuronales:
- CBOW: el modelo predice la palabra objetivo dada una ventana de palabras (el contexto)
- Skip-gram: el modelo predice las palabras contexto a partir de la palabra objetivo

#### FastText[Bojanowskiet al. 2017]

Es una extensión del modelo Word2Vec en el que cada palabra se trata como la suma de sus composiciones de n-gramas de caracteres. El vector para una palabra está compuesto por la suma de sus n-gramas de caracteres. Por ejemplo el vector para la palabra “apple” estaría compuesto por la suma los vectores para los n-gramas "<ap, app, appl, apple>, ppl, pple, pple>, ple, ple>, le> …". El objetivo es obtener mejores representaciones para palabras poco frecuentes y poder generar vectores para palabras que no se encuentran en el vocabulario de los wordembeddings. 

#### Modelos contextualizados: Bert

Las representaciones preentrenadas pueden ser independientes de contexto o contextualizadas y estas últimas unidireccionales o bidireccionales. Los modelos independientes del contexto como word2vec o GloVe generan una única representación vectorial para una palabra ("banco" de entidad financiera y "banco" de sentarse tienen la misma representación). Los modelos contextualizados generan una representación de cada palabra que está basada en las otras palabras de la oración en la que aparece: unidireccional si solo tiene en cuenta las previas y bidireccional si tiene en cuenta las previas y las siguientes.

Bert es un modelo contextualizado para el inglés. Existen modelos de lenguaje contextualizados para diferentes lenguas:
- Español: RigoBERTa (https://www.iic.uam.es/inteligencia-artificial/procesamiento-del-lenguaje-natural/modelo-lenguaje-espanol-rigoberta/)
- Español: BETO (https://github.com/dccuchile/beto)
- Multilingue: Bert multilingüe hasta 104 lenguas dependiendo de la versión (https://github.com/google-research/bert/blob/master/multilingual.md)

## Funciones de pesado

Las funciones de pesado (term weighting functions) constituyen funciones de proyección F dentro de la definición formal del modelo vectorial de representación de documentos. Se pueden identificar 2 tipos: locales y globales.

### Funciones locales.
Aquellas que toman únicamente información del propio documento para obtener el peso de un rasgo en un documento.

#### Binaria

$$
F: Bin(t_i, t_j)= = \begin{cases}
    1 & \text{si el rasgo } t_i \text{ aparece en } d_j \\ 
    0 & \text{si no.}
\end{cases}
$$

#### Term Frequency

$$ F: TF(t_i, d_j) = f_{ij}$$

donde $f_{ij}$ es la frecuencia del rasgo en el documento.

#### Weighted Term Frequency

Proporción de la frecuencia respecto al total de términos del documento.

$$ F: WTF(t_i, d_j) = \frac{f_{ij}}{\sum_{t_p \in d_j} f_{pj}}$$

#### Augmented Normalized Term Frequency

Para compensar el sesgo que pueden introducir los documentos más largos. El denominador
corresponde a la frecuencia máxima en el documento

$$ F: ANTF(t_i, d_j) = 0.5 + 0.5\frac{f_{ij}}{\max(f_{pj} | t_p \in d_j)}$$

#### Normalización Logarítmica

$$ F: L(t_i, d_j) = \log_2(f_{ij} + 1)$$

### Funciones globales

Aquellas que toman información de la colección para obtener el peso de un rasgo en un documento. Para su cálculo suele ser necesario el uso de un fichero invertido con información relativa a las frecuencias de los rasgos en cada documento de la colección.

#### Frecuencia inversa del documento

Medida de si el término es común o no en la colección de documentos

$$ F: IDF(t_i, d_j) = \log \frac{N}{1 + df(t_i)} $$

donde $df(t_i)$ es el nº de documentos de la colección en los que aparece el rasgo $t_i$ y N el nº documentos total, se suma 1 al denominador ya que podría de otra forma ser 0 si el término no está en la colección.

#### Frecuencia del Término x Frecuencia inversa de Documento (TF-IDF)

Un valor alto TF-IDF se alcanza con una elevada frecuencia del término (en el documento dado) y una pequeña frecuencia de ocurrencia del término en la colección completa de documentos. El cociente dentro de la función logaritmo del idf es siempre mayor o igual que 1, el valor de IDF y de TF-IDF es mayor o igual que 0. Cuando un término aparece en muchos documentos el valor de TF-IDF se acerca a 0.

Muy habitual en tareas de minería de textos. [Ver video](
https://www.coursera.org/lecture/ml-clustering-and-retrieval/document-representation-nl267)

$$ F: \operatorname{TF-IDF}(t_i, d_j) = TF \times IDF = f_{ij} \log \frac{N}{1 + df(t_i)}$$


#### Frecuencia inversa Probabilística

$$ F: PIF(t_i, d_j) = \log \frac{N - df(t_i)}{df(t_i)} $$

#### Función Normal

$$ F: N(t_i, d_j) = \sqrt{\frac{1}{\sum_{d_k \in C} f_{ij}^2}} $$

#### Frecuencia Global x Frecuencia Inversa de Documento

$$ F: \operatorname{GF-IDF}(t_i, d_j) = \frac{\sum_{d_{j}\in C }f_{ij}}{df(t_i)}$$


## Selección y reducción de rasgos

La **selección de rasgos** consiste en la transformación de un texto en el conjunto de rasgos que lo podrán representar. Normalmente estas transformaciones básicas se encuadran en lo que se denomina preprocesamientode los textos.

Tareas básicas de preprocesamientoson (ver tema 1):
- Tokenización
- Normalización léxica
- Truncado (Stemming)
- Lematización
- Eliminación de palabras vacías (stopwords)

La **reducción de rasgos** permite disminuir la dimensión de las representaciones que puede llegar a ser muy alta para colecciones de gran tamaño. Permite realizar una ponderación en base a la cual se pueden ordenar todos los rasgos de un vocabulario para seleccionar un subconjunto de ellos. La selección se puede hacer estableciendo un umbral de ponderación mínima o bien prefijando una dimensión reducida, generando así un vocabulario que resultará ser un subconjunto del vocabulario inicial. En muchas ocasiones las funciones de selección y reducción son funciones "orientadas a tarea", es decir, que se definen en función de la tarea posterior.

Las funciones presentadas en el apartado “funciones de pesado” pueden emplearse también como funciones de selección de rasgos:
- Con funciones locales, se seleccionan los rasgos con mayor peso en cada documento y se genera con ellos un vocabulario reducido
- Con funciones globales, se seleccionan los rasgos con más peso dentro de la colección y se genera con ellos un vocabulario reducido.


### Selección de características/rasgos en aprendizaje automático:

Objetivos:
- Reducir el sobreebtranamiento(overfitting): menos datos redundantes se traduce en eliminar ruido.
- Mejorar la precisión: con la eliminación de ruido.
- Reducir el tiempo de entrenamiento: con la reducción de rasgos los algoritmos se entrenan más rápido.

Algunas técnicas:
- Selección univariante: utilizar pruebas estadísticas para seleccionar aquellas características que tienen la relación más fuerte con la variable de salida
- Importancia de las características: Se puede obtener la relevancia de cada característica del conjunto de datos utilizando la propiedad de importancia de la característica del modelo. La importancia de la característica da una puntuación para cada característica de los datos, cuanto mayor sea la puntuación, más importante o relevante será la característica para su variable de salida.
- Ganancia de información (Clasificación): se usa para establecer la calidad de un determinado rasgo en una tarea de clasificación.
- Información mutua (Clasificación). Esta función se ha empleado habitualmente en el contexto del modelado estadístico del lenguaje, fundamentalmente para encontrar relaciones entre rasgos.


