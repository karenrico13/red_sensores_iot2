
# Bitacora individual - Semana [03]

> Copia este archivo y renombralo como `s[XX]-[tu-nombre].md`.
> Completa todas las secciones con tus propias palabras. Esta bitacora es
> individual, aunque el codigo pueda haberse construido en equipo.

## 1. Datos de la actividad

* **Estudiante:** Karen Manuela Rico Maldonado
* **Equipo:** los violentos
* **Semana:** 3
* **Fecha del laboratorio:** 2026-09-20
* **Fecha del taller:** 2026-09-27
* **Tema principal:** Búsqueda lineal, búsqueda binaria, precondiciones y análisis de eficiencia ($O(n)$ vs $O(\log n)$)


* **Pregunta de la semana:** ¿Cómo encontramos una lectura específica cuando el repositorio pasa de cientos
  a cientos de miles o millones de registros, y cuánto cuesta hacerlo?



## 2. Prediccion antes de ejecutar

Antes de abrir o ejecutar el programa, responde:

1. **Que creo que va a ocurrir?**
   [Espero que la búsqueda lineal gaste una cantidad de comparaciones directamente proporcional al 
   tamaño del arreglo ($n$), mientras que la búsqueda binaria sea exponencialmente más rápida, requiriendo muy pocas 
   iteraciones incluso con un millón de registros, siempre y cuando los datos estén ordenados por el timestamp.]


2. **Que parte del programa o del algoritmo puede fallar?**
   [El ciclo `while` de la búsqueda binaria si se configuran mal los punteros de inicio y fin 
   (como usar `inicio = medio` en vez de `inicio = medio + 1`), lo cual causaría un bucle infinito.  
   También el uso de `==` para comparar los Strings de las estaciones en lugar de `.equals()`.]


3. **Como comprobare mi prediccion?**
   [Ejecutando el `BancoDePruebas` con tamaños de 1.000, 100.000 y 1.000.000 de lecturas, midiendo el 
   contador de comparaciones exactas que retorna cada algoritmo.]




## 3. Evidencia del laboratorio

### Resultado observado

[Al ejecutar la prueba con un millón de registros buscando el último elemento, la búsqueda lineal realizó 
exactamente 1.000.000 de comparaciones, mientras que la búsqueda binaria resolvió el mismo problema con apenas 
unas 20 comparaciones. Al probar la búsqueda binaria con PM2.5 (que no está ordenado), el algoritmo falló al no 
encontrar valores existentes.]

### Diferencia entre la prediccion y el resultado

[Los resultados coincidieron exactamente con la teoría de complejidad algorítmica. Lo único que varió 
ligeramente fue el tiempo en milisegundos debido a los procesos en segundo plano de la máquina virtual de Java, 
por lo que el conteo de operaciones lógicas (comparaciones) demostró ser una métrica mucho más estable.]



### Error o comportamiento inesperado

* **Que ocurrio?** [Al implementar la búsqueda binaria inicialmente, el ciclo se quedó congelado (loop infinito) 
al buscar elementos inexistentes.]


* **Por que ocurrio?** [Porque el índice `inicio` se quedaba estancado en la misma posición al actualizarse 
   con `inicio = medio` en lugar de avanzar con `inicio = medio + 1`.]
* 


* **Como lo corregimos o que falta corregir?** [Se corrigió ajustando correctamente los límites del intervalo 
(`inicio = medio + 1` y `fin = medio - 1`) para obligar al buscador a reducir el espacio de búsqueda en cada iteración.]



## 4. Explicacion en lenguaje llano

Buscar de forma lineal es como revisar un libro página por página desde la primera hasta encontrar la palabra que buscas; 
si el libro tiene un millón de páginas, te tardarás muchísimo. En cambio, la búsqueda binaria es como buscar una palabra 
en el diccionario abriéndolo siempre a la mitad: descartas 
instantly la mitad de las páginas que no te interesan, encontrando el dato en poquitos intentos.

### Ejemplo o analogia

