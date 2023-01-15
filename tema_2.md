# Minería de Textos - Tema 2 - Extracción de información

Área del Procesamiento del Lenguaje Natural con el objetivo de convertir la información no estructurada contenida en los textos en datos estructurados:
- Detectar y extraer automáticamente ciertas partes relevantes de un texto.
- Combinar la información encontrada en base a sus relaciones.

Se distinguen las tareas y subtareas:
- Detección de Entidades Nombradas (NER)
- Extracción de relaciones.
- Reconocimiento de expresiones temporales.
- Extracción de sucesos.
- Rellenado de plantillas.

## Detección de Entidades Nombradas
Una entidad nombrada (named entity) son nombres propios (personas, organizaciones, lugares, etc.) y, por extensión, expresiones que identifican conceptos de una clase: enfermedades, medicamentos, proteínas, etc. El **reconocimiento de entidades** trata de identificar el segmento de texto que constituye la entidad y asignarle una clase.

```
<ENAMEX TYPE=„LOCATION“>Italy</ENAMEX>‘s business world was rocked by
the announcement <TIMEX TYPE=„DATE“>last Thursday</TIMEX> that Mr.
<ENAMEX TYPE=„PERSON“>Verdi</ENAMEX> would leave his job as vice-president
of <ENAMEX TYPE=„ORGANIZATION“>Music Masters of Milan, Inc</ENAMEX>
to become operations director of <ENAMEX TYPE=„ORGANIZATION“>Arthur Andersen
</ENAMEX>.
```

Identificar un nombre propio (o concepto) y su clase depende de:
- La estructura interna. *Sr. Alvarez*
- El contexto: *His job as vice-president of Music Masters of Milan* (Indicio de que Music Masters of Milan es una compañía)

### Métodos

#### Diccionarios

Diccionarios o gazeteers: Un gazeteer es una lista de nombres, generalmente de lugares. Incluyen las distintas formas de nombrar una localización, junto con información política, geográfica y geológica detallada. Un ejemplos en https://gate.ac.uk/sale/tao/splitch13.html

En muchos casos son demasiado numerosas para recogerlas todas en diccionarios. Cambian frecuentemente, aparecen en formas variadas o abreviadas.

#### Reglas

Un método para NER es usar reglas o patrones que se ajustan al tipo de entidades buscadas. Suelen utilizarse expresiones regulares, que en muchos casos se construyen manualmente. Funciona bien cuando existe un patrón común a las entidades buscadas

La expresiones regulares (regex) son un lenguaje para construir patrones a partir de otros más simples. https://cheatography.com/davechild/cheat-sheets/regular-expressions/

#### Aprendizaje automático

1. Entrenamiento: recoger un conjunto de documentos de entrenamiento representativo y etiquetar cada token (palabra/signo) con la clase de su entidad o con other (O). Diseñar los rasgos apropiados para caracterizar el texto y las clases. Algunos rasgos típicos son: palabra, palabras vecinas, POS, POS de las palabras vecinas, contiene un determinado prefijo o sufijo, mayúsculas, etc. Entrenar un clasificador secuencial para predecir las etiquetas de los datos.

2. Predicción/evaluación: recibir un conjunto de documentos a etiquetar y ejecutar el sistema de ML entrenado en la fase anterior para predecir la etiqueta de cada token. Producir una salida con las entidades reconocidas en un formato legible. Se evalua sobre un conjunto de datos (test) no usados en el desarrollo del sistema (entrenamiento). Algunas métricas más usuales son la precisión, cobertura y medida-F. La unidad de evaluación es la entidad, no la palabra: **Tim Wagner** cuenta como una única respuesta. Este componente de segmentación que no está presente causa algunos problemas en la evaluación. Por ejemplo, un sistema que etiquetara a **American** pero no a **American Airlines** como una organización causaría dos errores, un falso positivo para O y un falso negativo para I-ORG. Además, si se usan entidades como unidad de respuesta pero de palabras como unidad de entrenamiento se produce un desajuste entre las condiciones de entrenamiento y de test.

#### Deep Learning

