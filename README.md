---
title: "CASO 16 DISTRIBUCCION BINOMIAL"
author: "ANGELICA ITZEL LOPEZ DELGADO"
date: '2022-04-28'
output: html_document: 
    toc: yes
    toc_depth: 5
    code_folding: hide
    toc_float: yes
    number_sections: yes
bibliography: references.bib
editor_options: 
  markdown: 
    wrap: 72
---

# Objetivo

Encontrar probabilidades de acuerdo a la distribución binomial.

# Descripción

Se identifican ejercicios casos de la literatura de **distribuciones de probabilidad binomial** y se realizan cálculos de probabilidades, se determinan el valor esperado y se calcula la varianza y la desviación.

Los ejercicios que se presenta utilizan funciones relacionadas con la distribución binomial *dbinom() pbinom(), rbinom()* en algunos ejercicios del caso se utiliza la función *f.prob.binom()* previamente codificada y que encapsula la fórmula para determinar probabilidad binomiales.

# Fundamento teórico

El experimento de lanzar al aire una moneda es un ejemplo sencillo de una importante variable aleatoria **discreta** llamada variable aleatoria binomial. Muchos experimentos prácticos resultan en datos similares a que salgan cara o cruz al tirar la moneda [@mendenhall_introduccion_2006]

Un experimento binomial es el que tiene estas cinco características:

-   El experimento consiste en $n$ intentos idénticos.

-   Cada intento resulta en uno de dos resultados, el resultado uno se llama éxito, 'S', y el otro se llama fracaso, 'F'.

-   La probabilidad de éxito en un solo intento es igual a $p$ y es igual de un intento a otro. La probabilidad de fracaso es igual a $q= (1 - p)$.

-   Los intentos son independientes.

-   El interés es el valor de $x$, o sea, el número de éxitos observado durante los $n$ intentos, para $x = 0, 1, 2, …, n.$ [@mendenhall_introduccion_2006].

Un experimiento de Bernoulli puede tener como resultado un éxito con probabilidad $p$ y un fracaso con probabilidad $q = 1 − p$. Entonces, la distribución de probabilidad de la variable aleatoria binomial $x$, el número de éxito $k$ en $n$ ensayos independientes [@walpole_probabilidad_2012]:

Fórmula:

$$prob(x=k) = \binom{n}{k} \cdot p^{k} \cdot q^{(n-k)} $$ Para $$x = 0,1,2,3...n$$ y recordando las combinaciones cuantos éxitos $k$ en $n$ ensayos.$$\binom{n}{k} = \frac{n!}{k!\cdot(n-k)!}$$

El valor esperado está dado por: $$\mu = n \cdot p$$

La varianza y la desviación estándar se determinan mediante: $$\sigma^{2} = n \cdot p \cdot(1-p)$$ y $$\sigma = \sqrt{\sigma^{2}}$$

En programación R, para calcular la función de probabilidad binomial para un conjunto de valores discretos, $x$, un número de ensayos $n$ y una probabilidad de éxito $p$ se puede hacer uso de la función *dbinom()*.

De semejante forma, para calcular la probabilidad acumulada de una distribución binomial se puede utilizar la función *pbinom() o* para calcular la probabilidad de que una variable aleatoria $x$ que sigue una distribución binomial tome valores menores o iguales a $x$ puedes hacer uso de la función *pbinom()* [@rcoderbinom].

En los siguientes ejercicios también se utilizan funciones de paqutes base de R para la comprensión de la distribución binomial.

![](images/funciones%20binomial%20en%20R.jpg){width="400"}

[@rcoder]

[@statology2019]

# Desarrollo

## Cargar librerías

```{r message=FALSE, warning=FALSE}
library(dplyr)
library(ggplot2)
library(mosaic) # Gráficos de distribuciones
options(scipen=999) # Notación normal
# options(scipen=1) # Notación científica
```

## Cargar funciones

Se carga función de servicio github o de manera local

