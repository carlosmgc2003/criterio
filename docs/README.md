# Corrección asistida por agentes con revisión docente

**Tipo de documento:** concepto de producto y requisitos iniciales — PRD
**Versión:** 0.5
**Fecha:** 2026-10-03
**Estado:** borrador para explorar y validar
**Alcance:** solución genérica para corregir ejercicios, exámenes y trabajos prácticos.

> **Nota de versión (0.5):** admite entregas individuales o grupales, siempre atribuidas a un alumno o grupo identificado; elimina los autores sin identificar.
> **Nota de versión (0.4):** establece la atribución unívoca de cada entrega y elimina las asociaciones dudosas.
> **Nota de versión (0.3):** resuelve las decisiones sobre tipos de evaluación prioritarios, criterio de cierre de entrega, información entregada al alumno y alojamiento de datos.
> **Nota de versión (0.2):** incorpora la visión del producto y enlaza los principios de funcionamiento con esa visión. El resto del documento mantiene la estructura y el alcance de la versión 0.1.

## Visión

La corrección no es un cuello de botella administrativo: es un acto docente. El sistema existe para proteger ese acto, no para reemplazarlo.

La visión del producto se sostiene en cuatro afirmaciones:

1. **No deshumanizar la corrección.** La corrección es un juicio humano y la calificación es responsabilidad del profesor. El sistema organiza, evidencia y propone; nunca decide. Un resultado sin revisión y aprobación docente es un borrador, no una calificación.
2. **Suplementar la devolución agéntica.** Los agentes se suman al trabajo del profesor, no lo sustituyen. Absorben la carga mecánica —recorrer entregas, localizar respuestas, aplicar criterios, ordenar hallazgos— para devolverle al docente el margen de criterio que solo él puede ejercer.
3. **Mantener el engagement profesor-alumno.** La devolución no es un veredicto: abre una conversación. El sistema debe mejorar el material con el que el profesor dialoga con el alumno, no interponerse entre ambos ni clausurar el intercambio.
4. **Mejorar la calidad de vida de los profesores al finalizar los semestres.** El cierre de semestre es el pico de carga y el punto donde se degrada la docencia. Aliviar ese pico de forma verificable es la métrica de éxito del producto.

### Cómo se relacionan

Las cuatro afirmaciones no son eslóganes sueltos: forman un solo argumento con una tensión interna que el sistema debe resolver.

La sobrecarga del cierre de semestre **(4)** empuja naturalmente hacia la automatización total. Ese atajo está vedado: automatizar el juicio **deshumaniza la corrección (1)** y **rompe el engagement (3)**. El único camino admisible es **suplementar (2)**: el agente absorbe lo mecánico y le devuelve al profesor el margen para ejercer su criterio. Con ese margen recuperado, el engagement vuelve a ser posible, y un docente presente corrige mejor, lo que a su vez hace sostenible el rol **(4)**.

```
sobrecarga de cierre ──presiona hacia──▶ automatizar todo
        │                                       │
        │                          prohibido por (1) y (3)
        ▼                                       ▼
calidad de vida (4) ◀──habilita── suplementar (2)
        │
        ▼
engagement (3) ──▶ corrige mejor ──▶ rol sostenible (4)
```

La tensión entre aliviar la carga y no deshumanizar no se resuelve eligiendo un bando, se resuelve con **diseño**: la frontera entre lo que el agente hace solo y lo que exige una decisión humana es el objeto central del sistema.

### Relación con los principios de funcionamiento

| Afirmación de la visión | Principios que la implementan |
|---|---|
| 1. No deshumanizar la corrección | 1 (Autoridad docente), 2 (Evaluación fundada), 3 (Originales preservados), 6 (Evidencia verificable), 8 (Criterios controlados) |
| 2. Suplementar la devolución agéntica | 4 (Cobertura explícita), 6 (Evidencia verificable), 8 (Criterios controlados) |
| 3. Mantener el engagement profesor-alumno | Se materializa en las secciones 7 y 9; se apoya en los principios 1 y 2 |
| 4. Mejorar la calidad de vida al cerrar el semestre | 4 (Cobertura explícita), 5 (Historial persistente), 7 (Alcance genérico); se mide en la sección 11 |

## 1. Idea del producto

Una aplicación que permita al profesor cargar una consigna, una rúbrica y las entregas de sus alumnos. El sistema organiza los materiales, propone unidades de revisión y utiliza agentes para analizar las respuestas.

