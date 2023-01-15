# Minería de textos - Tema 1 - Introducción PLN

## Conceptos preliminares

La **minería de textos** es el proceso de analizar colecciones de textos para descubrir información y patrones que no aparecen de forma explícita en los textos. Engloba tareas como la **extracción de información**, la **recuperación de información**, la **categorización** y **agrupamiento** de documentos. En minería de textos se utilizan principalmente técnicas de procesamiento del lenguaje natural y de aprendizaje automático. La minería de textos se aplica a colecciones de documentos con información textual no estructurada y escrita en lenguaje natural. La información textual normalmente conforma documentos que pueden agruparse en colecciones o corpus. Las colecciones de documentos o corpus pueden contener anotaciones y metadatos.

La **extracción de información** consiste en extraer automáticamente información estructurada a partir de textos. Abarca procesos como el reconocimiento de nombre de entidades (NER: Named Entity recognition), la extracción de terminología y de relaciones.

La **recuperación de información (Information Retrieval)** consiste en buscar documentos, buscar información dentro de los documentos y en buscar metadatos que describan los documentos. Incluye la búsqueda en todo tipo de repositorios y bases de datos, tanto aisladas como conectadas en red y con hipertexto, como World Wide Web.

La **categorización de documentos** consiste en asignar a un documento una o más categorías en función de su contenido. Las categorías con las que se hace la clasificación están definidas previamente.

El **agrupamiento de documentos (Document Clustering)**, es una forma de organización de documentos en grupos en la que ni la naturaleza de los grupos, ni en ocasiones su número están definidos de antemano.

## Procesamiento del lenguaje natural 

El **procesamiento del lenguaje natural (PLN)** es la rama de la informática cuyo objetivo es el desarrollo de sistemas que permitan a los ordenadores comunicarse con personas utilizando el lenguaje humano.

PLN tiene muchas aplicaciones. PLN se utiliza para adquirir conocimientos a partir de cantidades masivas de datos textuales. Por ejemplo, comprobación de hipótesis a partir de informes médicos, generación de resúmenes, sistemas de diálogos, etc.

### ¿Que hace que PLN sea difícil?

- Alta ambigüedad a todos los niveles: Léxico, Sintáctico, Semántico, de discursos.
	- *Estuvo esperando en el banco durante dos horas* ¿banco de sentarse o banco de dinero?
	– *No los aceptaron en el partido por sus prejuicios* ¿los prejuicios son de ellos o del partido?
	– *Los niños eligieron juguetes alegres* ¿quien o qué era alegre?

- El lenguaje natural implica conocimiento del mundo. *Estuve esperando una hora en el banco para sacar dinero.* el conocimiento del mundo nos permite saber que “banco” es una institución bancaria, y no otro tipo de banco.

- Hay muchos aspectos del lenguaje que intervienen en la interpretación correcta:
	– Negación: al extraer la información es importante saber si un dato, concepto, relación etc. está siendo negado.
	– Especulación: Muchas veces se dice que “puede” darse un hecho.
	– Correferencia: es necesario identificar expresiones que hacen referencia a la misma entidad. *Aunque todos vieron al Presidente, ninguno reconoció al ganador de las últimas elecciones*.

### Evaluación de las tareas de PLN

Obtener datos de evaluación de los modelos y sistemas utilizados es fundamental para el avance del área. Es necesario comparar modelos y sistemas de formas justa: sobre los mismos datos (corpus o colecciones comunes) y usando las mismas medidas.

#### Corpus