```{r message=FALSE, warning=FALSE}
# source("../funciones/funciones.para.distribuciones.r")
# o
source("https://raw.githubusercontent.com/rpizarrog/probabilidad-y-estad-stica/master/Enero%20Junio%202022/funciones/funciones.para.distribuciones.r")
```

Se determina una semilla porque algunos ejercicios calculan valores aleatorios.

```{r}
set.seed(2022)
```

## Ejercicios

### Tienda de ropa MartinClothingStore

Tienda de ropa MartinClothingStore [@anderson_estadistica_2008]

De acuerdo con la experiencia, el gerente de la tienda estima que la probabilidad de que un cliente realice una compra es 0.30 o 30%

-   Identificar las probabilidad para cuando se compre 0,1,2,3, determinar la tabla de probabilidad incluyendo probabilidad acumulada

-   Encontrar la probabilidad de que compren dos clientes

-   Encontrar la probabilidad de que compren los tres próximos clientes.

-   Encontrar la probabilidad de que sean menor o igual que dos.

-   Calcular la probabilidad de que sean mayor que dos

-   Determinar el valor esperado y su significado

-   Determinar la varianza y la desviación estándar y si significado

-   Interpretar

#### Probabilidad para 0,1,2,3 y tabla de distribución

Identificar las probabilidad para cuando se compre 0,1,2,3, determinar la tabla de probabilidad incluyendo probabilidad cumulada

-   Inicializar valores

```{r}
x <- c(0,1,2,3)
n <- 3
exito <- 0.30
```

-   Determinar tabla de probabilidad usando la función creada y conforme a la fórmula

```{r}
tabla1 <- data.frame(x=x, f.prob.x = f.prob.binom(x,n,exito), f.acum.x = cumsum(f.prob.binom(x,n,exito)))
tabla1
```

-   Determinar tabla de probabilidad usando función propia de los paquetes base de R *dbinom()*

```{r}
tabla2 <- data.frame(x=x, f.prob.x = dbinom(x = x, size = n, prob = exito), f.acum.x = cumsum(dbinom(x = x, size = n, prob = exito)))
tabla2
```

con *pbinom()* en lugar de *cumsum()*

```{r}
tabla3 <- data.frame(x=x, f.prob.x = dbinom(x = x, size = n, prob = exito), f.acum.x = pbinom(q = x, size = n, prob = exito))
tabla3
```

#### Visualizar tabla de distribución

```{r}
plotDist(dist = "binom", size=3, prob=0.30,xlab = paste("Variables ",min(tabla1$x),"-",max(tabla1$x) )) 
plotDist(dist = "binom", size=3, prob=0.30,xlab = paste("Variables ",min(tabla1$x),"-",max(tabla1$x) ), kind = "histogram") 
```

#### Probabilidad de que compren dos clientes

Encontrar la probabilidad de que compren dos clientes

-   Identificar la probabilidad cuando $P(x=2)$ de la tabla.
-   Se puede usar tabla1, tabla2 o tabla3 es la misma.

```{r}
valor.x <- 2
la.probabilidad <- filter(tabla1, x == valor.x) 
la.probabilidad
paste("La probabilidad cuando x es ", valor.x, " es igual a : ", la.probabilidad$f.prob.x )
```

Usando *dbinom()*

```{r}
dbinom(x = 2, size = 3, prob = exito)
```

#### Probabilidad de que compren los tres próximos clientes

Encontrar la probabilidad de que compren los tres próximos clientes

-   Identificar la probabilidad cuando $P(x=3)$ de la tabla.
-   Se puede usar tabla1, tabla2 o tabla3 es la misma.

```{r}
valor.x <- 3
la.probabilidad <- filter(tabla1, x == valor.x) 
la.probabilidad
paste("La probabilidad cuando x es ", valor.x, " es igual a : ", la.probabilidad$f.prob.x )
```

Usando *dbinom()*

```{r}
dbinom(x = 3, size = 3, prob = exito)
```

#### Probabilidad de que sean menor o igual que dos

