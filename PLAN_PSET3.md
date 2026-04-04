# Plan de trabajo - DM PSet 3

Este archivo es tu guia de ejecucion paso a paso para completar el PSet 3.

## Estado general

- [x] Diagnostico inicial del estado actual
- [x] Punto 1: Estructura base del proyecto
- [x] Punto 2: Variables de ambiente e infraestructura
- [x] Punto 3: Notebook 01 - Ingesta RAW
- [x] Punto 4: Notebook 02 - Enriquecimiento y unificacion
- [x] Punto 5: Notebook 03 - Construccion OBT
- [x] Punto 6: Notebook 04 - Validaciones y auditoria
- [ ] Punto 7: Notebook 05 - 20 preguntas de negocio (en progreso)
- [ ] Punto 8: README final y evidencias

## Punto 1 - Estructura base del proyecto (completado)

Objetivo: dejar lista la estructura de carpetas y archivos para construir todo sobre una base ordenada.

Checklist:

- [x] Crear `docker-compose.yml`
- [x] Crear `.env.example`
- [x] Crear `README.md`
- [x] Crear carpeta `notebooks/`
- [x] Crear 5 notebooks base:
  - [x] `notebooks/01_ingesta_parquet_raw.ipynb`
  - [x] `notebooks/02_enriquecimiento_y_unificacion.ipynb`
  - [x] `notebooks/03_construccion_obt.ipynb`
  - [x] `notebooks/04_validaciones_y_exploracion.ipynb`
  - [x] `notebooks/05_data_analysis.ipynb`
- [x] Crear carpeta `evidencias/`

## Siguiente paso cuando acabemos el punto 1

Pasar al punto 2:

- Definir variables de entorno obligatorias para Snowflake y parametrizacion de cargas.
- Conectar notebooks a esas variables (sin hardcodear credenciales).

## Punto 2 - Variables de ambiente e infraestructura (completado)

Objetivo: cumplir el requisito del PDF de manejar credenciales y parametros por variables de entorno, y dejar Compose listo para reproducibilidad.

Checklist:

- [x] Actualizar `docker-compose.yml` con `env_file: .env`
- [x] Exponer variables obligatorias del pipeline al contenedor
- [x] Completar `.env.example` con variables de Snowflake, fuente y parametros
- [x] Crear `.env` local editable (sin subir a GitHub)
- [x] Crear `.gitignore` para proteger secretos y artefactos locales
- [x] Conectar notebook `01_ingesta_parquet_raw.ipynb` a variables de entorno

Verificacion final (previo a punto 3):

- [x] `docker compose ps` muestra `pyspark-notebook` en estado `Up (healthy)`
- [x] Variables requeridas disponibles dentro del contenedor (`MISSING=[]`)
- [x] Parametros de rango/cobertura correctos (`2015-2025`, `yellow,green`, meses `01..12`)
- [ ] Guardar capturas en `evidencias/` (Compose, Jupyter, cfg, safe_preview)
- [ ] Rotar credenciales de Snowflake si fueron expuestas accidentalmente

## Siguiente paso

Pasar al punto 3:

- Implementar ingesta mensual 2015-2025 para yellow y green.
- Escribir en esquema `RAW` de Snowflake con metadatos de lineage.
- Registrar auditoria por lote e idempotencia.

## Punto 3 - Notebook 01 Ingesta RAW (completado)

Checklist de ejecucion:

- [x] Notebook `notebooks/01_ingesta_parquet_raw.ipynb` preparado con flujo end-to-end
- [x] Lectura parametrica desde `.env` (anios, meses, servicios, RUN_ID)
- [x] Descarga mensual de parquet con manejo de meses faltantes
- [x] Estandarizacion base yellow/green a un esquema comun RAW
- [x] Metadatos de lineage (`run_id`, `source_year`, `source_month`, `ingested_at_utc`, `source_path`)
- [x] Clave natural hash (`trip_nk`) para idempotencia base
- [x] Escritura a `RAW.TRIPS` y auditoria a `RAW.INGESTION_AUDIT`
- [x] Ejecutar backfill completo en Jupyter
- [x] Validar conteos por servicio/anio/mes en Snowflake
- [ ] Capturas de evidencia de corrida (logs + queries de verificacion)

## Punto 4 - Notebook 02 Enriquecimiento y unificacion (completado)

Checklist de ejecucion:

- [x] Implementar lectura de `RAW.TRIPS` desde Snowflake
- [x] Integrar Taxi Zones (PU/DO zone + borough)
- [x] Normalizar catalogos (`vendor`, `rate_code`, `payment_type`, `trip_type`)
- [x] Escribir resultado unificado en `RAW.TRIPS_UNIFIED`
- [x] Ejecutar notebook 02 completo en Jupyter
- [x] Validar conteos y nulos del dataset unificado
- [ ] Guardar capturas de evidencia de punto 4

## Punto 5 - Notebook 03 Construccion OBT (completado)

Checklist de ejecucion:

- [x] Implementar notebook `notebooks/03_construccion_obt.ipynb`
- [x] Construccion de `ANALYTICS.OBT_TRIPS` con columnas minimas del PDF
- [x] Derivadas implementadas (`trip_duration_min`, `avg_speed_mph`, `tip_pct`)
- [x] Inclusion de metadatos de lineage (`run_id`, `ingested_at_utc`, `source_service`, `source_year`, `source_month`)
- [x] Ejecutar notebook 03 completo en Jupyter
- [x] Validar conteos/calidad de OBT
- [ ] Guardar evidencias de construccion OBT

## Punto 6 - Notebook 04 Validaciones y auditoria (completado)

Checklist de ejecucion:

- [x] Implementar notebook `notebooks/04_validaciones_y_exploracion.ipynb`
- [x] Validaciones minimas de calidad (nulos esenciales, rangos, coherencia temporal)
- [x] Cobertura por servicio/anio/mes desde `ANALYTICS.OBT_TRIPS`
- [x] Auditoria de lotes desde `RAW.INGESTION_AUDIT` (status y performance)
- [x] Exploracion basica (top meses y boroughs)
- [x] Ejecutar notebook 04 completo en Jupyter
- [ ] Guardar evidencias de validaciones y auditoria

## Punto 7 - Notebook 05 Data analysis (en progreso)

Checklist de ejecucion:

- [x] Implementar notebook `notebooks/05_data_analysis.ipynb`
- [x] Incluir 20 consultas (a-t) usando `ANALYTICS.OBT_TRIPS`
- [ ] Ejecutar notebook 05 completo en Jupyter
- [ ] Agregar breve interpretacion (1-2 lineas) por pregunta en markdown
- [ ] Guardar evidencias y resultados para README