Un **corpus** es una compilación de textos en formato electrónico. Son un recurso fundamental en el desarrollo de aplicaciones basadas en PLN:
- Permiten evaluar los sistemas.
- Proporcionan un marco común para comparar técnicas alternativas.
- Permiten entrenar los sistemas de aprendizaje automático supervisados: ajustan sus parámetros a partir de una parte del corpus usada para entrenamiento.
- Proporcionan una muestra amplia y natural del lenguaje.
- Son una fuente de datos estadísticos sobre las palabras, sus relaciones y sus construcciones.
- Algunas de sus aplicaciones son:
	- Asignación automática de categorías léxicas,
	- Desambiguación sintáctica
	- Extracción de gramáticas
	- Traducción automática
	- Extracción de entidades (nombres, medicamentos, etc.)
	- Extracción de relaciones

##### Tipos generales de corpus

- Sin anotar: texto puro.
- Anotados: marcados con distintos tipos de información lingüística (se les denomina Gold standard y se usan de referencia).

Otra clasificación puede ser **monolingüe** o **multilingüe**: los primeros si pertenecen a una única lengua. Los segundos si proporcionan anotaciones similares en distintas lenguas, pueden ser:
- Corpus paralelo: contiene los mismos textos en más de una lengua
- Corpus comparable: Contiene un número equilibrado de textos en distintas lenguas. También están equilibrados respecto al tema y tipo de textos.

##### Ejemplos