Encontrar la probabilidad de que sean menor o igual que dos

-   Ahora usar la función acumulada por la pregunta
-   $P(x=0) + P(x=1) + P(x=2)$

```{r}
valor.x <- 2
la.probabilidad <- filter(tabla1, x == valor.x) 
la.probabilidad
paste("La probabilidad de que sea menor o igual a ", valor.x, " es igual a : ", la.probabilidad$f.acum.x )
```

Usando *pbinom()*

```{r}
pbinom(q = 2, size = 3, prob = exito)
```

#### Probabilidad de que sean mayor que dos

La expresión *lower.tail = FALSE como atributo de la función pbinom()* significa encontrar en la tabla de distribución la sumatoria de las probabilidades a partir de el valor de $x$, o lo que es lo mismo, $1 - prob.acum(x)$, $1 - 0.97 = 0.27$.

```{r}
pbinom(q = 2, size = 3, prob = exito, lower.tail = FALSE)
```

#### Valor esperado

Determinar el valor esperado y su significado

-   El valor esperado de la distribución binomial

$$\mu = n \cdot p$$ Siendo $p$ el éxito de la probabilidad y $n$ el número de experimentos

```{r}
VE <- n * exito
paste ("El valor esperado es: ", VE)
```

El valor esperado $VE$ significa el valor medio o el valor promedio de todos valores de la distribución de probabilidad.

#### Varianza y desviación estándar

Determinar la varianza y la desviación estándar y su significado.

-   La varianza en la distribución binomial $$\sigma^{2} = n \cdot p \cdot(1-p)$$

```{r}
varianza <- n * exito *( 1 - exito)
paste ("La varianza es: ", round(varianza,2))
```

-   La desviación $$\sigma = \sqrt{\sigma^{2}}$$

```{r}
desviacion.std <- sqrt(varianza)
paste("La desviación std es: ", round(desviacion.std, 2))
```

#### Interpretar el ejercicio

Pendiente ...

### Jugador de basquetbol

Un jugador encesta con probabilidad 0.55. [@noauthor_distribucion_nodate]:

-   Determinar las probabilidad de los tiros del 1 al 6 con la tabla de probabilidad

-   Determinar la probabilidad de encestar cuatro tiros $P(x=4)$

-   Determinar la probabilidad de encestar todos tiros o sea seis $P(x=6)$

-   Determinar la probabilidad de encestar al menos tres $P(x \leq 3)$ o, $P.acum(x = 3)$

-   Determinar el valor esperado VE

-   Determinar la varianza y su desviación estándard

-   Interpretar el ejercicio

#### Tabla de probabilidad (0-6)

Se construye la tabla de probabilidades tal y como se construye usando el código de *tabla3*

Se inicializan valores:

```{r}
x <- 0:6
n <- 6
exito <- 0.55
```

```{r}
tabla <- data.frame(x=x, f.prob.x = dbinom(x = x, size = n, prob = exito), f.acum.x = pbinom(q = x, size = n, prob = exito))
tabla
```

#### Visualización de probabilidades

Dos formas de visualizar las probabilidades

```{r}
plotDist(dist = "binom", size=n, prob=exito,xlab = paste("Variables ",min(tabla$x),"-",max(tabla$x) )) 
plot(x = tabla$x, y=tabla$f.prob.x, type = "h", xlab = paste(min(tabla$x), '-', max(tabla$x)), ylab= "f(x)")
```

#### Probabilidad de encestar cuatro tiros

Calcular la probabilidad de encestar cuatro tiros $P(x=4)$

```{r}
dbinom(x = 4, size = n, prob = exito)
```

#### Probabilidad de encestar todos los tiros

Determinar la probabilidad de encestar todos tiros o sea seis $P(x=6)$

```{r}
dbinom(x = 6, size = n, prob = exito)
```

#### Probabilidad de encestar al menos tres

Usando la función pbinom()

```{r}
pbinom(q = 3, size = n, prob = exito)
```

