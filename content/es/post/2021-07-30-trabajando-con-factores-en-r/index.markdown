---
title: Trabajando con factores en R
author: José Luis Texcalac Sangrador
date: '2021-07-30'
slug: trabajando-con-factores-en-r
categories: []
tags: []
subtitle: ''
summary: ''
authors: []
lastmod: '2021-07-30T01:53:01-05:00'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: true
projects: []
---


Una tarea recurrente cuando trabajamos en el procesamiento de datos, es la transformación de variables para ajustarlas al formato adecuado para su posterior análisis, tal es el caso de variables que pueden estar en formato numérico o de texto y que contienen datos de categorías que necesitamos procesar. R nos permite almacenar una variable con codificación de tipo **factor** que indica que los valores que la conforman son categorías, por ejemplo: sexo (hombre, mujer), estado civil(soltero, casado), etc. En este post intentaremos dar un vistazo simple a lo que esto significa.

&nbsp;

Cargamos las librerías a utilizar


```r
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
library(stringr)
```

Generemos una `tabla` de ejemplo usando una semilla para la reproducibilidad de 
resultados.


```r
set.seed(1234)

tabla <- 
  tibble(id = str_c("folio_", 1:10), 
         sexo = sample(c(1L, 2L), 10, replace = TRUE), 
         edo_civ = sample(c(1L, 2L, 3L), 10, replace = TRUE), 
         ent = sample(c("edo1", "edo2", "edo3"), 10, replace = TRUE)) %>% 
  print()
```

```
## # A tibble: 10 x 4
##    id        sexo edo_civ ent  
##    <chr>    <int>   <int> <chr>
##  1 folio_1      2       2 edo1 
##  2 folio_2      2       3 edo3 
##  3 folio_3      2       2 edo3 
##  4 folio_4      2       2 edo3 
##  5 folio_5      1       2 edo1 
##  6 folio_6      2       3 edo2 
##  7 folio_7      1       2 edo1 
##  8 folio_8      1       2 edo2 
##  9 folio_9      1       2 edo2 
## 10 folio_10     2       2 edo3
```

Demos un vistazo a la estructura de la `tabla`


```r
glimpse(tabla)
```

```
## Rows: 10
## Columns: 4
## $ id      <chr> "folio_1", "folio_2", "folio_3", "folio_4", "folio_5", "folio_…
## $ sexo    <int> 2, 2, 2, 2, 1, 2, 1, 1, 1, 2
## $ edo_civ <int> 2, 3, 2, 2, 2, 3, 2, 2, 2, 2
## $ ent     <chr> "edo1", "edo3", "edo3", "edo3", "edo1", "edo2", "edo1", "edo2"…
```

Las columnas *sexo*, *edo_civ* y *ent* contienen valores que categorizan la 
información, la tabla las tiene codificadas como numéricas. Generemos medidas de 
resumen de la `tabla` y veamos las implicaciones de considerarlas como numéricas


```r
summary(tabla)
```

```
##       id                 sexo        edo_civ        ent           
##  Length:10          Min.   :1.0   Min.   :2.0   Length:10         
##  Class :character   1st Qu.:1.0   1st Qu.:2.0   Class :character  
##  Mode  :character   Median :2.0   Median :2.0   Mode  :character  
##                     Mean   :1.6   Mean   :2.2                     
##                     3rd Qu.:2.0   3rd Qu.:2.0                     
##                     Max.   :2.0   Max.   :3.0
```

Como podemos observar, el resultado previo nos muestra los resultados de las 
columnas *sexo* y *edo_civ* como variables numéricas y a la columna *ent* como 
caracter. Sobra decir que resulta extraño tener cuartiles de la variable *sexo* 
cuando sólo contiene valores "1" y "2" que corresponden a "hombre" y "mujer" 
respectivamente, sería más conveniente si obtuvieramos las frecuencias para cada 
uno de los sexos codificados en la tabla.

Para resolver esto podemos hacer uso de la transformción de la columna de 
numérica a factor. Un factor en R otorga niveles a cada una de las categorías 
existentes en una variable, en el caso de la variable sexo de nuestro ejemplo, 
tenemos dos niveles que corresponden a los valores codificados como 1 y 2. 
Además, podemos especificar etiquetas para cada uno de los niveles de nuestro factor.

&nbsp;

**Generar un factor sin especificar niveles y etiquetas**

Podemos convertir una variable a factor sin necesidad de especificar los niveles 
que la conforman, sólo es necesario especificar dentro del comando _factor_ a la 
variable a transformar.

¿qué es lo que sucede?

R se encargará de identificar los distintos valores dentro de la columna y 
establecerá de forma automática cada uno como un nivel. En el ejemplo siguiente, 
convertimos a la variable _ent_ a tipo factor y la sobreescribimos bajo el mismo 
nombre en la tabla.