[Imagínate adivinar un número del 1 al 100. Si vas preguntando uno por uno ("¿es el 1?, ¿es el 2?"), 
eso es búsqueda lineal. Si preguntas "¿es mayor o menor a 50?" y descartas mitades, eso es búsqueda binaria. 
La analogía deja de ser exacta cuando 
los datos están revueltos; si el diccionario estuviera desordenado, abrirlo a la mitad no te serviría de nada.]

## 5. El vacio que encontre

Al intentar explicar el tema, identifica el punto que aun no comprendes bien.

* **Mi duda concreta es:** [¿Qué estrategia es más eficiente si los datos se están escribiendo y 
consultando en tiempo real de forma masiva?]


* **Lo que ya puedo explicar es:** [Cómo funciona el comportamiento logarítmico de la búsqueda 
binaria y por qué el orden de los datos es una precondición obligatoria.]


* **Para resolver la duda consulte:** [La guía de la Semana 3 del proyecto integrador y las pruebas experimentales en el BancoDePruebas.]


* **Ahora lo entiendo asi:** [Que las estructuras de datos y los algoritmos no trabajan solos; 
un algoritmo rápido es inútil si no se cumplen las condiciones previas de los datos que manipula.]



## 6. Trazado de la solucion

Escoge una ejecucion, recorrido o caso representativo y trazalo paso a paso.
Incluye los valores importantes despues de cada paso.

| Paso | Estado de los datos o estructura | Decision o resultado |
| --- | --- | --- |
| 1 | Arreglo `[0, 1, 2, 3]`, buscando el valor `3`. `inicio=0`, `fin=3` | `medio = (0+3)/2 = 1`. `datos[1] = 1`. Como `1 < 3`, `inicio = medio + 1` (nuevo inicio = 2)

|
| 2 | Subarreglo activo `[2, 3]`. `inicio=2`, `fin=3` | `medio = (2+3)/2 = 2`. `datos[2] = 2`. Como `2 < 3`, `inicio = medio + 1` (nuevo inicio = 3)

|
| 3 | Subarreglo activo `[3]`. `inicio=3`, `fin=3` | `medio = (3+3)/2 = 3`. `datos[3] = 3`. Coincide con el objetivo. Retorna la posición 3.

|

**Completa o agrega filas si es necesario.** Si trabajaste con una estructura,
dibuja su estado en cada paso o inserta aqui una imagen legible.

## 7. Decision de diseño

Relaciona lo aprendido con la Plataforma de Monitoreo Ambiental Urbano.

* **Problema que debiamos resolver:** [Consultar lecturas de sensores de forma eficiente cuando el volumen de datos escala a un millón de registros.]


* **Estructura, algoritmo o estrategia elegida:** [Búsqueda binaria basada en la precondición de ordenamiento cronológico por timestamp.]


* **Alternativa descartada:** [Búsqueda lineal pura para todas las consultas intensivas del sistema.]


* **Por que elegimos la primera:** [Reduce drásticamente el costo computacional de $O(n)$ a $O(\log n)$, optimizando los recursos de la plataforma.]


* **Que evidencia respalda la decision:** [Los experimentos ejecutados con 1.000.000 de datos, donde la binaria requirió menos de 20 comparaciones frente al millón de la lineal.]



## 8. Aporte al proyecto

* **Archivo(s) o modulo(s) trabajado(s):** [`BuscadorLecturas.java`, `GeneradorDatos.java`, `BancoDePruebas.java`, `IngestaSensores.java`]


* **Cambio realizado:** [Implementación de algoritmos de búsqueda, generador de datos sintéticos a escala y estructuración del banco de experimentos conectados al único main.]


* **Como se conecta con la capa anterior:** [Toma el repositorio de lecturas y los arreglos estructurados en las semanas 1 y 2 para dotarlos de capacidades de consulta avanzada.]


* **Que queda pendiente para la siguiente semana:** [Analizar los algoritmos de ordenamiento para resolver el problema de las precondiciones en campos no ordenados.]



