# criterio

Corrección asistida por agentes con revisión docente.

`criterio` es una aplicación para corregir exámenes, ejercicios y trabajos prácticos. El profesor carga la consigna, la rúbrica y las entregas de sus alumnos; el sistema las organiza en **unidades de revisión** y usa agentes para proponer **hallazgos** con evidencia. El profesor acepta, modifica o descarta cada propuesta, agrega su propio feedback y aprueba la calificación y la devolución final.

El principio que guía el diseño: **el agente propone, el profesor decide.** Ningún resultado se convierte en calificación sin revisión y aprobación docente, y la aplicación conserva el registro de qué analizaron los agentes, qué revisó el profesor y qué decidió.

## Estado

El proyecto está en **fase de definición de producto**. Todavía no hay código: el trabajo actual es cerrar los requisitos y validar la hipótesis central con profesores antes de construir.

| Documento | Versión | Contenido |
|---|---|---|
| [PRD](docs/README.md) | 0.8 | Visión, principios, flujo de trabajo, modelo de hallazgos, trazabilidad, arquitectura propuesta y alcance de la primera versión |

## Cómo leer el PRD

- **Visión (V1–V6):** por qué existe el sistema y cómo debe sentirse usarlo.
- **Principios (P1–P8, sección 3):** qué puede hacer el sistema por su cuenta y qué queda siempre en manos del profesor.
- **Flujo de trabajo (sección 5):** Carga → Preparación → Análisis → Revisión docente → Devolución y calificación.
- **Primera versión (sección 11):** el recorrido mínimo a construir y qué se mide en el piloto.
- **Decisiones (sección 12):** lo resuelto y lo pendiente, separando lo que bloquea la primera versión.

## Arquitectura propuesta

A validar durante el desarrollo (detalle en la sección 10 del PRD): interfaz en React + TypeScript, backend en Python + FastAPI, PostgreSQL para relaciones, decisiones e historial, almacenamiento compatible con S3 para originales y resultados, y workers en Python con adaptadores por proveedor de modelos.

## Licencia

[Apache 2.0](LICENSE).
