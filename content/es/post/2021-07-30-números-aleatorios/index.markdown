---
title: Números aleatorios
author: José Luis Texcalac Sangrador
date: '2021-07-30'
slug: números-aleatorios
categories: []
tags: []
subtitle: ''
summary: ''
authors: []
lastmod: '2021-07-30T01:50:12-05:00'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
---

Una de las funciones posibles en R es la generación de números aleatorios, los resultados pueden concatenarse y almacenarse en forma de vector.

### Números aleatorios enteros

El comando **`sample`** nos permite seleccionar/generar números aleatorios enteros.


```r
aleatorios <- sample(25:40, 25, replace = T)
```

El primer argumento es un vector de números válidos a partir de los cuáles se generarán los valores aleatorios, en este caso van de 25 a 40. El segundo argumento indica el número de valores aleatorios a generar, que para este ejemplo serán 25 números aleatorios. El tercer argumento `replace = T` indica que cada valor aleatorio puede tener duplicados. Finalmente el resultado es guardado en el objeto de nombre `aleatorios`.

Revisamos el resultado


```r
aleatorios
```

```
##  [1] 38 35 27 35 27 40 32 36 39 34 33 34 36 37 29 33 30 37 32 32 26 37 37 39 36
```

### Generación aleatoria de valores de tipo carácter

También es posible seleccionar y/o generar valores aleatorios de tipo carácter, para ello necesitamos en el primer argumento un vector con datos válidos a seleccionar, como segundo argumento el total de valores a generar y definir si se permiten duplicados. 


```r
aleat_txt <- 
  sample(c("pera", "manzana", "guayaba", "papaya", "mango"), 25, replace = TRUE)
  aleat_txt
```

```
##  [1] "manzana" "manzana" "manzana" "mango"   "mango"   "papaya"  "manzana"
##  [8] "pera"    "mango"   "mango"   "manzana" "manzana" "papaya"  "pera"   
## [15] "mango"   "guayaba" "manzana" "pera"    "pera"    "papaya"  "papaya" 
## [22] "mango"   "papaya"  "pera"    "guayaba"
```

### Números aleatorios con decimales

El comando **`runif`** nos permite generar números aleatorios a partir de la distribución uniforme. Aquí un ejemplo para generar 25 números aleatorios entre 12.7 y 24.6


```r
aleat_unif <- runif(25, 12.7, 24.6)
aleat_unif
```

```
##  [1] 17.00063 22.67238 13.62301 18.07719 18.29160 18.50013 15.79959 19.68406
##  [9] 24.59555 20.07738 16.38827 13.20129 14.44109 16.39173 13.56270 14.76234
## [17] 21.79659 19.84469 16.29160 16.21104 19.08911 16.12468 21.39027 14.01321
## [25] 15.17195
```

Para redondear los resultados debemos agregar el comando `round` y especificar el redondeo a 1 dígito


```r
aleat_unif <- round(runif(25, 12.7, 24.6), 1)
aleat_unif
```

```
##  [1] 24.5 21.5 16.3 13.7 13.8 14.5 24.1 22.4 15.5 18.5 19.6 21.8 14.5 13.4 23.7
## [16] 22.1 20.4 23.6 15.8 14.6 18.9 13.1 22.9 22.6 17.0
```

El comando **`rnorm`** nos permite generar números aleatorios a partir de la distribución normal. Aquí un ejemplo para generar 25 números aleatorios con media 24.7 y desviación estándar de 4.3


```r
aleat_rnorm <- rnorm(25, 24.7, 4.3)
aleat_rnorm
```

```
##  [1] 18.78362 27.18988 19.76586 21.21905 21.60027 32.64012 27.73634 21.74829
##  [9] 29.33435 31.00413 19.58693 20.12818 21.71159 26.18978 19.96131 28.12992
## [17] 22.85746 22.09058 30.53628 20.68278 25.19788 17.47848 31.23702 23.46720
## [25] 23.33840
```

Para redondear podemos utilizar nuevamente el comando `round`


```r
aleat_rnorm <- round(rnorm(25, 24.7, 4.3), 1)
aleat_rnorm
```

```
##  [1] 25.8 28.1 24.2 21.0 24.8 18.0 14.0 28.1 22.1 24.2 36.7 24.4 30.7 18.8 21.7
## [16] 23.9 26.1 26.5 26.7 19.9 25.7 23.8 24.5 28.2 30.8
```