Suelen utilizar embeddings de palabras para representar la entrada: Word embeddings es el nombre de un conjunto de técnicas para construir representaciones vectoriales del significado de las palabras de un idioma, de forma que la dimensión de los vectores es mucho menor que la cantidad de palabras del idioma.  Su generación se basa en la hipótesis distribucional: las palabras que aparecen en contextos similares tienden a tener significados similares. (Tema 3)

Los sistemas DL de detección de entidades usan típicamente Bi-LSTM (Long Short Term Memory). La entrada son los embedding de palabras y caracteres de las palabras de entrada.Producen una probabilidad para cada posible etiqueta asignable a cada palabra. Pueden mejorarse concatenando layers que mejoren la salida.

Los **transformers** son redes neuronales profundas para procesar secuencias, basadas en la autoatención, que están llevando a enormes avances en tareas de PLN. Bidirectional Encoder Representations from Transformers(BERT) es uno de los modelos más utilizados. La atención ayuda a establecer conexiones entre cualquier parte de la secuencia, por lo que las dependencias de largo alcance dejan de ser un problema: las dependencias de largo alcance tienen la misma probabilidad de ser tenidas en cuenta que cualquier otra dependencia de corto alcance. Pueden superar los resultados de otros tipos de redes si se cuenta con suficientes datos y potencia de cómputo. También se han aplicado a NER.

### Sistemas de etiquetado

Los **sistemas de etiquetado** establecen clases para las palabras que los clasificadores pueden aprender. Tipos:
- IOB: Formado por 
    - B-{ENT}: Token inicial de la entidad ENT
    - I-{ENT}: Resto de tokens de la entidad ENT
    - O: No es entidad (Other)
- IO: Solo utiliza I y O
- Existen otros: BILOU, etc.

### Corpus y competiciones

Algunas **competiciones** que han tratado NER son:
- Conll 2002 (Spanish, Dutch) Language-Independent Named Entity Recognition (I) https://www.clips.uantwerpen.be/conll2002/ner/
- Conll 2003 (English, German) Language-Independent Named Entity Recognition (II) https://www.clips.uantwerpen.be/conll2003/ner/

Algunos **corpus** con entidades anotadas:
-  The CoNLL 2003 corpus (inglés) https://www.clips.uantwerpen.be/conll2003/ner/ (annotations) and NIST (text).
- Corpus del dominio biomédico: ADE: reacciones adversas a medicamentos https://github.com/trunghlt/AdverseDrugReaction/tree/master/ADE-Corpus-V2

## Extracción de relaciones

Trata de identificar la existencia de relaciones entre ciertos tipos de entidades. Generalmente una relación es un conjunto ordenado de tuplas sobre los elementos de un dominio. Algunos tipos son *Parte de (part-of)**, **PERSONA Presidente-de ORGANIZACIÓN**, **MEDICAMENTO Cura ENFERMEDAD**, **MEDICAMENTO provoca EFECTO ADVERSO**

### Métodos

#### Patrones léxico-sintácticos

Conjunto de reglas léxico-sintácticas que especifícan explícitamente la relación. Por ejemplo: *Y such as X*, *Y, such as X*, *X or other Y*, *X and other Y”, *Y including X*. Suelen tener una precisión alta y pueden adaptarse a dominio específicos. Perosuelene tener baja cobertura (difícil pensar en todos los patrones posibles) y requerir alto esfuerzo.

#### Aprendizaje automático supervisado

Un posible enfoque  es como dos problemas de clasificación: primero decidir si dos entidades están relacionadas y después decidir la clase de un par de entidades
relacionadas. Se suelen utilizar carcaterísticas de las entidades y del contexto como rasgos del clasficador.

Otro enfoque es utilizar clasificadores secuenciales. El clasificador lee los datos secuencialmente de izquierda a derecha. Los datos se anotan en forma de secuencia con las etiquetas de salida consideradas.


#### Técnicas semisupervisadas

Si hay suficientes datos de entrenamiento y el conjunto de test es similar al de entrenamiento, los métodos supervisados identifican relaciones de forma muy precisa. Sin embargo, el etiquetado manual de un conjunto de entrenamiento extenso es muy costoso. Por ello se recurre a otras técnicas que requieren menos datos como las semi-supervisadas.

