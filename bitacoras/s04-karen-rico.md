# Bitácora individual - Semana 4

## 1. Datos de la actividad

- **Estudiante:** Karen Manuela Rico Maldonado
- **Equipo:** los violentos
- **Semana:** 4
- **Fecha del taller:** 2026-10-05
- **Tema principal:** Algoritmos de ordenamiento simples y avanzados, complejidad temporal y espacial, y efectos colaterales en precondiciones[cite: 4, 6].
- **Pregunta de la semana:** Si la búsqueda binaria exige datos ordenados, ¿cuánto cuesta ordenar y qué consecuencias tiene sobre el resto de la plataforma?[cite: 4, 6]

## 2. Predicción antes de ejecutar

1. **¿Qué creo que va a ocurrir?**
   [Espero que los algoritmos simples como burbuja y selección realicen una cantidad masiva de comparaciones e 
    intercambios en comparación con los avanzados, y que QuickSort sufra o se desborde si se mantiene un pivote fijo 
    con los datos ordenados cronológicamente de la red][cite: 4, 6].

   2. **¿Qué parte del programa o del algoritmo puede fallar?**
   [El método de QuickSort con pivote fijo al recibir las lecturas sintéticas en orden cronológico, generando 
   particiones desbalanceadas y un error de desbordamiento de pila (`StackOverflowError`)][cite: 4, 6].

   3. **¿Cómo comprobaré mi predicción?**
   [Ejecutando el `BancoDeOrdenamiento` para contrastar métricas de comparaciones, 
   intercambios y milisegundos en los cinco experimentos][cite: 8].

## 3. Evidencia del laboratorio

### Resultado observado
[Se comprobó que Inserción es extremadamente veloz cuando los datos ya están ordenados 
(casi cero intercambios), mientras que MergeSort y HeapSort mantienen un crecimiento predecible y 
eficiente ($O(n \log n)$) a gran escala. QuickSort con pivote fijo colapsó con los 50.000 datos cronológicos
hasta que se aplicó la solución del pivote aleatorio. Además, el experimento 5 demostró de forma gráfica que 
ordenar por PM2.5 rompe de inmediato la precondición de la búsqueda binaria por timestamp][cite: 4, 6, 8].

### Error o comportamiento inesperado
- **¿Qué ocurrió?** [El `StackOverflowError` en QuickSort con el caso de datos cronológicos][cite: 4, 8].
- **¿Por qué ocurrió?** [Porque al elegir siempre el primer elemento como pivote con un arreglo ordenado,
se creaban subarreglos vacíos y se agotaba la pila de recursión][cite: 6].
- **Cómo lo corregimos?** [Introduciendo un pivote aleatorio antes de particionar para equilibrar las mitades][cite: 4, 5, 6].

## 4. Explicación en lenguaje llano
Ordenar datos es como organizar una biblioteca gigante: si intentas ordenar un libro a la vez revisando 
todo con métodos lentos, te gastas la vida. Los algoritmos avanzados dividen el trabajo inteligentemente. 
Sin embargo, cambiar el orden de los libros para complacer a una sección (como buscar por contaminación)
puede arruinarle el sistema a otra sección que necesitaba los libros organizados por hora.

## 5. El vacío que encontré
- **Mi duda concreta es:** [¿Cómo manejan las bases de datos de producción múltiples índices ordenados 
al mismo tiempo sin duplicar excesivamente la memoria de los registros?[cite: 6]]
- **Lo que ya puedo explicar es:** [Por qué el estado inicial de los datos (desordenados vs. cronológicos) 
cambia drásticamente el rendimiento de un algoritmo de ordenamiento][cite: 5, 6].

## 6. Trazado de la solución (Ejemplo: Inserción)
| Paso | Estado de los datos | Acción |
|---|---|---|
| 1 | `[3, 1, 4]` | Se selecciona el `1` como elemento actual |
| 2 | `[1, 3, 4]` | Se desplaza el `3` y se inserta el `1` a su izquierda por ser menor |
| 3 | `[1, 3, 4]` | El segmento queda completamente ordenado |