El profesor recibe cada unidad con el material original, los criterios aplicables y las observaciones propuestas. Puede aceptar, modificar o descartar hallazgos, pedir evidencia adicional y agregar feedback propio.

La aplicación conserva un registro de qué analizaron los agentes, qué revisó el profesor y qué decisiones tomó. A partir de ese registro, prepara una devolución y una calificación para aprobación docente.

En términos de la visión: los agentes **suplementan** el trabajo de corrección, y toda salida permanece como propuesta hasta que el profesor la resuelve. La aplicación no reemplaza el juicio docente; le da soporte y trazabilidad.

## 2. Problema que busca resolver

Corregir una evaluación requiere identificar entregas, interpretar respuestas, aplicar criterios y redactar devoluciones. Cuando participan agentes, también hay que verificar sus observaciones y resolver posibles interpretaciones incorrectas.

Actualmente, esas tareas pueden quedar repartidas entre archivos, conversaciones y anotaciones. Esto dificulta:

- Saber qué partes de una entrega siguen pendientes.
- Distinguir una propuesta del agente de una conclusión docente.
- Retomar una corrección sin volver a leer todo.
- Descartar rápidamente un hallazgo y conservar el motivo.
- Aplicar un criterio de manera consistente entre alumnos.
- Incorporar el feedback humano a la devolución final.
- Reconstruir cómo se llegó a una calificación.

El problema se concentra y se vuelve crítico al finalizar el semestre, cuando el volumen de entregas coincide con el cierre de la cursada. En ese pico, la carga mecánica desplaza al trabajo pedagógico: las devoluciones se vuelven más breves y más parecidas, el acompañamiento se resiente y el vínculo docente-alumno se degrada. Ese deterioro no es un efecto secundario, es la razón por la que el producto existe.

## 3. Objetivos y principios

El producto busca reducir el trabajo de organización y repetición, facilitar la revisión humana y conservar evidencia suficiente para explicar cada conclusión.

Los principios siguientes operacionalizan la visión: acotan qué puede hacer el sistema por su cuenta y qué queda siempre en manos del profesor.

Principios de funcionamiento:

1. **Autoridad docente.** El profesor decide las interpretaciones, las observaciones que se comunican y la calificación final.
2. **Evaluación fundada.** Las conclusiones deben vincularse con la consigna, la rúbrica y la evidencia de la entrega.
3. **Originales preservados.** Las anotaciones y los análisis se almacenan separados del material recibido.
4. **Cobertura explícita.** Abrir un fragmento, analizarlo y revisarlo son acciones diferentes.
5. **Historial persistente.** Las decisiones conservan autor, fecha, alcance y versiones involucradas.
6. **Evidencia verificable.** Una inferencia estática y un resultado de ejecución se identifican como tales.
7. **Alcance genérico.** El modelo admite documentos, código, imágenes, procedimientos y otros tipos de respuesta.
8. **Criterios controlados.** Las aclaraciones del profesor quedan registradas; los agentes no agregan requisitos por su cuenta.

## 4. Usuarios y entradas

El usuario principal es el profesor que realiza la corrección. La colaboración entre varios docentes se contempla como una evolución del producto.

Entradas necesarias:

| Entrada | Contenido |
|---|---|
| Consigna | Preguntas, actividades, requisitos y entregables esperados |
| Rúbrica | Criterios, niveles de desempeño, pesos y reglas de calificación |
| Entregas | Respuestas y archivos de cada alumno o grupo |

Entradas opcionales: nómina de alumnos, respuesta de referencia, aclaraciones docentes y exportación de un campus virtual.

La identidad del alumno puede permanecer oculta durante la revisión si el profesor configura una corrección anónima.

## 5. Flujo de trabajo

**Carga → Preparación → Análisis → Revisión docente → Devolución y calificación**

### 5.1. Carga e inventario

El sistema registra los archivos recibidos, su procedencia y una huella del contenido para detectar duplicados y distinguir versiones.

Identifica cada entrega y la atribuye de forma unívoca a un alumno o grupo, a partir de la nómina o del origen de la carga. Toda entrega corresponde a un alumno o grupo identificado; no se contemplan autores sin identificar ni asociaciones dudosas. Debe contemplar:

- Varios archivos para una misma entrega.
- Entregas grupales.
- Versiones alternativas o reentregas.
- Archivos ilegibles, incompletos o mencionados pero ausentes.

### 5.2. Preparación de la evaluación

El sistema interpreta la consigna y la rúbrica para construir un mapa de actividades, requisitos, criterios y entregables.

Señala cuestiones que necesitan aclaración: criterios sin actividad asociada, pesos inconsistentes o instrucciones ambiguas. El profesor registra la interpretación que se aplicará.

Después localiza las respuestas de cada alumno y las relaciona con ese mapa.

**Una respuesta no localizada queda pendiente de verificación.** El sistema debe distinguir entre ausencia confirmada, problemas de lectura y dificultades para asociar el contenido.

La preparación produce un resumen con entregas identificadas, excepciones, respuestas localizadas y unidades propuestas. Las unidades listas pueden avanzar mientras se resuelven otras excepciones.

### 5.3. Análisis por agentes

Los agentes analizan las unidades contra los criterios correspondientes y registran:

- Evidencia de cumplimiento.
- Posibles errores o carencias.
- Fortalezas.
- Ambigüedades.
- Evidencia insuficiente.
- Preguntas que requieren criterio docente.

Los resultados quedan identificados como propuestas hasta que el profesor los resuelva.

### 5.4. Revisión docente

El profesor recorre una cola de unidades de revisión. Puede seguir el orden de la entrega, revisar por actividad o comparar un mismo criterio entre alumnos.

Cada unidad reúne el original, la consigna pertinente, los criterios aplicables, la evidencia y los hallazgos propuestos.

El profesor registra el alcance revisado y resuelve las observaciones.

### 5.5. Elaboración del resultado

El sistema prepara la devolución y la calificación usando las decisiones registradas. El profesor revisa y aprueba el resultado.

El registro de aprobación identifica las versiones de la entrega, los criterios y las decisiones utilizadas.

## 6. Unidades de revisión

Una **unidad de revisión** es un conjunto de material y contexto que permite al profesor resolver una pregunta de evaluación concreta.

La segmentación combina la estructura de la entrega con las reglas de la actividad:

| Tipo de ejercicio | Posible unidad |
|---|---|
| Examen teórico | Una respuesta y sus criterios |
| Ensayo | Un argumento, sus fuentes y su relación con la tesis |
| Matemática | Un procedimiento con resultado y justificación |
| Programación | Una clase, función o colaboración entre objetos |
| Proyecto | Una dimensión de la rúbrica que atraviesa varios archivos |

Cada unidad debe tener un propósito reconocible, por ejemplo: "evaluar si el procedimiento justifica el resultado" o "verificar cómo se implementa esta regla de dominio".

El profesor puede dividir, agrupar o reorganizar unidades. También puede expandir el contexto y acceder a la entrega completa.

Las unidades pueden compartir fragmentos. Las relaciones deben conservarse para evitar trabajo repetido y penalizaciones duplicadas.

## 7. Hallazgos y feedback docente

Un **hallazgo** es una observación propuesta sobre la entrega.

Debe contener:

- Identificador.
- Título y explicación.
- Criterio o requisito relacionado.
- Fragmento original y ubicación.
- Evidencia disponible y forma de obtención.
- Consecuencia observada o inferida.
- Sugerencia de mejora, cuando corresponda.
- Autor y versión del análisis.

### Decisiones sobre un hallazgo

| Acción | Resultado |
|---|---|
| Aceptar | Confirma la observación |
| Modificar | Registra una formulación o interpretación docente |
| Descartar | Conserva el hallazgo y el motivo de descarte |
| Pedir evidencia | Solicita una comprobación adicional |
| Dejar pendiente | Mantiene la cuestión abierta |

El descarte debe ser rápido: motivos frecuentes seleccionables, comentario opcional y atajos de teclado.

Ejemplos de motivos: interpretación incorrecta, requisito inexistente, duplicado, evidencia insuficiente o criterio docente.

Se pueden resolver varios hallazgos juntos, mostrando previamente el conjunto afectado. Las decisiones pueden deshacerse mediante una nueva acción registrada.

**Un análisis posterior debe respetar los descartes existentes.** Puede proponer reabrir una cuestión si cambió el material, el criterio o la evidencia, explicando el motivo.

### Feedback propio del profesor