## 9. Commits realizados

Registra los commits que muestran tu aporte individual.

| Commit | Mensaje | Que demuestra |
| --- | --- | --- |
| `a82f31d` | `feat: implementa busqueda lineal y binaria`<br> | Creación de la lógica de búsqueda y conteo de comparaciones
|
| `7bc421a` | `feat: agrega generador de datos y banco de pruebas`<br> | Simulación de escenarios masivos y experimentos de eficiencia
|
| `5da120f` | `docs: documenta decisiones de diseno y precondiciones`<br> | Justificación arquitectónica y teórica de los algoritmos
|

## 10. Reexplicacion final

Despues del taller, vuelve a responder la pregunta de la semana en cinco lineas
o menos. Esta respuesta debe ser mas precisa que la de la seccion 4 y debe
incluir la razon de tu decision tecnica.

> Encontrar un dato entre un millón requiere cambiar de una búsqueda lineal $O(n)$ a una 
> binaria $O(\log n)$. Decidimos implementarla usando el timestamp porque su generación 
> natural mantiene el orden cronológico estricto, cumpliendo la precondición matemática 
> necesaria para reducir el costo operativo del sistema de forma radical.
>
>

## 11. Reflexion individual

Responde con honestidad:

1. **Lo que ahora puedo hacer y antes no podia:**
   [Medir y comparar la eficiencia real de un algoritmo usando conteo de operaciones y no solo percepciones de tiempo.]


2. **El error o supuesto que mas me enseno:**
   [Entender que un algoritmo perfecto (como la búsqueda binaria) arroja resultados erróneos 
   si no se respetan las precondiciones del estado de los datos.]


3. **La pregunta que llevaria a la proxima clase:**
   [¿Cuánto cuesta computacionalmente ordenar un arreglo desordenado para poder aplicarle búsqueda binaria?]


4. **Que parte del trabajo fue realmente mia:**
   [La resolución analítica de la traza, la estructuración de los experimentos y la integración limpia al único punto de entrada del sistema.]



## Lista de verificacion antes de entregar

* [x] Escribi la prediccion antes de consultar el resultado.
* [x] Inclui evidencia concreta del laboratorio.
* [x] Explique un concepto sin depender de jerga.
* [x] Registre un vacio, una duda o un error real.
* [x] Trace al menos un caso paso a paso.
* [x] Justifique una decision del proyecto y una alternativa descartada.
* [x] Registre mis commits y mi aporte individual.
* [x] Deje claro que queda pendiente.
* [x] Renombre el archivo con el formato `sXX-nombre.md`.


Pregunta 1 Una empresa tiene un millón de registros y realiza únicamente cinco búsquedas durante todo el día. ¿Tiene sentido diseñar toda la
estrategia de almacenamiento alrededor de una búsqueda binaria? ¿Qué otros costos o factores considerarías?Respuesta:
No, no tendría sentido sobrediseñar toda la arquitectura del sistema solo para optimizar búsquedas que casi no se hacen. 
Si el sistema realiza únicamente cinco consultas al día, el costo computacional, la complejidad del código y
el esfuerzo de mantener los datos ordenados permanentemente superan por completo el beneficio de usar una búsqueda binaria. 
Otros factores importantes a considerar son:   El costo de inserción/escritura: Mantener un arreglo o estructura ordenada
requiere procesos de ordenamiento o inserciones costosas cada vez que llegan nuevos datos de los sensores IoT. 
Si escribir los datos cuesta mucho más que buscarlos ocasionalmente, es mejor una estrategia de almacenamiento 
secuencial o sin ordenar.Recursos de memoria y mantenimiento: El esfuerzo humano y técnico de mantener el sistema 
sincronizado bajo estrictas precondiciones no justifica una ganancia de rendimiento imperceptible 
para solo cinco operaciones diarias.

Pregunta 2 Un algoritmo puede ser mucho más rápido que otro y, sin embargo, producir una respuesta incorrecta. 
¿Por qué consideras que la corrección debe analizarse antes que la eficiencia?

