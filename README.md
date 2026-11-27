# Medición del énfasis temático en discursos presidenciales colombianos mediante modelos de lenguaje

Repositorio reproducible para el análisis temático y longitudinal del discurso político en español mediante modelos de lenguaje, embeddings semánticos, asignación de tópicos, evaluación mediante LLM-as-a-Judge y análisis temporal.

Este repositorio contiene los principales materiales metodológicos, datos procesados, prompts, matrices consolidadas, notebooks y resultados finales asociados al trabajo de investigación.

## Objetivo

El repositorio documenta el pipeline utilizado para:

1. preparar, normalizar y segmentar el corpus;
2. construir codebooks independientes mediante dos modelos de lenguaje;
3. comparar semánticamente dichos codebooks y consolidarlos en un Codebook Maestro;
4. asignar tópicos mediante diferentes estrategias de representación;
5. auditar las asignaciones mediante LLM-as-a-Judge;
6. comparar las estrategias y seleccionar la representación más adecuada;
7. refinar la valoración temática seleccionada;
8. analizar longitudinalmente la evolución de los tópicos.

El repositorio público contiene únicamente las **versiones finales o consolidadas** de cada etapa. Las versiones históricas, archivos de seguimiento, controles parciales y resultados intermedios permanecen en el entorno privado de trabajo.

---

## Pipeline general

```text
Corpus
  ↓
Preparación y segmentación
  ↓
Codebook ChatGPT ─────┐
                      ├──→ Comparación semántica
Codebook Claude ──────┘
                              ↓
                       Codebook Maestro
                              ↓
             ┌────────────────┼─────────────────┐
             ↓                ↓                 ↓
        Binaria (B)      Ordinal (O)      Embeddings (E)
             └────────────────┼─────────────────┘
                              ↓
                       Auditoría Judges
                              ↓
                    Comparación de métodos
                              ↓
                Selección del método ordinal
                              ↓
                  Reclasificación informada
                              ↓
                    Matriz ordinal O*
                              ↓
                    Análisis longitudinal
```

---

## Estructura del repositorio

```text
politica-discurso-llm-embeddings/
├── 01_corpus/
├── 02_codebooks_taxonomia/
├── 03_prompts/
├── 04_asignacion_topicos/
├── 05_evaluacion_validacion/
├── 06_analisis_temporal/
├── 07_notebooks/
├── 08_resultados/
├── 09_material_suplementario/
├── README.md
└── CITATION.cff
```

### `01_corpus/`

Contiene los 48 discursos utilizados en el estudio y los productos derivados de su preparación.

Incluye:

- fuentes originales;
- textos renombrados;
- textos en formato Markdown;
- metadatos;
- matriz de segmentación;
- segmentos discursivos individualizados.

Los identificadores documentales utilizados durante el procesamiento se conservan para mantener la trazabilidad entre el corpus y las etapas posteriores del pipeline.

### `02_codebooks_taxonomia/`

Contiene las versiones finales de los codebooks independientes construidos mediante ChatGPT y Claude, los productos utilizados para su comparación semántica y el Codebook Maestro final.

El archivo de referencia final es:

`Master_code_v3_sintetico.xlsx`

La taxonomía temática está incorporada en el Codebook Maestro mediante la organización de los tópicos, sus macrodimensiones y sus definiciones operacionales.

### `03_prompts/`

Contiene las versiones finales de los prompts utilizados durante las distintas etapas experimentales:

- construcción inductiva de los codebooks;
- asignación temática mediante hard prompting;
- valoración de intensidad mediante soft prompting;
- Judge 1;
- Judge 2;
- Judge 3;
- reclasificación informada.

Los prompts de construcción del codebook siguen una secuencia metodológica que incluye trazabilidad, configuración, inventario, inducción, depuración y representación para embeddings.

### `04_asignacion_topicos/`

Contiene los resultados consolidados de las estrategias iniciales de representación temática.

Las principales representaciones son:

- **B — Binaria:** asignación de presencia o ausencia temática mediante hard prompting.
- **O — Ordinal:** valoración de intensidad temática mediante una escala ordinal.
- **E — Embeddings:** representación basada en similitud coseno entre embeddings de segmentos discursivos y embeddings de tópicos.

### `05_evaluacion_validacion/`

Contiene los resultados consolidados del proceso de auditoría, comparación y refinamiento metodológico.

Incluye:

- respuestas del Judge aplicado a la representación binaria;
- respuestas del Judge aplicado a la representación ordinal;
- respuestas del Judge aplicado a la representación mediante embeddings;
- comparación entre las tres estrategias;
- reclasificación ordinal informada por las observaciones del Judge;
- segunda auditoría de la valoración ordinal refinada.

La matriz obtenida después del proceso de refinamiento corresponde a la representación:

**O\* — Matriz ordinal revisada.**

Los archivos de seguimiento por lote y los controles intermedios no forman parte del repositorio público.

### `06_analisis_temporal/`

Contiene los productos utilizados para estudiar longitudinalmente la intensidad y evolución de los tópicos.

Incluye:

- matriz ordinal revisada;
- agregaciones por periodo;
- matrices de promedios;
- tablas de distribución temática;
- tablas de comportamiento temporal;
- figuras longitudinales.