o utilizando el renglón de la tabla de distribución en la columna de probabilidad acumulada *f.acum.x*.

```{r}
valor.x <- 3
la.probabilidad <- filter(tabla, x == valor.x) 
la.probabilidad
```

#### Valor esperado

```{r}
VE <- n * exito
paste("El valor esperado es: ",VE)
```

El valor esperado de `r VE` significa que es lo que se espera encestar en promedio de los $n=$ `r n` tiros.

#### Varianza y desviación

Varianza

```{r}
varianza <- n * exito *( 1 - exito)
paste ("La varianza es: ", round(varianza,2))
```

Desviación

```{r}
desviacion.std <- sqrt(varianza)
paste("La desviación std es: ", round(desviacion.std, 2))
```

De el valor esperado `r VE` hay una desviación aproximada de `r desviacion.std` hacia arriba o hacia abajo.

### Recuperación de un paciente

La probabilidad de que un paciente se recupere de una rara enfermedad sanguínea es $0.4$. Si se sabe que $15$ personas contraen tal enfermedad,

-   Determine tabla de probabilidad de 1 al 15

-   Visualizar la gráfica de probabilidades

-   ¿Cuál es la probabilidad de que sobrevivan al menos diez,

-   ¿Cuál es la probabilidad de que sobrevivan de tres a ocho?, y

-   ¿Cuál es la probabilidad de que sobrevivan exactamente cinco?

-   ¿Cuál es el valor esperado 'VE' o la esperanza media?

-   ¿Cual es la varianza y la desviación estándar?

-   ¿Cómo se comportarían las probabilidades para un experimento de 100 personas?

-   Interpretación del ejercicio [@walpole_probabilidad_2012].

#### Tabla de distribución

Inicializar valores

```{r}
x <- 0:15
n <- 15
exito <- 0.40
```

Se construye la tabla de probabilidades con las funciones construidas que se encuentran en enlace citado al principi del documento y con la función *cumsum()* para el acumulado de la probabilidad.

```{r}
tabla <- data.frame(x=x, f.prob.x = f.prob.binom(x,n,exito), f.acum.x = cumsum(f.prob.binom(x,n,exito)))
tabla
```

#### Gráfica de probabilidades

La gráfica se presenta con la función *plot()* que requiere las coordenadas de x & y siendo estás las variables aleatorias discretas y las probabilidades respectivamente.

```{r}
plot(x = tabla$x, y=tabla$f.prob.x, type = "h", xlab = paste(min(tabla$x), '-', max(tabla$x)), ylab= "f(x)")
```

#### Probabilidad de que sobrevivan al menos diez

Se requiere la suma de las probabilidades endonde $P(\leq 10)$ o bien $P(x=0) + P(x=1) + P(x=2) ... + P(x=10)$ o mediante la función acumulada de la probabilidad.$F(x=10)$. Como se necesita la probabilidad acumulada entonces se usa *pbinom()*.

```{r}
x = 10
prob <- pbinom(q = x, size = n, prob = exito)
paste ("La probabilidad de que se enfermen menos que diez es: ", prob, " o el ", round(prob * 100, 2), "%") 
```

#### La probabilidad de que sobrevivan de tres a ocho

Se requiere el valor acumulado entre tres y ocho es decir, $F(x=8) - F(x=2)$ , o sumar las probabilidades de tres a ocho $P(x=3) + P(x=4) + P(x=5) + P(x=6)+ P(x=6)+P(x=7)+P(x=8)$

Se usa la resta usando la función *pbinom()*

```{r}
x1 = 2  #
x2 = 8
prob <- pbinom(q = x2, size = n, prob = exito) - pbinom(q = x1, size = n, prob = exito) 
paste ("La probabilidad de que se enfermen de tres a ocho es: ", prob, " o el ", round(prob * 100, 2), "%")
```

Se comprueba sumando las probabilidades de tres a ocho

```{r}
sum(dbinom(x = 3:8, size = n, prob = exito))
```

