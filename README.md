# Worksample SetUp — Service Centers ETL & Análisis con IA

Notebook de Google Colab que automatiza el seguimiento de aperturas de Service Centers en tres entregables: consolidación y transformación de datos con exportación a Excel, tablero interactivo en Looker Studio, y módulo de detección de riesgos operativos usando GPT-4.0

---

## Entregables

| # | Descripción | Output |
|---|---|---|
| 1 | Consolidación de datos con Python + exportación Excel | `Service_Centers_ETL.xlsx` en Google Drive |
| 2 | Tablero interactivo en Looker Studio | [Ver tablero](https://datastudio.google.com/u/0/reporting/2a866f6b-e9c8-42d4-bd43-63c66c0ebd59/page/FkmwF) |
| 3 | Módulo de IA para detección de riesgos operativos | Análisis generado por GPT-4.0 |

---

## Requisitos

### Librerías Python
```bash
pip install pandas openpyxl openai
```
En Google Colab pandas y openpyxl están preinstaladas. Solo openai puede requerir instalación: !pip install openai

### Acceso necesario
- Cuenta de Google con Google Drive
- Archivo Service_centers.csv guardado en la raíz de Google Drive
- API Key de OpenAI con saldo disponible

### Secrets de Google Colab requeridos

| Secret | Descripción |
|---|---|
| Api_GPT | API Key de OpenAI |
| Github | Token de acceso de GitHub |
| User | Nombre de usuario de GitHub |

---

## Estructura del archivo fuente

| Columna | Tipo | Descripción |
|---|---|---|
| id_sc | string | Identificador del Service Center |
| estado | string | Estado actual del proyecto |
| tipo_sc | string | Tipo de Service Center |
| avance_obra_civil_pct | float | % de avance en obra civil |
| avance_compras_pct | float | % de avance en compras |
| avance_licencias_pct | float | % de avance en licencias |
| fecha_estimada_apertura | string/date | Fecha estimada de apertura |

---

## Entregable 1 — Consolidación de Datos

### 1.1 Carga y transformación

- Cast de fecha: convierte fecha_estimada_apertura a formato YYYY-MM-DD
- Avance total: promedio horizontal de obra civil, compras y licencias
- Días de retraso: diferencia entre hoy y la fecha estimada, con mínimo de 0
- Resumen agrupado: agrupa por columnas clave y suma dias_retraso

### 1.2 Exportación a Excel formateado

- Encabezados con fondo gris oscuro (#434343) y texto blanco en negrita
- Filas alternadas en tonos amarillo claro (#FEF5D9 / #FEF0C7)
- Bordes delgados en todas las celdas
- Ancho de columnas ajustado automáticamente (máximo 40 caracteres)
- Formato numérico con 2 decimales y separador de miles

Nota: Se utilizó openpyxl para la exportación ya que gspread no estaba disponible por restricciones de APIs en el entorno.

---

## Entregable 2 — Tablero en Looker Studio

Visualización interactiva disponible en:
https://datastudio.google.com/u/0/reporting/2a866f6b-e9c8-42d4-bd43-63c66c0ebd59/page/FkmwF

---

## Entregable 3 — Módulo de IA para Detección de Riesgos

Envía df_resumen en formato JSON a GPT-4o. La API Key se obtiene de forma segura desde los Secrets de Colab:

Principales riesgos identificados:
- Retrasos en obtención de licencias (SC-002, SC-009)
- Progreso lento en obra civil y compras (SC-004, SC-011)
- Acumulación de retrasos en proyectos avanzados (SC-005, SC-010)

---

## Control de versiones con GitHub

Para subir cambios ejecuta desde Colab:

  git add Worksample_SetUp.ipynb
  git commit -m "descripción del cambio"
  git push

### Convención de mensajes de commit

| Prefijo | Uso |
|---|---|
| feat: | Nueva funcionalidad |
| fix: | Corrección de bug |
| refactor: | Mejora de código sin cambiar funcionalidad |
| docs: | Cambios en documentación |

---

## Archivos generados

| Archivo | Ubicación | Descripción |
|---|---|---|
| Service_Centers_ETL.xlsx | Raíz de Google Drive | Reporte consolidado con formato aplicado |
| Worksample_SetUp.ipynb | Repositorio GitHub | Notebook con los 3 entregables |
| Readme.md | Repositorio GitHub | Documentacion del procedimiento |