```r
tabla <- tabla %>% mutate(ent = factor(ent)) %>% print()
```

```
## # A tibble: 10 x 4
##    id        sexo edo_civ ent  
##    <chr>    <int>   <int> <fct>
##  1 folio_1      2       2 edo1 
##  2 folio_2      2       3 edo3 
##  3 folio_3      2       2 edo3 
##  4 folio_4      2       2 edo3 
##  5 folio_5      1       2 edo1 
##  6 folio_6      2       3 edo2 
##  7 folio_7      1       2 edo1 
##  8 folio_8      1       2 edo2 
##  9 folio_9      1       2 edo2 
## 10 folio_10     2       2 edo3
```

Nuevamente ejecutamos el comando _summary()_ y vemos el cambio en el resultado,
ahora obtendremos la frecuencia de cada uno de los niveles en la malla de datos.


```r
summary(tabla)
```

```
##       id                 sexo        edo_civ      ent   
##  Length:10          Min.   :1.0   Min.   :2.0   edo1:3  
##  Class :character   1st Qu.:1.0   1st Qu.:2.0   edo2:3  
##  Mode  :character   Median :2.0   Median :2.0   edo3:4  
##                     Mean   :1.6   Mean   :2.2           
##                     3rd Qu.:2.0   3rd Qu.:2.0           
##                     Max.   :2.0   Max.   :3.0
```

&nbsp;

**Generar un factor especificando los niveles**

También podemos generar un factor especificando los distintos niveles que lo
componen, utilicemos el caso de la variable *sexo* para ejemplificarlo. La
variable _sexo_ está codificada con valores 1 y 2, en consecuencia podemos 
especificar dichos niveles al momento de transformar la variable _sexo_ a factor.


```r
tabla <- 
  tabla %>% 
  mutate(sexo = factor(sexo, levels = c(1, 2))) %>% 
  print()
```

```
## # A tibble: 10 x 4
##    id       sexo  edo_civ ent  
##    <chr>    <fct>   <int> <fct>
##  1 folio_1  2           2 edo1 
##  2 folio_2  2           3 edo3 
##  3 folio_3  2           2 edo3 
##  4 folio_4  2           2 edo3 
##  5 folio_5  1           2 edo1 
##  6 folio_6  2           3 edo2 
##  7 folio_7  1           2 edo1 
##  8 folio_8  1           2 edo2 
##  9 folio_9  1           2 edo2 
## 10 folio_10 2           2 edo3
```

Nuevamente visualizamos el _summary_ de la tabla y veremos el resultado con las 
frecuencias para cada nivel


```r
summary(tabla)
```

```
##       id            sexo     edo_civ      ent   
##  Length:10          1:4   Min.   :2.0   edo1:3  
##  Class :character   2:6   1st Qu.:2.0   edo2:3  
##  Mode  :character         Median :2.0   edo3:4  
##                           Mean   :2.2           
##                           3rd Qu.:2.0           
##                           Max.   :3.0
```

&nbsp;

**Generar un factor especificando niveles y etiquetas**

También podemos generar factores especificando, además de los niveles, una
etiqueta para cada nivel, por ejemplo, la variable *edo_civ* codifica
valores 1, 2 y 3 que corresponden a "soltero", "casado" y "divorciado"
respectivamente. A partir de esta información, podemos transformar la variable
*edo_civ* a factor, especificando los niveles que contiene y sus respectivas
etiquetas.


```r
tabla <- 
  tabla %>% 
  mutate(edo_civ = factor(edo_civ, 
                          levels = c(1, 2, 3), 
                          labels = c("soltero", "casado", "divorciado"))) %>% 
  print()
```

```
## # A tibble: 10 x 4
##    id       sexo  edo_civ    ent  
##    <chr>    <fct> <fct>      <fct>
##  1 folio_1  2     casado     edo1 
##  2 folio_2  2     divorciado edo3 
##  3 folio_3  2     casado     edo3 
##  4 folio_4  2     casado     edo3 
##  5 folio_5  1     casado     edo1 
##  6 folio_6  2     divorciado edo2 
##  7 folio_7  1     casado     edo1 
##  8 folio_8  1     casado     edo2 
##  9 folio_9  1     casado     edo2 
## 10 folio_10 2     casado     edo3
```

Veamos nuevamente el *summary()* en dónde obtendremos, al igual que los casos
anteriores, las frecuencias de cada uno de los niveles. Note que ahora los
valores 1,2 y 3 fueron sustituidos de la tabla por las respectivas etiquetas.


```r
summary(tabla)
```

```
##       id            sexo        edo_civ    ent   
##  Length:10          1:4   soltero   :0   edo1:3  
##  Class :character   2:6   casado    :8   edo2:3  
##  Mode  :character         divorciado:2   edo3:4
```

Hasta aquí la gestión básica de factores...
