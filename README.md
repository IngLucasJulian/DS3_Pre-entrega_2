# EDA de texto y pre-entrega: corpus preprocesado listo para modelado

Autor: Lucas Nahuel Julian

## Descripción

Pipeline de preprocesamiento y análisis exploratorio (EDA) sobre un corpus de noticias en inglés. El objetivo
es transformar el texto no estructurado en una estructura lista para modelado y validar su calidad mediante
análisis estadístico: distribución de longitudes, N-gramas, palabras frecuentes y distribución de clases.

Este corpus es la base de las siguientes pre-entregas y del Proyecto Final.

## Dataset

**AG News**: clasificación de noticias en inglés en 4 categorías: World, Sports, Business y Sci_Tech.

Archivos:

- `ag_news_train.csv`
- `ag_news_test.csv`

Columnas: `text` (título y primer párrafo de la noticia) y `label` (categoría).
Los splits train/test se mantienen tal como fueron provistos.

## Requisitos

- Python 3.10 o superior
- pandas, numpy, matplotlib, seaborn
- scikit-learn
- spaCy y el modelo `en_core_web_sm`
- transformers

Instalación:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn spacy transformers
python -m spacy download en_core_web_sm
```

## Ejecución

1. Copiar `ag_news_train.csv` y `ag_news_test.csv` en la carpeta `data/`.
2. Abrir `notebooks/NLP_ag_news_JulianLucas.ipynb`.
3. Ejecutar todas las celdas en orden.

El notebook genera el corpus preprocesado en `data/` y los gráficos del EDA (`hist_tokens.png`,
`distribucion_clases.png`, `top50_palabras.png`, `ngramas.png`).

## Pipeline de preprocesamiento

La función `preprocess_text(text)` integra:

1. **Limpieza con Regex**: eliminación de etiquetas y entidades HTML, URLs, emails, dominios web, firmas
   de agencias de noticias y números.
2. **Normalización**: conversión a minúsculas y eliminación de espacios redundantes.
3. **Lematización con spaCy** (`en_core_web_sm`): reducción de cada palabra a su forma base y filtrado de
   puntuación y stop-words.

Las negaciones (*no, not, never, nor, without, against*) se conservan, ya que modifican el sentido de la
oración.

El pipeline se aplica a la columna `text` de train y test.

## EDA técnico

- Histograma de cantidad de tokens por documento y percentil 95.
- Distribución de clases en train y test.
- Top 50 palabras más frecuentes y verificación de stop-words.
- Top 20 bigramas y top 20 trigramas.

## Resultados principales

| Métrica | Valor |
|---|---|
| Mediana de tokens por documento | 22 |
| Percentil 95 (`max_len`) | 31 tokens |
| Documentos que superan el `max_len` | 4,8% |
| Stop-words en el top 50 (texto crudo) | 38 de 50 |
| Stop-words en el top 50 (texto preprocesado) | 3 de 50 (*not, no, against*) |
| Balance de clases | 25% por clase en train y test |

Los N-gramas del corpus preprocesado corresponden a entidades y expresiones propias de cada categoría
(*new york, oil price, prime minister, boston red sox, wall street*), sin restos de HTML ni firmas de
agencias.