## 7. Decisión de diseño
- **Problema:** El ordenamiento por PM2.5 invalida el orden por timestamp requerido por la búsqueda binaria[cite: 4, 6].
- **Estrategia elegida:** Reconocer formalmente el efecto colateral y operar sobre copias o estructuras 
auxiliares cuando se requieran criterios múltiples[cite: 4, 6].
- **Justificación:** Protege la integridad lógica de las consultas del sistema[cite: 2, 6].

## 8. Aporte al proyecto
- **Módulos:** `Ordenador.java`, `BancoDeOrdenamiento.java`, `IngestaSensores.java`, `docs/decisiones.md`[cite: 4, 5, 8]
- **Cambio realizado:** Implementación de los 6 algoritmos de ordenamiento, resolución de los TODOs de optimización y estructuración del Hito 1[cite: 4, 5].

## 9. Commits realizados
| Commit | Mensaje | Qué demuestra |
|---|---|---|
| `feat` | `implementa ordenamientos y experimentos de semana 4`[cite: 5] | Creación de la lógica de ordenamiento y métricas de rendimiento[cite: 4, 5] |
| `docs` | `agrega bitacora y decisiones de semana 4 para H1`[cite: 5] | Justificación arquitectónica y cierre del Hito 1[cite: 4, 5] |

## 10. Reflexión individual
[Comprendí que la eficiencia no depende únicamente de escribir código rápido, sino de entender cómo interactúan 
los datos, las estructuras de control y las precondiciones entre los diferentes módulos de una plataforma de ingeniería][cite: 2, 6].
```[cite: 2, 4, 6]

---

### Paso 2: Preguntas de reflexión resueltas

Si te piden entregar las preguntas de reflexión de la guía de la semana resueltas, cópialas y guárdalas donde necesites:

1. **Explica la diferencia entre burbuja y selección sin usar las palabras “comparar” ni “intercambiar”.**
   * *Respuesta:* Burbuja evalúa de manera constante a parejas de elementos vecinos que van chocando y desplazándose
    paso a paso hasta ubicar al elemento más pesado en el extremo. Selección, en cambio, hace un barrido completo para 
    identificar de un solo golpe cuál es el valor mínimo global y trasladarlo directamente a su posición definitiva.

2. **¿En qué situación podría ser conveniente selección aunque realice muchas comparaciones?**
   * *Respuesta:* Cuando los objetos que estamos ordenando son muy pesados en memoria (por ejemplo, registros de
    objetos complejos de varios megabytes), ya que Selección se caracteriza por realizar una cantidad mínima de
    movimientos reales en comparación con los demás algoritmos simples.

3. **Si los datos llegan casi ordenados, ¿qué algoritmo resulta especialmente interesante y por qué?**
   * *Respuesta:* El algoritmo de Inserción, porque aprovecha que los elementos ya se encuentran en su sitio aproximado,
    reduciendo drásticamente su trabajo interno hasta comportarse de manera casi lineal ($O(n)$) en lugar de cuadrática.

4. **¿Qué enseña el `StackOverflowError` de QuickSort sobre confiar únicamente en el rendimiento promedio?**
   * *Respuesta:* Nos enseña que las estadísticas de "rendimiento promedio" de un algoritmo no sirven de nada si las 
   características de los datos reales del negocio (como llegar en orden cronológico estricto) chocan contra una mala 
   estrategia de selección de pivote.

5. **¿Por qué ordenar por PM2.5 rompe la búsqueda binaria por timestamp?**
   * *Respuesta:* Porque la búsqueda binaria no es mágica: exige como precondición estricta que los datos estén
    ordenados **exactamente** por el mismo criterio por el cual se va a consultar. Al alterar el orden por culpa
     del ranking de contaminación, el índice cronológico se destruye y la búsqueda binaria arroja resultados erróneos o no encuentra el dato.