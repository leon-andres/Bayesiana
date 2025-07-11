# Clasificador Bayesiano de Filtro de Spam

Autores: **Tomás Ávila** y **Andrés León**

---

## Análisis Descriptivo

El conjunto de datos consiste en mensajes que fueron o no etiquetados como spam. Se aplicaron análisis exploratorios para observar la distribución de las clases y características como la longitud de los mensajes.

### Frecuencias

![Distribución Spam vs Ham](figure-html/unnamed-chunk-3-1.png)

Vemos que el número de mensajes reportados como spam es bajo comparado con el número de mensajes que no fueron reportados. Esto se puede deber a la naturalidad de los mensajes o a una baja calidad del clasificador, lo cual verificaremos más adelante.

La siguiente tabla muestra el porcentaje de cada clase:

| label | count | percentage |
|-------|-------|------------|
| ham   | -   | -%      |
| spam  | -   | -%      |

### Longitud de los Mensajes

Se agregó una variable `message_length` que representa la cantidad de caracteres en cada mensaje.

![Longitud de los mensajes](figure-html/unnamed-chunk-4-1.png)

Es fácil evidenciar que uno de los factores clave al clasificar un mensaje, es su longitud. Mientras que los mensajes ham tienen un sesgo a la derecha, es decir, en su mayoría toman valores bajos. Los mensajes clasificados como spam alteran bastante la distribución con un claro sesgo negativo, es decir, toman valores muy altos para la longitud.
---

## Modelo de Regresión Logística

### Preprocesamiento

Se realizó una limpieza de los mensajes, con el fin de obtener valores más sencillos de manejar y con el fin de reducir la dimensionalidad. Se aplicaron los siguientes procesos:

- Conversión a minúsculas
- Eliminación de puntuación y números
- Eliminación de stopwords
- Aplicación de stemming
- Eliminación de espacios en blanco redundantes

Se construyó una matriz de términos (DTM), reducida con `removeSparseTerms` a una sparsidad del 0.999.

### Entrenamiento

Se dividieron los datos en 80% entrenamiento y 20% prueba. Se entrenó un modelo de regresión logística con todas las variables derivadas del texto:

```r
glm(label ~ ., data = train_set, family = "binomial")
```

---

## Resultados

### Matriz de Confusión

Se usó un umbral de 0.5 para clasificar los mensajes como `spam` o `ham`.

```r
confusionMatrix(data = predicted_labels, reference = test_set$label, positive = "spam")
```

Se evaluó el desempeño del modelo en términos de:

- Precisión
- Sensibilidad (Recall)
- Especificidad
- Accuracy

### AUC y Curva ROC

![Curva ROC](figure-html/unnamed-chunk-9-1.png)

El AUC obtenido fue de aproximadamente **0.98**, lo cual indica un excelente desempeño del modelo.

---

## Análisis de Resultados

- El modelo logra una separación clara entre spam y ham con base en el contenido textual.
- La longitud del mensaje parece ser una característica parcialmente informativa: los mensajes spam tienden a ser más largos.
- La limpieza del texto y reducción de dimensionalidad mejoraron la eficiencia del modelo.

---

## Conclusiones

- La regresión logística es una herramienta útil y eficaz para la clasificación de mensajes de texto como spam.
- A pesar de ser un modelo lineal, logra una alta precisión gracias a la representación adecuada del texto.
- Futuras mejoras podrían incluir:
  - Modelos no lineales (Random Forest, SVM)
  - Incorporación de embeddings semánticos como Word2Vec o BERT

---
