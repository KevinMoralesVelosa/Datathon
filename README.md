![HenryLogo](https://d31uz8lwfmyn8g.cloudfront.net/Assets/logo-henry-white-lg.png)
​
# Proyecto individual 2 HENRY Machine Learning 
​ En este Readme vamos a explicar un proyecto de machine learning llevado a cabo para identificar una prediccion a traves de un algoritmo de clasificación basado en unas propiedades colombianas y su clasificación entre caras o baratas dependiendo de unas dimensiones dadas.

## Mercado inmobiliario
​
Dentro de la sociedad globalizada e industrializada, es sabido que los precios de los inmuebles han presentado un constante cambio, por lo que quienes deseen invertir o vender una propiedad se enfrentan al fenómeno especulativo existente en la valorización de éstos. Esto, debido a la constante tendencia de las ciudades a crecer demográfica y comercialmente, llegando a un punto en donde no se tiene certeza de la valorización real dentro del sector en donde se desee invertir. 
​
Pese a que el precio depende, en cierta medida, de las tendencias que esté teniendo el mercado inmobiliario en un determinado tiempo, poder estimar adecuadamente el valor de una propiedad es una referencia clave para entender si es una buena oportunidad, ya sea de compra o de venta.
​
## Descripción del problema
​
Usted ha sido contactado de una importante empresa inversora dentro del rubro de la inmobiliaria en Colombia, con el fin de que implemente un modelo de clasificación que permita clasificar el precio de las propiedades en venta, utilizando los datos que se han puesto a su disposición correspondientes al año 2020.
​
Para esto, específicamente, debe predecir la **categorización** de las propiedades entre baratas o caras, considerando como criterio el valor promedio de los precios (la media). 

## Archivos provistos
​
Se proveen los archivos dentro del archivo comprimido 'properties_colombia.zip':
 - 'properties_colombia_train.csv': Contiene 197549 registros y 26 dimensiones, el cual incluye la información **numérica** del precio.
 - 'propiedades_colombia_test.csv': Contiene 65850 registros y 25 dimensiones, el cual no incluye la información del precio. 

Estos archivos los pueden encontrar en este repositorio con su respectivo nombre.

## Descripción de las dimensiones
- id - Identificador del aviso. No es único: si el aviso es actualizado por la inmobiliaria (nueva versión del aviso) se crea un nuevo registro con la misma id pero distintas fechas: de alta y de baja.
- ad_type - Tipo de aviso (Propiedad, Desarrollo/Proyecto).
- start_date - Fecha de alta del aviso.
- end_date - Fecha de baja del aviso.
- created_on - Fecha de alta de la primera versión del aviso.
- lat - Latitud.
- lon - Longitud.
- l1 - Nivel administrativo 1: país.
- l2 - Nivel administrativo 2: usualmente provincia.
- l3 - Nivel administrativo 3: usualmente ciudad.
- l4 - Nivel administrativo 4: usualmente barrio.
- l5 - Nivel administrativo 5.
- l6 - Nivel administrativo 6.
- rooms - Cantidad de ambientes.
- bedrooms - Cantidad de dormitorios (útil en el resto de los países).
- bathrooms - Cantidad de baños.
- surface_total - Superficie total en m².
- surface_covered - Superficie cubierta en m².
- price - Precio publicado en el anuncio.
- currency - Moneda del precio publicado.
- price_period - Periodo del precio (Diario, Semanal, Mensual)
- title - Título del anuncio.
- description - Descripción del anuncio.
- property_type - Tipo de propiedad (Casa, Departamento, PH).
- operation_type - Tipo de operación (Venta).
- geometry - Puntos geométricos formados por las coordenadas latitud y longitud. 


## Desarrollo del proyecto:

## 1. Clasificación 
Lo primero fue crear una columna 'Target' esta columna clasificaba las propiedades del archivo 'properties_colombia_train.csv' entre caras o baratas a traves de la media del precio, proveniente de la columna 'Price' con:
- Caras = 1
- Baratas = 0
- su clasificación dependia de si era mayor a la media que era $643.605.091 era clasificada como cara y si era menor era clasificada como barata.

## 2. EDA Archivo 1 'properties_colombia_train.csv
Inicialmente se hizo un análisis exploratorio de los datos en donde se encontraron varias cosas por mejorar y aca lo resumimos:
- 1. Existian en la columna 'currency' (Moneda del precio publicado) habían datos en USD y COP por lo cual, Saqué el promedio del dolar en el año 2020 que es $3.693 segun esta pagina en el rango de 1/01/2020 al 31/12/2020 https://es.investing.com/currencies/usd-cop-historical-data y uso ese valor para convertir los USD a COP y luego de eso cambiar la clasificación de esas propiedades nuevamente.
- 2. Usamos las columnas 'Lat' y 'Lon' (Latitud y Longitud) para verificar la ubicación real de las propiedades del archivo a través de un mapa de calor que nos mostro la mayoría de propiedades en el centro y norte de Colombia y en las islas de San Andres y Providencia, el mapa lo encontrarán en este repositorio o esta pagina a continuación https://github.com/KevinMoralesVelosa/Datathon/blob/main/mapa.png 

-3. Realizamos matriz de correlación para evidenciar variables con importancia, esta matriz la pueden encontrar en este repositorio o en esta ruta https://github.com/KevinMoralesVelosa/Datathon/blob/main/Matriz%20correlacion.png

- 4. verificamos toda la cantidad de registros nulos en las columnas generando la siguiente informacion:

- l3 = 11.032
- l4= 152.182
- l5= 170.140
- l6 = 190.682
- rooms = 170.012
- bedrooms = 157.024
- bathrooms = 41.082
- surface_total = 190.575
- surface_covered = 187.747
- price = 63
- currency = 67
- price_period = 161578
- title = 1
- description = 121

- % de valores faltantes con respecto al total de registros:

 - variable l3: 5.58%
 - variable l4: 77.03%
 - variable l5: 86.12%
 - variable l6: 96.52%
 - variable rooms: 86.06%
 - variable bedrooms: 79.48%
 - variable bathrooms: 20.79%
 - variable surface_total: 96.46%
 - variable surface_covered: 95.03%
 - variable price: 0.03%
 - variable currency: 0.03%
 - variable price_period: 81.79%
 - variable title: 0.0005%
 - variable description: 0.061%

- 4.1 Imputación de valores
* imputacion en columna 'price' que es float, miramos cuales departamentos habían y sacar la respectiva media por departamento y rellenar campos:

- el promedio de Cundinamarca es: $996.130.526.46
- el promedio de Antioquia es: $582.801.682.61
- el promedio de Atlántico es: $624.413.850.86
- el promedio de Caldas es: $392.277.460.89
- el promedio de Norte de Santander es: $444.346.287.34
- el promedio de Quindío  es: $482.843.250.20
- el promedio de Risaralda es: $592.445.573.33
- el promedio de Santander es: $416.175.614.69
- el promedio de Tolima es: $543.456.346.55
- el promedio de Valle del Cauca es: $584.936.367.94

* Imputamos valores en las variables con la moda:
- rooms - Cantidad de ambientes.
- bedrooms - Cantidad de dormitorios (útil en el resto de los países).
- bathrooms - Cantidad de baños.

* Imputamos valores nulos con categoria 'Otros' para las propiedades que no tienen detallado
- l3 - Nivel administrativo 3: usualmente ciudad.
- l4 - Nivel administrativo 4: usualmente barrio.
- l5 - Nivel administrativo 5.
- l6 - Nivel administrativo 6.

- .5 Eliminamos columnas que no daban información relevante o que tenían datos repetidos de otras columnas a continuación detallo todo:
 * Eliminamos la columna (geometry - Puntos geométricos formados por las coordenadas latitud y longitud) 
 * Surface_total - Superficie total en m². por cantidad de registros nulos
 * Surface_covered - Superficie cubierta en m². por cantidad de registros nulos
 * ad_type - Tipo de aviso (Propiedad, Desarrollo/Proyecto). Debido a que todas eran 'Propiedad'
 * created_on - Fecha de alta de la primera versión del aviso. Debido a que es la misma información de la columna start_date.
 
 - 6 Estandarizacion, normalizacion y escalamiento datos:
 Primero graficamos los datos para conocer un poco de su distribución, esta imagen la pueden encontrar en este repositorio o en esta ruta https://github.com/KevinMoralesVelosa/Datathon/blob/main/grafica%201.png

Usamos RobustScaler() , podemos eliminar los valores atípicos y luego usar StandardScaler o MinMaxScaler para preprocesar el conjunto de datos.
Cómo funciona RobustScaler: escala características utilizando estadísticas que son robustas para valores atípicos. Este método elimina la mediana y escala los datos en el rango entre el 1er cuartil y el 3er cuartil, es decir, entre el rango de cuantil 25 y 75 . Este rango también se denomina rango intercuartílico.

## 3. Codificación variables categoricas:
Usamos One Hot Encoder para la codificación de las siguientes variables:
- property_type - Tipo de propiedad (Casa, Departamento, PH).
- operation_type - Tipo de operación (Venta).
- l2 - Nivel administrativo 2: usualmente provincia.
- l3 - Nivel administrativo 3: usualmente ciudad.
​

## 4. Creación dataset para modelo de regresión
Usamos el algoritmo KNeighborsClassifier(), entrenamos el modelo y luego hicimos la EDA para la siguiente base de testeo - 'propiedades_colombia_test.csv':     Contiene 65.850 registros y 25 dimensiones, el cual no incluye la información del precio. 

## 5. EDA 'propiedades_colombia_test.csv':
* Eliminación de de columnas sin información
* Imputación de valores nulos 
* One Hot Encoder para categorización 

## 6. realizamos la predicción con el nuevo dataset y la posterior evaluación
  
 ## Métrica a utilizar
​
Como método de evaluación del desempeño del modelo, se utilizará la métrica de Exhaustividad (Recall) para las propiedades caras, a partir de la matriz de confusión (Confusion Matrix). 
​
$$ Recall=\frac{TP}{TP+FN}$$
​
Donde $TP$ son los verdaderos positivos y $FN$ los falsos negativos.

Adicionalmente, se incluye la Accuracy como métrica de control.

## Conclusiones
Este es el trabajo que se realizo con la proyección de el algoritmo KNeighborsClassifier basado en un dataset de entrenamiento y otro de testeo, un gusto siempre aprender y crecer independientemente del resultado, un modelo siempre se puede mejorar y esa es la idea de crear una inmersión en estos proyectos.

Todo el código lo encuentran en el archivo con nombre (Resolucion PI2 ord.ipynb) o en esta ruta:  https://github.com/KevinMoralesVelosa/Datathon/blob/main/Resolucion%20PI2%20ord.ipynb comentado.

