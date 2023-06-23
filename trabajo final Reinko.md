## Trabajo Final

## "Manejo de Datos en Biología Computacional. Herramientas de Estadística"


1. Descripción tabla de datos

Los datos se obtuvieron mediante análisis de imágenes de microscopía en células cardiomiocitos aislados de ratas. El uso de un fluoróforo sensible al calcio permite visualizar los cambios en los niveles de calcio intracelular en respuesta a diferentes estímulos, como la estimulación eléctrica o alteraciones en el comportamiento normal.
Cuando ocurre un estímulo eléctrico, se produce una contracción generalizada en los cardiomiocitos. Sin embargo, antes de la contracción, hay un pico de salida de calcio que ocurre en una milésima de segundo. Este pico de salida de calcio es seguido por la relajación de la célula en respuesta a la cantidad de calcio presente. Posteriormente, el calcio regresa al retículo endoplásmico, donde una parte es liberada hacia el citoplasma y otra parte entra en las mitocondrias. Este proceso de liberación y recaptación de calcio se repite de forma cíclica.
En este contexto, los "sparks" se refieren a las chispas de calcio que se observan en las células cardiomiocitos. Estos eventos de liberación de calcio intracelular pueden ser detectados y cuantificados utilizando técnicas de análisis de imágenes y se describen mediante las variables mencionadas. El flag, o categoría, indica si un evento en particular es un "spark" (valor 1) o no (valor 2), y estos valores ya se conocen de antemano debido a la clasificación previa de los eventos.


Las variables medidas en sparks para este trabajo fueron:

1) Tiempo maximo: tiempo transcurrido desde el inicio del evento hasta el punto máximo de la señal de sparks.
2) Intensidad maxima: valor máximo alcanzado por la intensidad de los sparks (fluoroforo).
3) Intensidad minima: valor mínimo alcanzado por la intensidad de los sparks (fluoroforo).
4) Tiempo valle: tiempo transcurrido desde el inicio del evento hasta el punto mínimo de la señal de sparks.
5) Intensidad valle: valor mínimo alcanzado por la intensidad de los sparks.
6) Sparks amplitud: diferencia entre la intensidad máxima y la intensidad mínima de los sparks.
7) TTP (Time to Peak): tiempo transcurrido desde el inicio del evento hasta el punto máximo de la señal de sparks.
8) Sparks tiempo pico50: Es el tiempo transcurrido desde el inicio del evento hasta el punto en el que la señal de sparks alcanza el 50% de su valor máximo.
9) Sp tau: medida de la duración temporal característica de la señal de sparks.
10) TTP50 (Time to Peak 50%): tiempo transcurrido desde el inicio del evento hasta el punto en el que la señal de sparks alcanza el 50% de su valor máximo.
11) fullWidth: difusión del sparks, cuanto se fue al costado o que reduce canales, habla del volumen de calcio que sale.
12) (ΔF/F0)/ΔTmax: medida de la tasa de cambio de la intensidad de los sparks en relación con el tiempo.
13) fullDuration: duración total del evento de sparks, desde el inicio hasta el final.
14) width: duración de la señal de sparks a media amplitud.
15) high: Indica si el evento de sparks es considerado de alta intensidad o no.
16) flag: variable adicional utilizada para indicar alguna condición especial o etiqueta asociada al evento de sparks. 2 no corresponde y 1 si corresponde.


2. Carga de datos

Lo primero que se realizó es el importe de todos los paquetes que se necesitan para el analisis.
Después la carga de la tabla de datos:
Se cargo el archivo CSV desde la ubicación donde se encuntre guardado en este caso:'C:/Users/ivana/Desktop/tabla_trabajo_final.csv' 
y lo almacena en el data frame: datos_pdb_nan. 

Luego se utiliza la función: pd.read_csv() de la biblioteca Pandas para leer el archivo CSV y separar los valores por coma (,) como delimitador.
Una vez que se ha cargado la tabla, se utiliza el método head() para mostrar las primeras filas de los datos cargados.


### Tratamiento de los datos

El código datos_pdb_nan.describe() proporciona un resumen estadístico de los datos cargados en el DataFrame datos_pdb_nan (biblioteca Pandas).
Para cada columna numérica en el DataFrame, se calculo el recuento de valores, la media, la desviación estándar, los valores mínimo y máximo, y los percentiles (25%, 50% y 75%). Esto brinda una visión general de la distribución y las características de los datos numéricos en la tabla.


