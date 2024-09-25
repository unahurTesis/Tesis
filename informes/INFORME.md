## Función `dibujar_puntos`

## Descripción General

La función `dibujar_puntos` está diseñada para dibujar dos puntos en una imagen utilizando la biblioteca OpenCV. Cada punto es representado como un pequeño círculo lleno.

## Parámetros de Entrada

- **imagen**: La imagen sobre la cual se dibujarán los puntos. Esta imagen es representada como una matriz de píxeles.
- **punto_referencia**: Coordenadas `(x, y)` para el punto de referencia. Este punto se dibuja en rojo.
- **punto_marcado**: Coordenadas `(x, y)` para el punto marcado. Este punto se dibuja en azul.

## Operaciones y Cálculos

La función realiza las siguientes operaciones:

1. **Dibujo del Punto de Referencia:**
   - Se utiliza la función `cv2.circle` para dibujar un círculo en la posición `punto_referencia`.
   - El círculo tiene un radio de 5 píxeles y se dibuja en color rojo (BGR: `(0, 0, 255)`).
   - Se rellena completamente usando el valor `-1` para el grosor.

2. **Dibujo del Punto Marcado:**
   - Similarmente, se dibuja un círculo en la posición `punto_marcado`.
   - Este círculo es de color azul (BGR: `(255, 0, 0)`).

## Notas

- La función asume que las coordenadas de los puntos están dentro de los límites de la imagen.
- Los colores se especifican en formato BGR, que es el estándar para OpenCV.


## Función `crear_matriz`

### Descripción General

La función `crear_matriz` crea y devuelve una matriz de ceros con un tamaño especificado por el número de filas y columnas.

### Parámetros de Entrada

- **num_columnas**: Número de columnas en la matriz.
- **num_filas**: Número de filas en la matriz.

### Operaciones y Cálculos

- La matriz se construye utilizando una lista por comprensión.
- Cada elemento de la matriz es un `0`.
- La estructura devuelta es una lista de listas (una lista de filas).

## Función `calcular_centro_grilla`

### Descripción General

`calcular_centro_grilla` calcula las coordenadas del centro de una cuadrícula basándose en un punto de referencia y una escala de píxeles por milímetro, y devuelve también el tamaño de la celda.

### Parámetros de Entrada

- **x_ref**: Coordenada x del punto de referencia.
- **y_ref**: Coordenada y del punto de referencia.
- **pixeles_por_mm**: Escala de píxeles por milímetro para definir el tamaño de las celdas.
- **num_filas**: Número de filas de la cuadrícula.

### Operaciones y Cálculos

- `centro_x` y `centro_y` son las coordenadas del centro de la cuadrícula, y se asignan a `x_ref` y `y_ref`.
- `tamanio_celda` se calcula directamente como `pixeles_por_mm`.

## Notas

- La función `crear_matriz` es útil para inicializar estructuras de datos donde se requiera una matriz de tamaño fijo.
- `calcular_centro_grilla` proporciona un método sencillo para determinar el centro de una cuadrícula, útil para aplicaciones gráficas o de diseño.

## Función `dibujar_grilla`

## Descripción General

La función `dibujar_grilla` dibuja una cuadrícula sobre una imagen, centrada en un punto especificado. Cada celda de la cuadrícula puede contener un valor que se muestra en el centro de la celda.

## Parámetros de Entrada

- **imagen**: Imagen sobre la cual se dibuja la cuadrícula (matriz de píxeles).
- **centro_x**: Coordenada x del centro de la cuadrícula.
- **centro_y**: Coordenada y del centro de la cuadrícula.
- **tamanio_celda**: Tamaño de cada celda en píxeles.
- **num_columnas**: Número de columnas de la cuadrícula.
- **num_filas**: Número de filas de la cuadrícula.
- **matriz_grilla**: Matriz de valores a mostrar en cada celda.

## Operaciones y Cálculos

1. **Dibujo de Líneas Horizontales:**
   - Se dibujan líneas verdes horizontales desde `centro_x - (num_columnas // 2) * tamanio_celda` hasta `centro_x + (num_columnas // 2) * tamanio_celda` a lo largo del eje y.

2. **Dibujo de Líneas Verticales:**
   - Se dibujan líneas verticales en color verde claro a intervalos de `tamanio_celda` entre `centro_y` y `centro_y - num_filas * tamanio_celda`.

3. **Inserción de Valores:**
   - Cada celda muestra su valor centrado en el interior, utilizando la fuente `cv2.FONT_HERSHEY_SIMPLEX` en color rojo.

# Explicación de la Función `calcular_posicion_celda`

La función `calcular_posicion_celda` calcula la posición de la celda dentro de una cuadrícula para un punto marcado en la imagen. Utiliza las coordenadas del punto marcado, el centro de la cuadrícula y el tamaño de cada celda para determinar los índices de la celda.

## Parámetros de la Función