Respuesta: La corrección es el pilar fundamental de cualquier software porque un resultado rápido pero erróneo es completamente inútil 
(o incluso peligroso, especialmente en una red de monitoreo ambiental donde se toman decisiones basadas en datos reales). 
Analizar primero la 
corrección garantiza que el sistema cumpla con los requerimientos lógicos del negocio. Si optimizamos la eficiencia de un 
algoritmo que tiene fallas lógicas o no respeta sus precondiciones (como vimos al intentar aplicar búsqueda binaria en datos 
de PM2.5 desordenados), lo único que logramos es propagar errores a una velocidad masiva. Primero aseguramos 
que el sistema haga lo correcto, y solo después optimizamos qué tan rápido lo hace.   

Pregunta 3 Imagina que una plataforma consulta constantemente por timestamp, pero ocasionalmente necesita consultar por PM2.5.
¿Qué consecuencias tendría organizar los datos pensando principalmente en uno de estos campos?
Respuesta: Organizar los datos físicamente pensando en un solo campo optimiza de inmediato las consultas sobre ese atributo específico 
(gracias a la búsqueda binaria), pero penaliza severamente a los demás.   Si estructuramos y ordenamos todo el repositorio en función del timestamp, 
las consultas por este campo serán ultra rápidas ($O(\log n)$), lo cual es ideal para nuestra plataforma porque el flujo cronológico 
es vital. Sin embargo, como el PM2.5 queda completamente desordenado ante ese criterio, cualquier búsqueda frecuente 
o análisis centrado en los niveles de contaminación obligaría al sistema a recurrir a una búsqueda lineal ($O(n)$) o 
a reorganizar la estructura, encareciendo el costo operativo en esas consultas secundarias.  

Pregunta 4 Supón que tienes un conjunto de datos perfectamente ordenado y alguien modifica algunos registros sin 
conservar el orden. ¿Qué riesgos aparecen si el sistema continúa utilizando búsqueda binaria 
sin verificar las condiciones de los datos?

Respuesta: El riesgo principal es la pérdida de integridad y veracidad en los resultados.  
La búsqueda binaria asume matemáticamente que si el elemento del medio es menor que el objetivo, todo lo que está a la
izquierda puede descartarse de forma segura. Si el orden se rompe debido a una modificación mal hecha, el algoritmo 
puede "saltarse" el valor buscado al descartar erróneamente la mitad del arreglo donde realmente se encontraba.
Esto provocará que el sistema retorne un falso negativo (un -1 indicando que el dato no existe, 
cuando en realidad sí está en el repositorio), comprometiendo la estabilidad y confiabilidad de la plataforma.   

Pregunta 5 En ingeniería de software suele decirse: "Que funcione no significa que sea una buena solución." 
Relaciona esta afirmación con lo aprendido en las semanas 1, 2 y 3 del proyecto. ¿Qué ha cambiado en la manera 
en que analizas una solución desde que comenzó el proyecto?

Respuesta: Esta frase resume perfectamente la evolución
de nuestro pensamiento como ingenieros a lo largo de estas tres semanas:   En la Semana 1, nuestro único objetivo 
era lograr que el sistema recibiera y validara datos sin que estallara (el enfoque básico de "que funcione").   
En la Semana 2, entendimos que la forma en que organizamos esos datos en memoria (estructuras de almacenamiento y arreglos) 
importa para mantener la estabilidad.   En la Semana 3, dimos el salto crítico hacia la ingeniería real al introducir el
concepto de escala y eficiencia ($O(n)$ vs $O(\log n)$) y el respeto estricto por las precondiciones.   
Lo que ha cambiado es que ya no evaluamos un programa solo viendo si compila o arroja un resultado en nuestra máquina 
con diez datos de prueba; ahora analizamos cómo se comportará ese código cuando el sistema enfrente un millón de registros, 
cuánto cuesta computacionalmente mantenerlo y bajo qué restricciones lógicas y arquitectónicas opera de manera sostenible.   