El código print(datos_pdb_nan.groupby('flag').size()) realiza una agrupación de los datos en el DataFrame datos_pdb_nan según los valores únicos de la columna 'flag' y luego muestra el tamaño de cada grupo, (groupby('flag') agrupa los datos por los valores únicos de la columna 'flag') (size() calcula el tamaño de cada grupo).
Al utilizar print() para mostrar el resultado
flag
(1.0  /  1113)
(2.0  / 1284)


El código datos_pdb_nan.isna().sum() cuenta la cantidad de valores nulos (NaN) en cada columna del DataFrame datos_pdb_nan.
El método isna() crea un DataFrame booleano del mismo tamaño que datos_pdb_nan, donde los valores son True si el valor correspondiente es nulo y False en caso contrario. Luego, al llamar a sum() en ese DataFrame, se suma la cantidad de valores True en cada columna, lo que proporciona la cantidad de valores nulos en cada una de ellas.


La variable nan_por_fila se crea para contar la cantidad de valores nulos por fila en el DataFrame datos_pdb_nan. Para esto, se utiliza el método isna() para identificar los valores nulos y luego se llama a sum(axis=1) para sumarlos a lo largo del eje de las filas.
La variable filas_a_eliminar se crea para almacenar las filas del DataFrame datos_pdb_nan que tienen más de 5 valores nulos. Se utiliza la condición nan_por_fila > 5 para filtrar las filas que cumplen esta condición.
La variable eliminadas se crea como una copia del DataFrame datos_pdb_nan que contiene únicamente las filas almacenadas en filas_a_eliminar. Esto se realiza utilizando la función copy().


El código datos_pdb = datos_pdb_nan.drop(filas_a_eliminar.index) elimina las filas del DataFrame datos_pdb_nan que se encuentran en el índice de las filas almacenadas en la variable filas_a_eliminar. El resultado se asigna al nuevo DataFrame datos_pdb.

El método drop() se utiliza para eliminar filas o columnas de DF. En este caso, se proporciona filas_a_eliminar.index como argumento, que contiene los índices de las filas que que eliminar. Al especificar index, le indico a Pandas que elimine las filas correspondientes.



3. Creación de dos grupos

El código realiza una filtración de datos en el DF datos_pdb para crear dos grupos separados basados en los valores de la columna "flag".
Se crea el DataFrame datos_pdb_flag1 que contiene las filas de datos_pdb donde el valor en la columna "flag" es igual a 1. Esto filtra el DF original y guarda las filas correspondientes al grupo flag 1 en datos_pdb_flag1.
Lo mismo para datos_pdb_flag2 con las filas donde el valor de "flag" es igual a 2.



4. Distribuciones de frecuencias de las variables

El código datos_pdb.hist(figsize=(12,12)) se genera un histograma para cada columna numérica en el DF datos_pdb. El parámetro figsize=(12,12) establece el tamaño de la figura en la que se mostrarán los histogramas.
Con plt.show() muestra los histogramas en una ventana gráfica (bibliotecas Pandas y Matplotlib).

Con plt.hist([datos_pdb_flag1["tiempo_valle"], datos_pdb_flag2["tiempo_valle"]]) genere un histograma comparativo de la columna "tiempo_valle" para los dos grupos de datos datos_pdb_flag1 y datos_pdb_flag2 (Matplotlib).

El código genera dos histogramas superpuestos, uno para datos_pdb_flag1["tiempo_valle"] y otro para datos_pdb_flag2["tiempo_valle"]. Esto te permitirá comparar visualmente las distribuciones de los valores de "tiempo_valle" entre los dos grupos.

También para la variable plt.hist([datos_pdb_flag1["intensidad_valle"], datos_pdb_flag2["intensidad_valle"]])



***Hay datos que se alejan del comportamiento estándar***
que pueden darse por errores al cargar los datos, Valores erráticos (Outliers). Se demostro con un gráfico de caja (boxplot) para la columna "intensidad_valle" en el DF datos_pdb_flag1, (Seaborn). 
La función sns.catplot() crea gráficos categóricos y se especifica kind="box" para indicar que se desea un gráfico de caja.

Se observa la distribución de los valores de "intensidad_valle" en el grupo de datos datos_pdb_flag1. Esto es útil para visualizar la dispersión, la mediana y los valores atípicos en la variable "intensidad_valle" dentro del grupo flag 1.
Lo mismo para datos_pdb_flag2).


***Criterio IQR para el tratamiento de outliers***
Se procede a eliminar los valores atípicos (outliers) de la columna "intensidad_valle" en el DF "datos_pdb_flag1" y "datos_pdb_flag2". una vez eliminado los outliers se volvio a graficar. df_sin_outliers2.head() mostrará las primeras filas del DF resultante después de eliminar los outliers y lo mismo para el otro grupo.


