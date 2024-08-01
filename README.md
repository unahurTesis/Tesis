# Documentación del Proceso de Detección y Marcado de Puntos en una Imagen

## Descripción General

Este proceso consiste en analizar imágenes de piezas metálicas, identificar y marcar puntos de interés, y generar una cuadrícula sobre la imagen. La cuadrícula contiene celdas que se marcan con valores `1` o `0` dependiendo de si los puntos de interés están presentes en ellas. Además, se guarda una matriz con estos valores en un archivo CSV para su posterior análisis.

## Funciones del Código

### 1. `dibujar_puntos(imagen, punto_referencia, punto_marcado)`

**Propósito:** Dibujar un punto de referencia y un punto marcado en la imagen.

**Parámetros:**
- `imagen`: La imagen en la que se dibujarán los puntos.
- `punto_referencia`: Coordenadas del punto de referencia (color rojo).
- `punto_marcado`: Coordenadas del punto marcado (color azul).

### 2. `crear_matriz(num_columnas, num_filas)`

**Propósito:** Crear una matriz inicial de `0`s del tamaño especificado.

**Parámetros:**
- `num_columnas`: Número de columnas de la matriz.
- `num_filas`: Número de filas de la matriz.

**Retorno:** Una matriz de `0`s de tamaño `num_columnas x num_filas`.

### 3. `calcular_centro_grilla(x_ref, y_ref, pixeles_por_mm, num_filas)`

**Propósito:** Calcular el centro de la cuadrícula.

**Parámetros:**
- `x_ref`: Coordenada `x` del punto de referencia.
- `y_ref`: Coordenada `y` del punto de referencia.
- `pixeles_por_mm`: Tamaño en píxeles de una celda de 1 mm.
- `num_filas`: Número de filas de la cuadrícula.

**Retorno:** Coordenadas del centro de la cuadrícula y el tamaño de la celda en píxeles.

### 4. `dibujar_grilla(imagen, centro_x, centro_y, tamanio_celda, num_columnas, num_filas, matriz_grilla)`

**Propósito:** Dibujar una cuadrícula en la imagen y anotar los valores `1` o `0` en cada celda según la matriz.

**Parámetros:**
- `imagen`: La imagen en la que se dibujará la cuadrícula.
- `centro_x`: Coordenada `x` del centro de la cuadrícula.
- `centro_y`: Coordenada `y` del centro de la cuadrícula.
- `tamanio_celda`: Tamaño de cada celda en píxeles.
- `num_columnas`: Número de columnas de la cuadrícula.
- `num_filas`: Número de filas de la cuadrícula.
- `matriz_grilla`: Matriz con los valores `1` y `0` para dibujar en la cuadrícula.

### 5. `obtener_posicion_grilla(x_marcado, y_marcado, centro_x, centro_y, tamanio_celda, num_columnas, num_filas)`

**Propósito:** Calcular la posición de una coordenada dentro de la cuadrícula.

**Parámetros:**
- `x_marcado`: Coordenada `x` del punto marcado.
- `y_marcado`: Coordenada `y` del punto marcado.
- `centro_x`: Coordenada `x` del centro de la cuadrícula.
- `centro_y`: Coordenada `y` del centro de la cuadrícula.
- `tamanio_celda`: Tamaño de cada celda en píxeles.
- `num_columnas`: Número de columnas de la cuadrícula.
- `num_filas`: Número de filas de la cuadrícula.

**Retorno:** Índices `grilla_x` y `grilla_y` que indican la posición en la matriz de la cuadrícula.

### 6. `procesar_imagen_individual(ruta_imagen, punto_referencia, puntos_marcados, pixeles_por_3mm, directorio_salida)`

**Propósito:** Procesar una imagen individual, dibujar la cuadrícula y marcar los puntos.

**Parámetros:**
- `ruta_imagen`: Ruta de la imagen a procesar.
- `punto_referencia`: Coordenadas del punto de referencia.
- `puntos_marcados`: Lista de coordenadas de los puntos marcados.
- `pixeles_por_3mm`: Número de píxeles que equivalen a 3 mm.
- `directorio_salida`: Directorio donde se guardará la imagen procesada.

**Retorno:** Matriz de la cuadrícula con los valores `1` y `0`.

### 7. `procesar_conjunto_datos(ruta_conjunto_datos, directorio_salida, ruta_salida_matriz)`

**Propósito:** Procesar un conjunto de datos de múltiples imágenes, generar las matrices de las cuadrículas y guardarlas en un archivo CSV.

**Parámetros:**
- `ruta_conjunto_datos`: Ruta del archivo CSV con el conjunto de datos.
- `directorio_salida`: Directorio donde se guardarán las imágenes procesadas.
- `ruta_salida_matriz`: Ruta donde se guardará el archivo CSV con las matrices de las cuadrículas.

## Explicación de los Cálculos

### Cálculo del Centro de la Cuadrícula

La función `calcular_centro_grilla` se encarga de calcular el centro de la cuadrícula a partir del punto de referencia. La cuadrícula se dibuja desde el punto de referencia hacia arriba.

### Determinación de la Posición en la Cuadrícula

La función `obtener_posicion_grilla` calcula la posición de un punto dentro de la cuadrícula mediante las siguientes fórmulas:

- `grilla_x`: Índice de la columna, calculado como:
  \[
  \text{grilla\_x} = \left\lfloor \frac{\text{x\_marcado} - \text{centro\_x} + \left(\frac{\text{num\_columnas}}{2}\right) \cdot \text{tamanio\_celda}}{\text{tamanio\_celda}} \right\rfloor
  \]

- `grilla_y`: Índice de la fila, calculado como:
  \[
  \text{grilla\_y} = \left\lfloor \frac{\text{centro\_y} - \text{y\_marcado}}{\text{tamanio\_celda}} \right\rfloor
  \]

Si las coordenadas calculadas están dentro de los límites de la cuadrícula, se marca la celda correspondiente con un `1`.

### Dibujo de la Cuadrícula y Valores en la Imagen

La función `dibujar_grilla` dibuja las líneas de la cuadrícula y los valores `1` o `0` en cada celda. Los valores se colocan en el centro de cada celda, calculando las coordenadas de la siguiente manera:

- `coord_x`: Coordenada `x` del centro de la celda.
- `coord_y`: Coordenada `y` del centro de la celda.

El texto con el valor se dibuja usando la función `cv2.putText`.

## Proceso Completo

1. **Lectura del Conjunto de Datos:** Se lee un archivo CSV con las coordenadas de los puntos marcados y otros datos necesarios.
2. **Procesamiento de Imágenes:** Para cada imagen:
    - Se carga la imagen y se dibujan los puntos de referencia y marcados.
    - Se calcula el centro de la cuadrícula y se crea una matriz de `0`s.
    - Se determina la posición de los puntos marcados en la cuadrícula y se actualiza la matriz con `1`s.
    - Se dibuja la cuadrícula y los valores en la imagen.
    - Se guarda la imagen procesada en el directorio de salida.
3. **Generación del Archivo de Matrices:** Se guarda un archivo CSV con las matrices de las cuadrículas de todas las imágenes procesadas.