La técnica de **Bootstrap** parte de un conjunto de patrones semilla muy precisos o de un pequeño conjunto semilla de tuplas de entidades relacionadas. Se toman las entidades de los pares de la semilla y se buscan oraciones en nuevos documentos (web, otros conjuntos de datos) que contengan a ambas entidades. Se extraen y generalizan los contextos de esas oraciones para aprender nuevos patrones. Los nuevos patrones se pueden utilizar para recoger nuevas tuplas.

Los sistemas de bootstrapping asignan valores de confianza a las nuevas tuplas para evitar la deriva semántica (semantic drift): un patrón erróneo lleva a la introducción de tuplas erróneas, que a su vez lleva a patrones erróneos. Los valores de confianza de los patrones se basan en el equilibrio de dos factores: el rendimiento del patrón con respecto al conjunto actual de tuplas (con cuantas encaja) y, la productividad del patrón en términos del número de coincidencias que produce en toda la colección de documentos. Establecer umbrales de confianza conservadores para la aceptación de nuevos patrones y tuplas durante el proceso de bootstrapping ayuda a evitar que el sistema se aleje de la relación objetivo.

La técnica de **supervisión distante** utiliza una base de datos para obtener un enorme conjunto de semillas (no validadas) en lugar de usar un pequeño conjunto de semillas. Extrae numerosos rasgos (léxicos, sintácticos, etc.) de todos estos ejemplos Los combina en un clasificador.

#### Técnicas no supervisadas

**Open Information Extraction** (Open IE) no utiliza datos de entrenamiento, ni listas de relaciones. Se basan en modelos estadísticos. Algunos se basan en extraer ciertas relaciones entre determinadas partes de la oración (SN y verbos) que cumplen determinadas restricciones léxicas y eliminar las relaciones que no aparecen con suficiente frecuencia.

#### Evaluación de la extracción de relaciones semi o no supervisada

Tiene varios problemas. En primer lugar es que no hay un gold estándar con el que comparar. EN el corpus utilizado no se puede calcular la precisión (no se sabe cuales son correctas),  ni la cobertura (no se sabe cuales han escapado a la detección).

Una posible aproximación a la precisión es elegir una muestra aleatoria de la salida y comprobar la precisión manualmente.

También puede calcularse la precisión a distintos niveles de cobertura. En cada caso se toma una muestra aleatoria del conjunto.

## Extracción de expresiones temporales

Reconocimiento y normalización de expresiones temporales (horas, fechas): *3 de la tarde*, *Mañana*, *Desde ayer*, *Dos veces al mes*... Tiene particular importancia en tareas como la búsqueda de respuestas.

Son construcciones gramaticales que tienen un disparador léxico como núcleo. Los disparadores pueden ser nombres, comunes y propios, adjetivos y adverbios. Algunos ejemplos(inglés):
- Nombres: morning, noon, night, winter, dusk, dawn, ...
- Nombres propios: January, Monday, Easter, Ramadan,...
- Adjetivos: recent, past, annual, former
- Adverbios: hourly, daily, monthly, yearly

### Tipos

Pueden referirse a: 
- Expresiones temporales absolutas: se corresponden con una fecha u hora concretas: *April 24, 1916*, *The summer of ’77*, *10:15 AM*, *The 3rd quarter of 2006*.
- Relativas: hacen referencia a otro punto del tiempo: *yesterday*, *next semester*, *two weeks from yesterday*, *last quarter*
- Duraciones: periodos de tiempo de distinta granularidad: *four hours*, *three weeks*, *six days*, *the last three quarters*.

### Esquema de anotación TimeML

Las expresiones temporales se anotan con la etiqueta `<TIMEX3>` y varios atributos para ella. Existen corpus anotados con este esquema como TimeBank 1.2
```
A fare increase initiated <TIMEX3>last week</TIMEX3> by
UAL Corp’s United Airlines was matched by competitors
over<TIMEX3>the weekend</TIMEX3>, marking the second
successful fare increase in <TIMEX3>two weeks</TIMEX3>
```
En aprendizaje automático se utiliza el esquema IOB (inside, outside, begin) para las
expresiones delimitadas por TIMEX3.