El profesor puede agregar observaciones sobre fragmentos, unidades o la entrega completa, aunque ningún agente haya señalado un problema.

El feedback puede incluir errores, fortalezas, recomendaciones y preguntas para una defensa oral. Su autoría se conserva.

Este feedback es el espacio donde el engagement con el alumno se ejerce: la devolución puede reconocer logros, orientar una mejora y dejar preguntas abiertas, en lugar de limitarse a un puntaje.

Si un agente propone reformularlo, el profesor debe poder revisar esa propuesta antes de incorporarla al resultado aprobado.

## 8. Seguimiento y trazabilidad

El sistema mantiene dimensiones independientes de seguimiento:

| Dimensión | Qué registra |
|---|---|
| Recepción | Qué material se recibió y a quién pertenece |
| Preparación | Qué respuestas y unidades se localizaron |
| Análisis por agentes | Qué se analizó, con qué criterios y qué limitaciones |
| Revisión humana | Qué revisó el profesor y con qué alcance |
| Resolución | Qué ocurrió con cada hallazgo |
| Finalización | Qué devolución y calificación fueron aprobadas |

Abrir una unidad no la marca como revisada. Descartar un hallazgo tampoco confirma que se haya revisado toda la unidad.

El tablero debe permitir responder:

- ¿Qué sigue pendiente para este alumno?
- ¿Qué requisitos tienen evidencia?
- ¿Qué revisaron solamente los agentes?
- ¿Qué revisó el profesor?
- ¿Qué decisiones necesitan reconsiderarse?

Mantener esta trazabilidad es lo que permite retomar una corrección sin releerla por completo, y por lo tanto lo que sostiene el alivio de carga buscado al cerrar el semestre.

### Versiones e historial

Cada decisión se vincula con las versiones relevantes de la entrega, la consigna y la rúbrica. Cada análisis registra también modelo, instrucciones y configuración utilizados.

Cuando cambia alguno de esos elementos, el sistema identifica las revisiones potencialmente afectadas y las presenta para reconsideración.

El historial debe sobrevivir al cierre de la aplicación y a nuevas corridas de agentes. Los reintentos deben evitar duplicar hallazgos, decisiones o exportaciones.

## 9. Devolución y calificación

La devolución combina observaciones aprobadas, feedback docente, fortalezas y recomendaciones. Cada conclusión conserva su vínculo con la evidencia.

Los hallazgos descartados permanecen en el historial interno. Los borradores identifican cualquier cuestión pendiente que pueda afectar el resultado.

La calificación se obtiene aplicando la rúbrica a las evaluaciones registradas por criterio. Debe mostrar:

- Nivel o puntaje asignado.
- Peso y contribución a la nota.
- Evidencia y justificación.
- Ajustes docentes, cuando existan.

La cantidad de hallazgos no determina directamente la nota. Un mismo problema puede aparecer en varios fragmentos o criterios; su tratamiento depende de las reglas de evaluación.

La devolución está pensada como material para el diálogo docente-alumno: explica el porqué, reconoce fortalezas y orienta próximos pasos, en línea con el objetivo de mantener el engagement. Una salida limitada a un puntaje contradiría tanto la autoridad docente como ese vínculo.

El resultado se entrega por ejercicio: el alumno recibe una calificación y una devolución por cada ejercicio, según la estructura del examen que resolvió.

Cada exportación conserva una versión. Los formatos iniciales pueden ser Markdown y PDF.

## 10. Arquitectura tecnológica propuesta

La tecnología se considera una propuesta a validar durante el desarrollo.

| Componente | Propuesta | Responsabilidad |
|---|---|---|
| Interfaz | React + TypeScript | Presentar entregas, evidencia y decisiones |
| Backend | Python + FastAPI | Gestionar evaluaciones, estados y exportaciones |
| Base de datos | PostgreSQL | Guardar relaciones, decisiones e historial |
| Archivos | Almacenamiento compatible con S3 | Preservar originales y resultados derivados |
| Procesamiento | Workers Python | Extraer contenido y ejecutar análisis |
| Modelos | Adaptadores por proveedor | Permitir cambiar modelos sin alterar el registro de revisión |
| Coordinación | Flujo explícito; LangGraph si se necesita | Organizar dependencias y pausas |

### Procesamiento por tipo de material

Se propone una interfaz común para procesadores especializados. Todos producen fragmentos localizables y relaciones:

