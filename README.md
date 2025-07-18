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
| ham   | 4825   | 86.59368%      |
| spam  | 747   | 13.40632%      |

### Longitud de los Mensajes

Se agregó una variable `message_length` que representa la cantidad de caracteres en cada mensaje.

![Longitud de los mensajes](figure-html/unnamed-chunk-4-1.png)

Es fácil evidenciar que uno de los factores clave al clasificar un mensaje, es su longitud. Mientras que los mensaajes `ham` tienen un sesgo a la derecha, es decir, en su mayoría toman valores bajos. Los mensajes clasificados como `spam` alteran bastante la distribución con un claro sesgo negativo, es decir, toman valores muy altos para la longitud.

---

## Modelo de Regresión Logística

### Tokenización

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
- Sensibilidad
- Especificidad

### AUC y Curva ROC

![Curva ROC](figure-html/unnamed-chunk-9-1.png)

La curva comienza en el punto (0,0) y crece rápidamente hacia el vértice superior izquierdo (0,1), lo cual es muy bueno. Después se aplana un poco, lo cual indica que el clasificador mantiene buena sensibilidad sin perder demasiada especificidad.
La línea diagonal gris representa un clasificador aleatorio (sin capacidad predictiva), como la curva está consistentemente por encima de esta diagonal, se concluye que el modelo es mejor que un clasificador al azar.

El AUC obtenido fue de aproximadamente **0.98**, lo cual indica un excelente y eficaz desempeño del modelo.

---

## Conclusiones

El modelo de regresión logística, apoyado en una adecuada limpieza y transformación del texto, logró una clasificación precisa entre mensajes spam y no spam. El análisis descriptivo mostró diferencias claras en la longitud promedio de los mensajes entre ambas clases. El proceso de tokenización y reducción de la matriz de términos permitió construir un conjunto de variables predictoras eficiente, lo que se reflejó en métricas de desempeño destacadas como una alta exactitud y un AUC cercano a 0.98. En conjunto, los resultados validan tanto el enfoque estadístico como las decisiones de preprocesamiento empleadas en el desarrollo del clasificador.

---