### Normalización temporal
Proceso de correspondencia de una expresión temporal con un punto específico del tiempo o con una duración. Puntos del tiempo pueden ser días del calendario y/o horas del día. Duraciones son longitudes de tiempo y también puntos de comienzo y fin. Los tiempos normalizados se representan con un atributo VALUE de la norma estándar ISO 8601.


| Unit |  Pattern  Sample | Value |
|------|------------------|-------|
|Fully specified dates | YYYY-MM-DD | 1991-09-28 |
|Weeks | YYYY-Wnn | 2007-W27 |
| Weekends | PnWE | P1WE |
| 24-hour clock times | HH:MM:SS | 11:13:45 |
| Dates and times | YYYY-MM-DDTHH:MM:SS | 1991-09-28T11:00:00 |
| Financial quarters | Qn | 1999-Q3 |

```
<TIMEX3 id = "t1" type=”DATE” value =”2007-07-02”
functionInDocument=”CREATION_TIME”> July 2, 2007 </TIMEX3> A fare
increase initiated <TIMEX3 id=”t2” type=”DATE” value =”2007-W26”
anchorTimeID=”t1”>last week</TIMEX3> by United Airlines was matched
by competitors over <TIMEX3 id =”t3” type=”DURATION” value =”P1WE”
anchorTimeID=”t1”> the weekend </TIMEX3>, marking the second
successful fare increase in <TIMEX3 id=”t4” type =”DURATION” value
=”P2W” anchorTimeID=”t1”> two weeks </TIMEX3>.
```

Las exp. temporales totalmente calificadas contienen año, mes y día en alguna forma convencional. Son infrecuentes en los textos reales. La mayor parte de las expresiones y suelen hacer referencia implícita a un punto del tiempo: el ancla temporal (temporal anchor) del documento.


## Extracción de sucesos
Trata de extraer sucesos de la vida real que ocurre en un punto del
tiempo y del espacio. Los sucesos se clasifican en acciones, estados, de notificación (dice, explica, etc.), percepción, etc. 

Suele nombre, tipo de evento, agente, tiempo y lugar.
```
[EVENT Citing] high fuel prices, United Airlines [EVENT said] Friday it has [EVENT increased] fares by $6 per round trip on flights to some cities also served by lower-cost carriers. American Airlines, a unit of AMR Corp., immediately [EVENT matched] [EVENT the move], spokesman Tim Wagner [EVENT said]. United, a unit of UAL Corp. [EVENT said] [EVENT the increase] took effect Thursday and [EVENT applies] to most routes where it [EVENT competes] against discount carriers, such as Chicago to Dallas and Denver to San Francisco.
```

La extracción de sucesos suele abordarse conaprendizaje supervisado (IO tagging) con los rasgos de siempre.

### Ordenación temporal de sucesos
Ordenar los sucesos y expresiones temporales en una línea de tiempo. Útil en sistemas de búsqueda de respuesta y generación de resúmenes.

Una tarea más simple es la ordenación parcial (**relaciones de Allen**): A before B, B after A, A overlaps B, A meets B, A equal B, A starts B, A finishes B, A during B, etc.

El corpus TimeBank contiene la mayor parte de la información mencionada en esta sección.

## Rellenado de plantillas (templates)

Organizar la información de los textos en secuencias estructuradas de información (scripts) sobre sucesos puede ser muy útil para realizar inferencias (razonamiento). Las plantillas son un caso simple de script con un conjunto fijo de slots a rellenar.

```
FARE-RAISE-ATTEMP: LEAD AIRLINE: UNITED AIRLINES
AMOUNT: $6
EFFECTIVE DATE: 2006-10-26
FOLLOWER: AMERICAN AIRLINES
```

La tarea suele modelarse con dos sistemas supervisados separados:
- Reconocimiento de plantillas
- Extracción de rellenos de rol (role-filler extraction): detección del rol de la entidad (LEAD-AIRLINE, AMOUNT, etc.)