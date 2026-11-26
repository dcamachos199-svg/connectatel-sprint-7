# ConnectaTel | Análisis de uso móvil

Proyecto 6 de TripleTen. ConnectaTel opera en México y Colombia y busca entender quién usa más las llamadas y los mensajes, qué casos requieren revisión y dónde conviene probar cambios en su oferta. El análisis cubre la actividad registrada entre el 1 de enero y el 30 de junio de 2024.

## Resultados principales

- De 40.000 eventos, **29.482** cumplen las reglas del análisis. **10.431** tenían fecha anterior al alta del usuario. Al aplicar el filtro a la misma cohorte con al menos 30 días observados, bajaron los totales registrados y las medianas por cuenta siguieron en 4 llamadas y 5 mensajes durante el período.
- Las medianas de llamadas y mensajes por 30 días fueron similares por edad, país y plan. El segmento de uso alto registró una mediana de **2,76 minutos más por 30 días** que el de uso bajo. Se marcaron **185 cuentas** para revisar por señales estadísticas de uso alto.
- Solo **5 de 14.271 cuentas-mes** (0,04 %) superaron un cupo observado. Los cinco casos fueron de minutos del plan Básico, sin excedentes repetidos. La conclusión se limita a voz y mensajes registrados. Conviene corregir los problemas de fechas y completar la cobertura antes de cambiar los planes.

## Archivos

| Archivo | Contenido |
|---|---|
| [`ConnectaTel.ipynb`](ConnectaTel.ipynb) | Diagnóstico, limpieza, integración, estadísticas, gráficos, detección de atípicos, segmentación y recomendaciones. Incluye las salidas de una ejecución completa. |
| [`requirements.txt`](requirements.txt) | Dependencias para ejecutar el notebook en un entorno local. |
| `data/` | Carpeta donde se guardan los tres CSV que proporciona TripleTen. El notebook registra su huella SHA-256. |

## Cómo reproducir el análisis

1. Abre el notebook en TripleTen para usar los archivos que están en `/datasets`. Para ejecutarlo en tu equipo, descarga los tres CSV de la sección «Datasets del Proyecto» de la lección. Crea `data/` junto al notebook y conserva los nombres originales.
2. Para ejecución local con Python 3.11 o posterior, abre una terminal en esa carpeta e instala las dependencias en un entorno virtual.

   ```bash
   python -m venv .venv
   source .venv/bin/activate
   python -m pip install -r requirements.txt
   jupyter lab ConnectaTel.ipynb
   ```

   En Windows PowerShell, usa `.venv\Scripts\Activate.ps1`. Si solo descargaste el notebook y este README, instala con `python -m pip install "pandas>=2.2,<4" "numpy>=1.26,<3" "matplotlib>=3.8,<4" "seaborn==0.13.2" "jupyterlab>=4,<5" "ipykernel>=6,<8"`.

3. Ejecuta **Restart Kernel and Run All Cells**. El notebook verifica las claves, las uniones y la reconciliación de eventos y totales. Si los CSV están en otra carpeta, define `CONNECTATEL_DATA_DIR` con esa ruta antes de iniciar Jupyter.

Para usar Google Colab, abre el `.ipynb`, carga los tres CSV al entorno y colócalos en `/content/data/`. Si tu carpeta de trabajo es distinta, agrega una celda inicial con `import os` y `os.environ['CONNECTATEL_DATA_DIR'] = '/content/data'`. Ejecuta el notebook de arriba a abajo. Usa las versiones indicadas en `requirements.txt` si tu entorno necesita actualizarse.

## Fuentes

- `plans.csv`, catálogo de planes, precios y beneficios.
- `users_latam.csv`, padrón de usuarios, fechas de alta y baja, ciudad y plan.
- `usage.csv`, eventos de llamadas y mensajes.
- [Enunciado del proyecto en TripleTen](https://tripleten.com/trainer/data-analyst/lesson/d9ea9659-1fff-4323-85b8-259a67b44602/).

## Cómo se construyeron las métricas

1. **Calidad de datos.** Se verifican las claves, se imputan las edades inválidas con la mediana y se marcan las ciudades y fechas fuera de rango. Las bajas en notación científica se convierten desde nanosegundos y se comparan a nivel de día. Los campos que no corresponden al tipo de evento se dejan vacíos.
2. **Integración y cobertura.** El análisis principal exige eventos fechados y coherentes con el alta y la baja. Una comparación de sensibilidad usa la misma cohorte sin exigir esa coherencia. Se segmenta el perfil con los cortes de la lección y se comparan tasas por 30 días para cuentas con al menos 30 días observados.
3. **Estadísticas y atípicos.** Se resumen llamadas y mensajes por evento y por cuenta. Se contrastan IQR y puntaje Z. Los extremos con formato válido se conservan y se señalan para revisión.
4. **Segmentación y cupos.** Se comparan edad, país derivado de la ciudad, plan e intensidad observada. La revisión de cupos usa cuentas con cobertura mensual completa entre enero y mayo. Junio se conserva en el análisis general.

## Límites

Los datos muestran actividad registrada, sin historial de cambios de plan, consumo de GB ni facturas. Se asume que la duración de llamada está en minutos. Las señales estadísticas orientan una revisión y no prueban fraude ni causalidad. Las tablas guardadas no incluyen nombres ni identificadores originales de usuarios.

Para compartir este análisis, basta con el notebook ejecutado y este README. Los archivos de entrada están disponibles en la lección.