o sumando los renglones de las probabilidades de tres a ocho de la tabla de probabilidad.

```{r}
sum(filter(tabla, x %in% 3:8) %>%
  select(f.prob.x))
```

#### La probabilidad de que sobrevivan exactamente cinco

Aquí se calcula la probabilidad con la función *dbinom()* cuando $P(x=5)$

```{r}
x = 5
prob <- dbinom(x = x, size = n, prob = exito)
paste ("La probabilidad de que se enfermen menos que diez es: ", prob, " o el ", round(prob * 100, 2), "%") 
```

Se comprueba la probabilidad extrayendo con la función *filter()* el registro de la tabla de distribución cuando $x==10$.

```{r}
filter(tabla, x==5)
```

#### Valor esperado

Se determina el valor medio o el valor esperado de la tabla de distribución.

```{r}
VE <- n * exito
paste("El valor esperado es: ",VE)
```

Se espera que se recuperen `r VE` en promedio

#### Varianza y desviación

Se calcula la varianza

```{r}
varianza <- n * exito *( 1 - exito)
paste ("La varianza es: ", round(varianza,2))
```

Se determina la desviación

```{r}
desviacion.std <- sqrt(varianza)
paste("La desviación std es: ", round(desviacion.std, 2))
```

Siendo la desviación una medida de variabilidad significa que tanto estarían las probabilidades por encima o por debajo del valor esperado.

#### Probabilidades para un experimento de 100 personas

Con la función de aleatoriedad *rbinom()* se calculan las probabilidades de una muestra de $100$, con ello las proporciones o frecuencias relativas siendo los elementos de la función $n$ la cantidad de experimentos que serían $100$, *size* el tamaño del estudio original es decir $15$ y *prob* la probabilidad de éxito.

La variable llamada *variables* contiene los valores aleatorios de la *muestra* y la *frecuencia* es la cantidad de ocasiones de cada variable aleatoria.

```{r}
muestra <- 100
variables <- rbinom(n = muestra, size = n, prob = exito)
variables
frecuencia = table(variables)
frecuencia
```

Las probabilidades relativas de la muestra

```{r}
probs <- prop.table(frecuencia)
probs
tablaexp <- data.frame(x=1:length(frecuencia), f.prob.x = as.vector(probs), f.acum.x = cumsum(as.vector(probs)))
tablaexp
```

#### Visualizando las probabilidades del experimento

A partir de la nueva tabla del experimento se compara con la tabla original en dos gráficas

Con la función *par(mfrow=c(1,2))* se puede ver dos gráficas tipo *plot()* al mismo tiempo en el mismo renglón.

```{r}
par(mfrow=c(1,2))
plot(x = tabla$x, y=tabla$f.prob.x, type = "h", xlab = "X", ylab= "f(x)", main = "15 pacientes")
plot(x = tablaexp$x, y=tablaexp$f.prob.x, type = "h", xlab = "X", ylab= "f(x)", xlim = c(0,15), ylim = range(0, 0.20), main="Simulando 100 pacientes")
```

¿Cómo se comportan las probabilidades del estudio con 15 y del experimento o simulación con 100 pacientes?, muy similares las probabilidades.

### Aprobar un examen

Un estudio refleja que al aplicar un examen de estadística la probabilidad de aprobar (éxito) es del $60%$. Se pide lo siguiente:

-   Encuentre la tabla de distribución binomial para 30 estudiantes que presentan el examen

-   ¿Cuál es la probabilidad de que aprueben 5 alumnos?

-   ¿Cuál es la probabilidad de que aprueben 10 alumnos?

-   ¿Cuál es la probabilidad de que aprueben 15 o menos alumnos?

-   ¿Cuál es la probabilidad de que aprueben entre 10 y 20 alumnos?

-   ¿Cuál es la probabilidad de que aprueben mas de 25 alumnos?

-   Determinar el valor esperado VE y su significado.

-   Determinar la varianza y su desviación estándard y su significado.