**Corpus de Brown** (http://www.nltk.org/nltk_data)

Contiene más de un millón de palabras. Anotado con categorías léxicas. Consta de archivos de 15 temáticas. Considera un conjunto de 87 etiquetas: AT artículo, BE verbo “to be” en infinitivo o imperativo, CC conjunción copulativa, DT determinante/pronombre, etc.

```
<s> The/AT Fulton/NP County/NP Grand/NP Jury/NP said/VBD Friday/NR
an/AT investigation/NN of/IN Atlanta/NP 's/\$ recent/JJ primary/NN
election/NN produced/VBD “/” no/AT evidence/NN ''/'' that/CS any/DTI
irregularities/NNS took/VBD place/NN ./.
<s> The/AT jury/NN further/RBR said/VBD in/IN term-end/NN
presentments/NNS that/CS the/AT City/NP Executive/NP Committee/NP ,/,
which/WDT had/HVD over-all/JJ charge/NN of/IN the/AT election/NN ,/, ”/”
deserves/VBZ the/AT praise/NN and/CC thanks/NNS of/IN the/AT City/NP
of/IN Atlanta/NP ''/'' for/IN the/AT manner/NN in/IN which/WDT the/AT
election/NN was/BEDZ conducted/VBN ./.
```

**Corpus de Susanne** (http://www.grsampson.net/Resources.html)
Contiene 130.000 palabras. Formado por un subconjunto de 64 archivos del corpus de Brown. Anotado con categorías léxicas y análisis sintáctico. Por cada palabra de texto hay una línea de anotaciones. Consta de:
- referencia (nombre de archivo, línea, posición en la línea)
- estado (indicativo de abreviatura o símbolo)
- categoría gramatical
- Palabra
- Lema
- análisis sintáctico

```
Ref. est. cat. palabra lema análisis
N06:0180.12 - NN1u Baldness baldness [S[Ns:s.Ns:s]
N06:0180.15 - VBDZ was be [Vsu.
N06:0180.18 - VVGt attacking attack .Vsu]
N06:0180.21 - APPGm his his [Ns:o.
N06:0180.24 - NN1c pate pate .Ns:o]S]
```

Otros ejemplos:

- Corpus ADE (relaciones entre efectos adversos y medicamentos) https://github.com/trunghlt/AdverseDrugReaction
- Bioscope: textos biomédicos anotados con negación y especulación. http://rgai.inf.u-szeged.hu/index.php?lang=en&page=bioscope
- Corpus DDI: anotación de medicamentos e interacciones entre ellos. https://hulat.inf.uc3m.es/DrugDDI/DrugDDI.html
- Corpus Europarl (Actas del parlamento europeo en 21 lenguas de Europa: Español, Inglés, Francés, Griego, …, con alineamiento o correspondencia a nivel de oración) http://www.statmt.org/europarl/
- Corpus DIANN: discapacidades anotadas https://github.com/gildofabregat/DIANNIBEREVAL-2018/tree/master/DIANN_CORPUS


#### Campañas de evaluación

Proponen retos a los participantes, proporcionando datos y un marco de evaluación común y bien definido.
- TREC: Text Retrieval conference (evaluación a gran escala de las metodologías de recuperación de textos)
- CLEF: Conference and Labs of the evaluation Forum (promueve la investigación, la innovación y el desarrollo de sistemas de acceso a la información con énfasis en la información multilingüe y multimodal).
- Semeval: Semantic Evaluation (evalúa sistemas de análisis semántico)
- Ibereval/Iberlef: Lenguas Ibéricas (organiza campañas de evaluación en castellano y otras lenguas ibéricas)

#### Medidas de evaluación

**Precisión**: fracción de predicciones del modelo propuesto acertadas (coinciden con los datos de referencia):

$p = \frac{tp}{tp + fp}$

**Cobertura o exhaustividad (recall)**: fracción de los datos de referencia que han sido propuestas por el modelo evaluado:

$r = \frac{tp}{tp + fn}$

Siendo:
- tp: true prositive
- fp: false positive
- fn: false negative

**Medida-$F$**: media armónica de precisión y cobertura: cuando p y r se ponderan igual.

$Medida-F=2\frac{p\times r}{p+r}$

**Medida $F_{\beta}$ : forma general de la media armónica que permite dar más importancia a la precisión o a la cobertura en función del parámetro $\beta$:

$Medida-F_{\beta}=(1+\beta^2)\frac{p\times r}{\beta^2 \times p+r}$


También se usan otras medidas específicas de tareas de PLN concretas.

#### Baseline

En una tarea es importante tener un referencia de qué niveles de las medidas pueden considerarse aceptables. El rango de valores cambia mucho de una tarea a otra. Los resultados de otras propuestas son una referencia. Pero no siempre se dispone de ellos cuando aparece una nueva tarea. **Baseline** para una tarea es la medida de los resultados de un modelo básico que resuelve la tarea.

Ejemplo:
- Tarea: asignar el significado correcto a una palabra ambigua en el contexto de un documento.
- Baseline: asignar el significado más frecuente de la palabra.
- Se espera que cualquier otro modelo propuesto sea capaz de superar el baseline.

#### Análisis de errores

Es crucial de cualquier aplicación de PLN. Puede ayudar a encontrar errores en los programas, problemas en los datos de entrenamiento y, también es fundamental para desarrollar nuevos modelos de conocimiento o algoritmos para usar en la resolución de problemas.

Las matrices de confusión o tablas de contingencia se utilizan para analizar los errores de los clasificadores. Una **matriz de confusión** para una tarea de clasificación de N clases es una matriz de NxN donde la celda(x,y) contiene el número de veces que un elemento con la clasificación correcta x fue clasificado por el modelo como y.

| Clase | 1 | 2 | 3 | 4 | Total |
|---------|----|----|----|----|------|
| 1 | 70 | 10 | 15 | 5 | 100 |
| 2 | 8 | 67 | 20 | 5 | 100 |
| 3 | 0 | 11 | 88 | 1 | 100 |
| 4 | 4 | 10 | 14 | 72 | 100 |

En el ejemplo, la clase 1 se ha clasificado 70 veces de forma correcta, 10 veces de forma errónea, como clase 2, etc.
A veces la matriz de confusión contiene porcentajes.

### Herramientas

#### Nltk: Natural Language Toolkit https://www.nltk.org/
Librería para desarrollar programas en python para procesar datos de lenguaje natural. Disponible para linux, windows y Mac OS

#### Spacy https://spacy.io/
Librería para desarrollar programas en python para procesar datos de lenguaje natural. Disponible para linux, windows y Mac OS. Tiene modelos preentrenados en inglés y español.

#### Otras herramientas
- Stanford
- Treetagger
- Freeling

## Introducción al procesamiento del Lenguaje Natural

Algunos procesos importantes para la minería de textos:

- Preprocesador de textos: Normalización, Expresiones, regulares, distancia de edición
- POS tagging
- Stemming
- Chunking
- Parsing
- Desambiguación del sentido de las palabras


### Preprocesado de textos

#### Normalización de textos

Proceso de transformación del texto para obtener una forma canónica. El objetivo es uniformizar la forma del texto para facilitar posteriores procesos. Tareas que se aplican normalmente como parte del proceso de normalización: normalización de la forma de las palabras y segmentación o tokenización de oraciones y palabras.

#### Normalización de palabras
El objetivo es unificar palabras que se refieren de distinta forma a la misma entidad (forman una clase común). Ejemplos: “U.S.A” y “USA”, “Manzana” y “manzana”, “helado de fresa” y “helado de fresa”,  Erratas de escritura...
Técnicas comunes: Eliminar acentos, quitar espacios repetidos, reducir todas las palabras a minúsculas, etc.

#### Expresiones regulares
Lenguaje formal para especificar patrones de texto. Herramienta fundamental en muchas tareas de PLN. Distinguen mayúsculas y minúsculas. Ver descripción en Capítulo 2: Speech and Language Processing. Daniel Jurafsky & James H. Martin.

#### Distancia de edición
Distancia de edición mínima entre dos cadenas: Número/coste de las operaciones de edición: Inserción (i), Borrado (d), Sustitución (s) necesarias para transformar una cadena en la otra. Cada operación puede tener un coste distinto. Es necesario explorar todas las combinaciones de operación para seleccionar la de menor coste. Es importante para medir la similitud entre dos cadenas. Se usa en la corrección a errores ortográficos, extracción de información, la traducción automática, la recuperación de información, etc.

#### Palabras vacías (stopwords)
Palabras sin significado propio: artículos, preposiciones, etc.  Suelen ser las más frecuentes en un idioma. En muchos procesos de PLN se eliminan para centrar el análisis en las palabras con contenido semántico.

#### Segmentación de palabras
Rompe una cadena de caracteres (grafemas) en una secuencia de palabras.

#### Segmentación (tokenización) de oraciones

Las claves usuales para segmentar un texto en oraciones son la puntuación: puntos, los signos de interrogación y los signos de exclamación. Los signos de interrogación y los signos de exclamación son marcadores relativamente inequívocos de los límites de las oraciones. Los puntos pueden ser ambiguos: fin de oración o marcador de abreviatura: Mr. O Inc. Por ello a veces la segmentación (o tokenización) de las oraciones y de las palabras se aborda conjuntamente.

Suelen decidir primero (en base de reglas o de aprendizaje automático) si un punto forma parte de la palabra o es un marcador de límite de la oración. Un diccionario de abreviaturas puede ayudar a determinar si el punto forma parte de una abreviatura de uso común.

### Análisis morfolológico
La morfología es el campo de la lingüística que estudia la estructura de las palabras. Un morfema es la unidad lingüística más pequeña que tiene significado semántico. El análisis morfológico es la tarea de segmentar una palabra en sus morfemas: carried --> carry + ed (past tense).

Se distinguen dos grandes grupos de morfemas: las raíces (o stems) y los afijos (que s)

- Raíz: morfema principal que proporciona significado a la palabra

- Afijos: Añaden significado adicional de distintos tipos (número, género, tiempo del
  verbo, etc.). Pueden ser prefijos o sufijos.

Ejemplos de análisis morfológico:
- Inglés: fox → fox;  cats → cat + s
- Español:  perros → perr + o + s;  acrecentar → a + crec + entar

Los **lematizadores** identifican el lema o forma canónica de una palabra: leíamos → leer. 

El proceso de **stemming** se refiere al proceso de eliminar la parte final de las palabras, que en muchas ocasiones es una aproximación a la lematización. El proceso de stemming es muy frecuente en PLN: Permite reconocer todas las palabras correspondientes a una misma raíz.

NLTK incluye varios stemmer externos (Porter y Lancaster). Son preferibles a los que podemos construir manualmente con expresiones regulares ya que son capaces de manejar casos irregulares. El stemmer de Porter es una buena elección y probablemente el más utilizado. Tiene versiones para distintos idiomas.

### POS tagging

Cada palabra de una oración se puede clasificar en clases: verbos, adjetivos, nombres, etc. El etiquetado gramatical o Part-Of-Speech (POS) Tagging es el proceso de etiquetar las palabras de una oración con su categoría léxica, en base a su definición y el contexto en la oración (las etiquetas y palabras que la rodean). Ejemplo (Corpus de Brown)

POS tagging es útil en muchos problemas de PLN: Identificación de entidades, Desambiguación del sentido de las palabras, Análisis sintáctico, etc.

Principales técnicas:

- Basados en reglas
- Modelos estadísticos: Modelo de probabilidades de las secuencias de palabras y sus etiquetas en función de los datos de un corpus anotado.
- Aprendizaje automático

Para evaluarlo se utiliza la exactitud (accuracy): número de etiquetas acertadas sobre el número total de palabras etiquetadas y precisión, cobertura y medida-F para cada tipo de etiqueta del conjunto de etiquetas considerado.

### Chunking
Parte y etiqueta secuencias multipalabra. Cada secuencia es un chunk. Uno de los más usados es el de las frases nominales: NP-chunking.
Ejemplo (Wall street journal):

```
[ The/DT market/NN ] for/IN [ system-management/NN
software/NN ] for/IN [ Digital/NNP ] [ ’s/POS
hardware/NN ] is/VBZ fragmented/JJ enough/RB that/IN
[ a/DT giant/NN ] such/JJ as/IN [ Computer/NNP
Associates/NNPS ] should/MD do/VB well/RB there/RB ./.
```

NP-chunks suelen ser menores que las frases nominales ya que NP-chunks no contienen otros NP-chunks. Los sintagmas preposicionales o las cláusulas subordinadas que modifican un nominal no se suelen incluir.

Se puede construir una gramática de detección de chunks usando expresiones regulares sobre las etiquetas POS. (Capitulo 7 del libro “Natural language processing with Python”)

También puede construirse con clasificadores entrenados.

### Análisis sintáctico

Identifica la estructura sintáctica de una oración. La estructura asignada depende del tipo de gramática considerada. Numerosas aplicaciones: Representación intermedia para el análisis semántico, Sistemas de búsqueda de respuestas, Sistemas de extracción de información, etc.

Ambigüedad estructural: una oración puede tener múltiples árboles de análisis respecto a una misma gramática.

Técnnicas:
- Programación dinámica: Análisis descendente (partiendo del símbolo inicial de la gramática) o Análisis ascendente (partiendo de los símbolos terminales)
- Aprendizaje automático a partir de un corpus anotado con análisis (treebank).

#### Análisis de constituyentes
Una frase bien formada se puede descomponer en constituyentes de acuerdo a unas reglas gramaticales. Un analizador sintáctico de constituyentes es un programa que obtiene la estructura asociada a una frase respecto a una gramática. Las gramáticas que utilizan estos analizadores son Context Free Grammar (CFG): las producciones o reglas no dependen de las palabras anteriores o posteriores a las asignadas a la regla.

Una CFG consiste en:
- Un conjunto de terminales (las palabras del lenguaje a representar) $\Delta$
- Un conjunto de símbolos no terminales N (S, NP, etc.)
- Un símbolo inicial de la gramática $S \in N$
- Un conjunto de producciones o reglas R: $A \rightarrow \alpha$ donde $\alpha \in (\Delta \cup N)*$

Algunos análisis validos de las oraciones pueden ser muy infrecuentes (como el ejemplo del elefante en pijama). En las probabilistic CFG (PCFGs) las reglas gramaticales tienen asociada una probabilidad. El analizador busca la secuencia de producciones más probable. Permiten asociar una probabilidad a cada árbol de análisis alternativo. Frecuentemente interesa el más probable.

#### Análisis de dependencias
Análisis alternativo basado en las relaciones entre palabras de una oración. Las dependencias pueden estar anotadas (tener nombre). Se aprenden de un corpus anotado con análisis de dependencias (Treebank de dependencias). Generalmente no hay una gramática (a diferencia del análisis de constituyentes). Las palabras están también en nodos internos (a diferencia del análisis de constituyentes que produce árboles con palabras sólo en los nodos hoja).

### Desambiguación del sentido de las palabras
Muchas palabras tienen más de un significado o sentido dependiendo del contexto en que se usan: *Los pescadores encontraron un gran banco de sardinas*, *Tuve que ir al banco a sacar dinero*, *Nos sentamos en el banco hasta que llegó*.

La desambiguación del sentido de las palabras (Word Sense Disambiguation (WSD)) consiste en seleccionar el sentido (interpretación o significado) correcto de una ocurrencia de una palabra en un documento (o conversación) de entre todos los posibles de la palabra. Muchas aplicaciones requieren seleccionar el sentido correcto. Aplicaciones:
- traducción automática: WSD es necesaria las palabras que tienen diferentes traducciones para diferentes sentidos.
- Recuperación de información: es necesario desambiguar las consultas: depresión puede hacer referencia una enfermedad, a una situación económica, etc.
- Extracción de información: para la interpretación correcta de los textos. Drug puede hacer referencia a un medicamento o a una sustancia ilegal.

Relaciones entre sentidos:
- Sinónimos: distintas palabras con el mismo (o muy similar) significado.
- Hipónimo: un sentido es hipónimo de otro si el primero es más específico (una subclase).
- Hiperónimo: un sentido es hiperónimo de otro si el primero es más general (superclase).

Técnicas:
- Métodos basados en diccionarios y bases de conocimiento léxico: por ejemplo en base a distancias semánticas entre las definiciones de las palabras.
- Métodos supervisados: entrenados sobre corpus anotados con los sentidos correctos de las palabras.
- Métodos semisupervisados o mínimamente supervisados: Usan una fuente secundaria de conocimiento como un pequeño corpus anotado como datos de semillas en un proceso de bootstrapping.
- Métodos no supervisados: Estos evitan (casi) completamente la información externa y trabajan directamente desde los corpus no anotados. La hipótesis subyacente es que sentidos similares aparecen en contextos similares y, por lo tanto, se pueden inducir sentidos a partir del texto agrupando ocurrencias de palabras utilizando alguna medida de similitud de contexto.

Wordnet: una base de datos de relaciones léxicas. Este lexicón o taxonomía se divide en 5 categorías: nombres, verbos, adjetivos, adverbios y palabras de función (pronombres, etc).Representación del sentido de las palabras: synsets (synonym sets) como unidades básicas. El significado de una palabra se representa listando las formas de la palabra que se pueden usar para expresarlo.

Los elementos del synset rara vez son verdaderos sinónimos: Wordnet no intenta capturar distinciones sutiles. Los synsets suelen ser suficiente para analizar diferencias y similitudes. Wordnet también proporciona glosses: definiciones y/o ejemplos. WordNet ha sido usado para diferentes y numerosos propósitos: desambiguación del significado de palabras, recuperación de información, clasificación automática de texto, resumen automático de texto, traducción automática.

A veces se trata con métodos supervisados, pero frecuentemente se recurre a diccionarios y tesauros. Algoritmo de Lesk: elige el sentido cuya definición (glosa) comparte más palabras con el contexto de la palabra a desambiguar.