- `x_marcado` (int): Coordenada x del punto marcado.
- `y_marcado` (int): Coordenada y del punto marcado.
- `centro_x` (int): Coordenada x del centro de la cuadrícula.
- `centro_y` (int): Coordenada y del centro de la cuadrícula.
- `ancho_celda` (int): Ancho de cada celda en píxeles.
- `num_columnas` (int): Número de columnas en la cuadrícula.
- `num_filas` (int): Número de filas en la cuadrícula.

## Proceso de Cálculo

La cuadrícula está centrada alrededor de `centro_x` y `centro_y`, con un desplazamiento horizontal y vertical determinado por `ancho_celda`.

### Cálculo de `grilla_x` (Índice de Columna)

Fórmula:

$$
\text{grilla\_x} = \left\lfloor \frac{\text{x\_marcado} - \text{centro\_x} + \left(\frac{\text{num\_columnas}}{2}\right) \times \text{ancho\_celda}}{\text{ancho\_celda}} \right\rfloor
$$

- `x_marcado - centro_x`: Diferencia horizontal entre el punto marcado y el centro de la cuadrícula.
- `(num_columnas // 2) * ancho_celda`: Ajusta la diferencia para considerar el desplazamiento horizontal.
- La división por `ancho_celda` convierte la distancia ajustada a un índice de columna.

### Cálculo de `grilla_y` (Índice de Fila)

Fórmula:

$$
\text{grilla\_y} = \left\lfloor \frac{\text{centro\_y} - \text{y\_marcado}}{\text{ancho\_celda}} \right\rfloor
$$

- `centro_y - y_marcado`: Diferencia vertical entre el centro de la cuadrícula y el punto marcado.
- La división por `ancho_celda` convierte esta diferencia en un índice de fila.

## Resultado

La función devuelve un `tuple` con los índices `(grilla_x, grilla_y)` que representan la columna y fila de la celda en la que se encuentra el punto marcado.

## Ejemplo Práctico

Supongamos que:

- `x_marcado = 150`
- `y_marcado = 100`
- `centro_x = 200`
- `centro_y = 200`
- `ancho_celda = 50`
- `num_columnas = 8`
- `num_filas = 8`

### Cálculo

- **Columna (`grilla_x`)**:

$$
\text{grilla\_x} = \left\lfloor \frac{150 - 200 + (8 // 2) \times 50}{50} \right\rfloor = \left\lfloor \frac{-50 + 200}{50} \right\rfloor = \left\lfloor \frac{150}{50} \right\rfloor = 3
$$

- **Fila (`grilla_y`)**:

$$
\text{grilla\_y} = \left\lfloor \frac{200 - 100}{50} \right\rfloor = \left\lfloor \frac{100}{50} \right\rfloor = 2
$$

El punto marcado se encuentra en la celda con índices `(3, 2)` de la cuadrícula.


# Informe Detallado de la Función `procesar_imagen_individual`

## Descripción General

La función `procesar_imagen_individual` realiza el procesamiento de una imagen para identificar y marcar ciertos puntos dentro de una cuadrícula. Además, guarda la imagen procesada en un directorio de salida especificado. La función sigue varios pasos para lograr esto, incluyendo la carga de la imagen, el dibujo de puntos y grillas, y la creación de una matriz que representa los puntos marcados en la cuadrícula.

## Parámetros de Entrada

- **`ruta_imagen`** (`str`): La ruta del archivo de imagen que se va a procesar.
- **`punto_referencia`** (`tuple`): Coordenadas (x, y) del punto de referencia en la imagen.
- **`puntos_marcados`** (`list` de `tuples`): Lista de puntos marcados en la imagen. Cada punto está representado como una tupla de coordenadas (x, y).
- **`pixeles_por_3mm`** (`float`): Número de píxeles que corresponden a 3 mm en la imagen.
- **`directorio_salida`** (`str`): Ruta del directorio donde se guardará la imagen procesada.

## Paso a Paso

1. **Carga de Imagen**

   ```python
   imagen = cv2.imread(ruta_imagen)
# Análisis de la Función `procesar_conjunto_datos`

## Descripción General

La función `procesar_conjunto_datos` procesa un conjunto de datos en formato CSV que contiene información sobre imágenes y puntos de referencia. Para cada imagen listada en el conjunto de datos, la función carga la imagen, marca los puntos especificados, y guarda una matriz que representa la cuadrícula con los puntos marcados. Finalmente, guarda la matriz en un archivo CSV.

## Parámetros de Entrada

- **`ruta_conjunto_datos`** (`str`): Ruta del archivo CSV que contiene el conjunto de datos.
- **`directorio_salida`** (`str`): Directorio donde se guardarán las imágenes procesadas.
- **`ruta_salida_matriz`** (`str`): Ruta del archivo CSV donde se guardarán las matrices de la cuadrícula para cada imagen.

## Paso a Paso

1. **Carga del Conjunto de Datos**

   ```python
   conjunto_datos = pd.read_csv(ruta_conjunto_datos)