#### Tabla de distribución binomial

Se incializan valores

```{r}
x <- 0:30
n <- 30
exito <- 0.60
```

Se construye la tabla

```{r}
tabla <- data.frame(x=x, f.prob.x = dbinom(x = x, size = n, prob = exito), f.acum.x = pbinom(q = x, size = n, prob = exito))
tabla
```

#### Visualizar la tabla de distribución

```{r}
plot(x=tabla$x, y=tabla$f.prob.x, 
     type='h', las=1, lwd=6, xlab = paste(min(tabla$x), '-', max(tabla$x)), ylab = "f(x)")
```

#### Probabilidad de que aprueben 15 o menos alumnos

Se calcula la probabilidad de $P(x=0) + P(x=1) + P(x=2) ... + P(15)$ o la probabilidad acumulada cuando $F(x=15)$

```{r}
prob <- pbinom(q = 15, size = n, prob = exito)
paste("La probabilidad de que aprueben 15 o menos es de ", prob)
```

#### Probabilidad de que aprueben entre 10 y 20 alumnos

Se calcula la probabilidad acumulada de $F(x=20) - F(x=10)$

```{r}
prob <- pbinom(q = 20, size = n, prob = exito) - pbinom(q = 10, size = n, prob = exito)
paste ("La probabilidad de que aprueben entre 10 y 20 estudiantes es de: ", prob)
# Se comprueba sumando los valores
sum(tabla$f.prob.x[11:21])
```

#### Probabilidad de que aprueben mas de 25 alumnos

Se debe calcular $P(x\geq26)$ o restar del el valor acumulado de 25 a 1. $1 - F(x=26)$

Con *pbinom*() y con *lower.tail() = TRUE* se encuentra la probabilidad.

```{r}
prob <- pbinom(q = 25, size = n, prob = exito, lower.tail = FALSE)
paste ("La probabilidad de que aprueben mas de 25 alumnos es de ", prob)
# Se puede comprobar sumando los renglones 27 al 31 de la tabla
sum(tabla$f.prob.x[27:31])
```

#### Valor esperado

El valor esperado es la cantidad de alumnos que aprueben el examen.

```{r}
VE <- n * exito
paste("El valor esperado es: ",VE)
```

#### Varianza y desviación

Varianza

```{r}
varianza <- n * exito *( 1 - exito)
paste ("La varianza es: ", round(varianza,2))
```

Desviación

```{r}
desviacion.std <- sqrt(varianza)
paste("La desviación std es: ", round(desviacion.std, 2))
```

La desviación como parte de la varianza significa la cantidad de alumnos que puede variar con respecto al valor medio $VE$ previamente calculado.

### Autobuses contaminantes

Suponga que un grupo de agentes de tránsito sale a una vía principal para revisar el estado de los autobuses de transporte intermunicipal. De datos históricos se sabe que un 10% de los camiones generan una mayor cantidad de humo de la permitida. En cada jornada los agentes revisan siempre 18 unidades (autobuses), asuma que el estado de un autobus es independiente del estado de los otros buses. [@hernández2021].

-   Construir la tabla de distribución

-   Visualizar la densidad o las probabilidades para cada variable discreta

-   Calcular la probabilidad de que se encuentren exactamente 2 buses que generan una mayor cantidad de humo de la permitida.

-   Calcular la probabilidad de que el número de autobuses que sobrepasan el límite de generación de gases sea al menos 4.

-   Calcular la probabilidad de que existan MAS DE TRES (a partir de CUATRO) autobuses que emitan gases por encima de lo permitido en la norma

-   Calcular el valor esperado.

-   Calcular la varianza y la desviación.

-   Generar una muestra aleatoria de 100 valores y comparar las frecuencias relativas con las probabilidad originales.

-   Interpretar el caso.

#### Construir la tabla de distribución

Se inicializan variables

```{r}
x <- 0:18
n <- 18
exito <- 0.10
```