***Evaluar normalidad ***
Se utilizo la función shapiro() de la biblioteca SciPy para realizar una prueba de normalidad en la columna "intensidad_valle" del DF "df_sin_outliers". 
ShapiroResult(statistic=0.7614481449127197, pvalue=5.28518795899995e-37)

El estadístico de prueba es utilizado para evaluar la desviación de la distribución de la muestra de la normalidad. Cuanto más cercano esté el estadístico a 1, mayor será la evidencia a favor de que los datos siguen una distribución normal.
El valor p es la probabilidad de observar los datos si la hipótesis nula de normalidad es verdadera. En este caso, el valor p es extremadamente pequeño (5.28518795899995e-37), lo que sugiere que hay una evidencia significativa para rechazar la hipótesis nula de que los datos siguen una distribución normal. Es decir, no se ajustan bien a una distribución normal.
Por lo tanto, con base en el resultado de la prueba de Shapiro, se puede concluir que la columna "intensidad_valle" en el DF "df_sin_outliers" no sigue una distribución normal.


***print(df_sin_outliers.shape)*** nos muestra número de filas y columnas de cada DF, después de haber realizado los pasos de filtrado y eliminación de outliers.
Significa que el DataFrame "df_sin_outliers" tiene 1090 filas y 17 columna, mientras que el DataFrame "df_sin_outliers2" tiene 1244 filas y 17 columna.
***df_sin_outliers*** se realiza una muestra aleatoria de tamaño 1090 del DF df_sin_outliers2 y lo almacena en una nueva variable llamada "df_sin_outliers_", para igualar el numero de datos que el grupo "df_sin_outliers".



4. Contraste de hipotesis
***Prueba de Mann-Whitney U*** es una prueba no paramétrica utilizada para comparar las distribuciones de dos grupos independientes. El estadístico de prueba evalúa si hay diferencias significativas entre las distribuciones de los grupos. Un valor p menor que el nivel de significancia establecido (generalmente 0.05) indica evidencia de diferencias significativas entre las distribuciones.
MannwhitneyuResult(statistic=649007.0, pvalue=0.07447910430857446) en este caso, el valor p obtenido es 0.07447910430857446, que es mayor que 0.05. Esto sugiere que no hay suficiente evidencia para rechazar la hipótesis nula de que las distribuciones de "df_sin_outliers['intensidad_valle']" y "df_sin_outliers2['intensidad_valle']" son iguales. Es decir, no se encontraron diferencias significativas entre los dos grupos en términos de la variable "intensidad_valle".


 ***kolmogorov test***
El valor p obtenido es 3.0764982270146246e-06, que es extremadamente pequeño.
La prueba de Kolmogorov-Smirnov se utiliza para evaluar si dos muestras provienen de la misma distribución. El valor p asociado a esta prueba indica la probabilidad de observar los datos si las muestras provienen de la misma distribución. H0: los datos proceden de una distribución normal, H1: los datos no proceden de una distribución normal. 

En este caso, el valor p obtenido es muy pequeño, lo que sugiere que hay evidencia significativa para rechazar la hipótesis nula de que las distribuciones de "df_sin_outliers['intensidad_valle']" y "df_sin_outliers2['intensidad_valle']" son iguales. Por lo tanto, se puede concluir que existen diferencias significativas entre los dos grupos en términos de la variable "intensidad_valle".



5. Tamaño muestral
El resultado para el tamaño muestral necesario para detectar un tamaño de efecto de 0.5, con un nivel de significancia de 0.05 y un poder estadístico de 0.8, nos indica que se requeriría un tamaño muestral de al menos 6 observaciones en cada grupo para detectar un tamaño de efecto de 0.5 con las condiciones establecidas.



6. Tabla de contingencia
Tabla de contingencia y test de chi-cuadrado para evaluar la independencia entre las variables "sparks_tiempo_pico50" y "TTP50", indican que no hay evidencia suficiente para rechazar la hipótesis nula de independencia entre las variables "sparks_tiempo_pico50" y "TTP50", ya que todos los valores p son iguales a 1.


7. Intervalo de confianza
Me salio muy raro y no logro entender el resultadosi estaria bien.


8. Test de correlación 

El coeficiente de correlación de Pearson y el valor p entre las variables "sparks_tiempo_pico50" y "TTP50" en el DF "datos_pdb" (coeficiente de correlación de Pearson=0.9812800712143517) lo cual indica una fuerte correlación positiva entre las dos variables. El valor p es 0.0, lo cual sugiere que la correlación es estadísticamente significativa.