Cuando una misma figura dispone de una versión horizontal y otra vertical, se publica únicamente la **versión vertical**. Las figuras que poseen una única versión se conservan en su formato disponible.

### `07_notebooks/`

Contiene los notebooks necesarios para reproducir las principales etapas computacionales del estudio.

```text
07_notebooks/
├── 01_preparacion_corpus/
├── 02_construccion_codebooks/
├── 03_asignacion_topicos/
├── 04_evaluacion_validacion/
└── 05_analisis_temporal/
```

Los notebooks están organizados según el orden lógico del pipeline y no según su ubicación histórica dentro del entorno privado de trabajo.

### `08_resultados/`

Contiene una selección de las tablas y figuras finales utilizadas para interpretar y presentar los principales resultados de la investigación.

Su función es facilitar la consulta de los productos finales sin duplicar los archivos metodológicos conservados en las carpetas anteriores.

### `09_material_suplementario/`

Contiene únicamente material transversal que no posee una ubicación metodológica más apropiada en las carpetas anteriores.

Su principal producto es la tabla consolidada de configuración experimental:

`configuracion_experimental.xlsx`

Esta tabla documenta, según disponibilidad:

- etapa experimental;
- plataforma;
- modelo y versión;
- fecha de ejecución;
- identificador del prompt;
- organización en lotes;
- parámetros disponibles;
- modelo de embeddings;
- dimensión vectorial;
- normalización;
- regla de segmentación;
- longitud y solapamiento cuando corresponda;
- parámetros no verificables retrospectivamente.

Esta carpeta se mantiene deliberadamente reducida para evitar duplicación de archivos.

---

## Representaciones temáticas

El estudio utiliza cuatro matrices principales de resultados:

| Código | Representación | Descripción |
|---|---|---|
| **B** | Binaria | Presencia o ausencia temática obtenida mediante hard prompting |
| **O** | Ordinal | Intensidad temática obtenida mediante valoración ordinal |
| **E** | Embeddings | Representación derivada de la similitud semántica segmento–tópico |
| **O\*** | Ordinal revisada | Resultado de la reclasificación informada y posterior proceso de auditoría |

---

## Embeddings

La estrategia basada en embeddings utiliza representaciones semánticas tanto para los tópicos como para los segmentos discursivos.

El repositorio incluye:

- estructura integrada utilizada para representar los tópicos;
- niveles de representación utilizados para generar los embeddings;
- embeddings de segmentos;
- resultados de similitud coseno segmento–tópico;
- matrices consolidadas derivadas de dicha comparación.

---

## LLM-as-a-Judge

Las estrategias B, O y E fueron sometidas a procesos de auditoría mediante modelos de lenguaje utilizados como judges.

Las asignaciones de los judges corresponden a las respuestas obtenidas durante estas auditorías y constituyen los insumos utilizados para evaluar las asignaciones temáticas, identificar discrepancias, comparar las estrategias de representación, justificar la selección metodológica y orientar el proceso posterior de reclasificación.

---

## Correspondencia con los apéndices

| Apéndice | Contenido | Ubicación principal |
|---|---|---|
| **A** | Material metodológico para la construcción inductiva del codebook | `02_codebooks_taxonomia/` y `03_prompts/` |
| **B** | 48 discursos, metadatos, segmentos discursivos y Codebook Maestro | `01_corpus/` y `02_codebooks_taxonomia/` |
| **C** | Prompts de construcción, asignación, auditoría y reclasificación | `03_prompts/` y `05_evaluacion_validacion/` |
| **D** | Resultados B, O, E y O\*; estructura integrada para embeddings; embeddings de segmentos y respuestas de los judges | `04_asignacion_topicos/` y `05_evaluacion_validacion/` |
| **E** | Notebooks y registro de ejecución experimental | `07_notebooks/` y `09_material_suplementario/` |

---

## Criterios de publicación

Para favorecer la claridad, trazabilidad y reproducibilidad del repositorio se aplican los siguientes criterios:

- se publica únicamente la versión final o consolidada de cada producto;
- no se publican versiones históricas sustituidas;
- los archivos de seguimiento permanecen en el entorno privado de trabajo;
- no se duplican archivos entre carpetas;
- cada archivo posee una ubicación metodológica principal;
- se conservan los identificadores que forman parte del pipeline;
- los notebooks se concentran en `07_notebooks/`;
- las tablas y figuras finales de consulta se concentran en `08_resultados/`;
- las figuras duplicadas por orientación conservan únicamente su versión vertical;
- los parámetros experimentales no verificables se identifican explícitamente como tales.

---

## Reproducibilidad

La organización del repositorio permite seguir el flujo analítico desde la preparación del corpus hasta la generación de los resultados longitudinales.

El orden general de reproducción es:

```text
01_corpus
   ↓
02_codebooks_taxonomia
   ↓
03_prompts
   ↓
04_asignacion_topicos
   ↓
05_evaluacion_validacion
   ↓
06_analisis_temporal
```

Los notebooks que implementan computacionalmente estas etapas se encuentran organizados en `07_notebooks/`.

---

## Citación

La información estructurada para citar este repositorio estará disponible en:

`CITATION.cff`