Se construye la tabla de distribución con *dbimom()* y *dbinom().*

```{r}
tabla <- data.frame(x=x, f.prob.x = dbinom(x = x, size = n, prob = exito), f.acum.x = pbinom(q = x, size = n, prob = exito))
tabla
```

#### Visualizar probabilidades

Se muestran las probabilidades de cada variable discreta usando directamente la función plot()

```{r}
plot(x=tabla$x, y=tabla$f.prob.x, 
     type='h', las=1, lwd=6, xlab = paste(min(tabla$x), '-', max(tabla$x)), ylab = "f(x)")
```

#### Probabilidad de que se encuentren exactamente 2 buses

```{r}
x <- 2
prob <- dbinom(x = x, size = n, prob = exito)
paste ("La probabilidad de encontrar dos camiones contaminantes es de : ", prob)
```

#### Probabilidad de menos de cuatro autobuses

Se requiere encontrar la probabilidad de cuando la variables tenga valores entre cero y cuatro. $P(x=0) + P(x=1) + P(x=2) + P(x=3) + P(x=4)$ o lo que es lo mismo $P(x\leq 4)$ o en términos de probabilidad acumulada $F(x=4)$.

```{r}
x <- 4
prob <- pbinom(q = x, size = n, prob = exito)
paste ("La probabilidad de encontrar menos de cuatro camiones es de: ", prob)
```

#### Probabilidad de MAS de tres autobuses

Se requiere encontrar la probabilidad de cuando la variables tenga valores entre cuatro y dieciocho. $P(x=4) + P(x=5) + P(x=6) + P(x=7) ... + ...P(x=18)$ o lo que es lo mismo $P(x \geq 3)$ o en términos de probabilidad acumulada $F(x=18) - F(x=4)$.

```{r}
x1 <- 4
x2 <- 18
prob <- pbinom(q = x2, size = n, prob = exito) - pbinom(q = x1, size = n, prob = exito)  
paste ("La probabilidad de encontrar menos de cuatro camiones es de: ", prob)
```

Se puede encontrar usando la expresión *lower.tail = FALSE*

```{r}
pbinom(q = 4, size = n, prob = exito, lower.tail = FALSE)
```

#### Valor esperado

```{r}
VE <- n * exito
paste("El valor esperado es: ",VE)
```

El valor esperado de `r VE` significa el valor medio de camiones que se pueden encontrar que contaminan

#### Varianza y desviación

Varianza

```{r}
varianza <- n * exito *( 1 - exito)
paste ("La varianza es: ", round(varianza,2))
```

Desviación

```{r}
desviacion.std <- sqrt(varianza)
paste("La desviación std es: ", round(desviacion.std, 2))
```

La varianza y de manera más específica la desviación significa que tanto varía (se aleja o se acerca) con respeto al valor medio o valor esperado $VE$ el número de autobuses con probabilidad de encontrarse con partículas contaminantes.

#### Valores aleatorios

Se utiliza la función *rbinom()* para simular un estudio y generar valores aleatorios conforme a la distribución binomial.

El estudio o la simulación se hace con un experimento de 100 camiones, a partir del estudio previo de 18 camiones.

```{r}
n.muestra <- 100
muestra <- rbinom(n = n.muestra, size = n, prob = exito)
muestra
```

Calculando frecuencias relativas

Con la función *table()* se determina la frecuencia y con *prop.table()* se encuentra la frecuencia relativa.

```{r}
table(muestra)
data.frame(prob = prop.table(table(muestra)))
```

Se observa que los mayores valores probabilísticos está entre 1 y 3, entonces la muestra se relaciona con los valores probabilísticos del origen de los datos.

# Interpretación

en este caso vimos como se distribuye el binomio en los datos que te pide y poder sacra la probabilidad de cada uno sabiendo el porcentaje o los numeros pedidos para poder graficarlos y saber la probabilidad.tambien basarnos en los ejercicios que ya esten resueltos y poder hacer bien el proceso

# Referencias bibliográficas