- Documentos: evaluar [Docling](https://docling-project.github.io/docling/) para extracción de estructura y OCR.
- Código: análisis sintáctico para identificar símbolos y colaboraciones.
- Imágenes y manuscritos: extracción asistida por modelos visuales, con verificación sobre el original.

Para presentar originales se consideran [PDF.js](https://mozilla.github.io/pdf.js/) y [Monaco Editor](https://github.com/microsoft/monaco-editor).

### Agentes

Las responsabilidades iniciales serían preparar la evaluación, analizar respuestas, contrastar observaciones y redactar el resultado. Pueden implementarse como tareas especializadas sin requerir un agente autónomo permanente para cada papel.

Los resultados deben seguir esquemas estructurados. Como opción, [Structured Outputs de OpenAI](https://developers.openai.com/api/docs/guides/structured-outputs) permite definir salidas mediante JSON Schema.

[LangGraph](https://docs.langchain.com/oss/python/langgraph/interrupts) se considera cuando hagan falta flujos con ramificaciones, persistencia y pausas. El registro docente permanece en el modelo de datos de la aplicación.

La ejecución de código entregado, si se incorpora, requiere un servicio aislado con límites de recursos y acceso.

## 11. Primera versión y validación

### Alcance inicial

La primera versión debe completar el recorrido con una evaluación y sus entregas:

1. Cargar consigna, rúbrica y archivos.
2. Identificar cada entrega y verificar su atribución unívoca al alumno o grupo.
3. Preparar y ajustar unidades de revisión.
4. Generar observaciones con evidencia localizable.
5. Aceptar, editar, descartar y agregar feedback propio.
6. Registrar cobertura de agentes y profesor.
7. Retomar una corrección conservando su estado.
8. Generar y aprobar una devolución y una calificación trazables.

Se propone empezar con documentos de texto, PDF digital y código fuente. Manuscritos, integraciones con campus y coordinación entre varios docentes pueden incorporarse después de validar el flujo principal.

### Validación con profesores

El piloto debe observar:

- Tiempo de preparación y corrección por entrega.
- Tiempo para resolver un hallazgo.
- Frecuencia y motivos de descarte.
- Calidad de la segmentación.
- Errores de asociación entre respuestas y consignas.
- Fidelidad con la que se incorpora el feedback humano.
- Capacidad para retomar la tarea sin releer toda la entrega.
- Percepción de carga y sostenibilidad del cierre de semestre.
- Calidad percibida del vínculo docente-alumno durante y después de la corrección.

Las metas cuantitativas se definirán con una línea de base de corrección manual.

## 12. Decisiones

### Decisiones resueltas

- **Tipos de evaluación y formatos prioritarios.** Se priorizan las evaluaciones semi-estructuradas, las preguntas a desarrollar y las tareas de programación que requieran revisar un código en base a requisitos. Estos casos son los que motivan el desarrollo. La extensión a otras materias queda planteada como evolución posterior.
- **Cierre de una entrega.** El profesor considera cerrada una entrega cuando acepta todas las unidades de revisión ("slides" de trabajo) que el agente propone, y aprueba la calificación y la devolución final para el alumno.
- **Información que recibe el alumno.** El alumno recibe una calificación y una devolución por ejercicio, de acuerdo con la estructura del examen que resolvió.
- **Alojamiento de datos.** Los datos se alojan en la base de datos y en almacenamiento de tipo blob.

### Decisiones pendientes

- ¿Cómo se presenta una revisión parcial o basada en muestreo?
- ¿Cómo se resuelven diferencias entre docentes?
- ¿Cómo se aplican aclaraciones de criterio a correcciones anteriores?
- ¿Qué política de conservación se aplica a los datos almacenados?
- ¿Qué presupuesto de procesamiento se admite por evaluación?
- ¿Qué integraciones y formatos de exportación necesita el primer grupo de profesores?

## 13. Hipótesis central

**Los agentes pueden reducir el trabajo de corrección si presentan evidencia organizada y propuestas fáciles de resolver, mientras la aplicación conserva las decisiones y el alcance real de la revisión docente.**

La validación del proyecto depende de comprobar que este flujo ahorra tiempo, mantiene la calidad, la consistencia y la autoría del feedback, y mejora la experiencia del profesor al cerrar el semestre sin deshumanizar la corrección ni deteriorar el vínculo con el alumno.
