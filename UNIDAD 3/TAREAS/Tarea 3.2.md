
### Objetivo

Revisar las características de un disparo del dron (una foto RGB y sus cinco bandas TIFF), identificar qué banda es cada archivo y comparar cómo cambian el contraste y el histograma de cada imagen al aplicar cuatro técnicas.

## 2. Material

- Usar el archivo 01 para verificar el contraste de llas imágenes a diferentes alturas y de los magueyes. Los conjuntos de imágenes son:
Magueyes con sus TIFF:
360 
370
380
390
Árbol a diferentes alturas con sus TIFF:
200
210
220
230
240
250
260

## 3. Desarrollo

1. Ejecuta primero solo la parte de datos y confirma qué banda es cada TIFF:
    
    ```bash
    python 02_conjunto_rgb_tiff.py carpeta_del_disparo --max 6 --solo-datos
    ```
    
2. Ejecuta el script completo con los nombres de las bandas que confirmaste:
    
    ```bash
    python 02_conjunto_rgb_tiff.py carpeta_del_disparo --max 6 --etiquetas "RGB,Azul,Verde,Rojo,Borde rojo,NIR"
    ```
    
3. Revisa la tabla impresa y las figuras guardadas en la carpeta `resultados_contraste`.
4. Repite el paso 2 con `--gamma 0.4` y con `--gamma 1.5`, y observa qué cambia.

> Ajusta el orden de las etiquetas al que realmente tengan tus archivos. No lo supongas: compruébalo con los metadatos.

---

## 4. Elementos que debe incluir el reporte

### 4.1 Portada

Nombre de la práctica, materia, nombre del alumno o del equipo, fecha y nombre del profesor.

### 4.2 Objetivo

Una o dos frases con tus propias palabras. No copies el objetivo de esta guía.

### 4.3 Material y método

- Los archivos que usaste (nombres exactos) y de qué disparo son.
- Los comandos que ejecutaste, incluidos los valores de `--gamma` que probaste.
- Versión de Python, OpenCV y NumPy (puedes obtenerlas con el script `verificar.py`).

### 4.4 Resultados

| Elemento                           | Qué debe contener                                                                                                                       |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| **Tabla de características**       | La tabla de la parte A: archivo, tamaño, megapíxeles, canales, tipo de dato, mínimo, máximo, media, desviación estándar y tamaño en MB. |
| **Identificación de bandas**       | Una tabla que relacione cada archivo con su banda y longitud de onda, indicando de dónde obtuviste ese dato (metadatos).                |
| **Estadísticas por canal del RGB** | Mínimo, máximo y media de B, G y R del JPG.                                                                                             |
| **Metadatos relevantes**           | Al menos fecha, altitud o GPS, y exposición o ISO de un archivo. Si no tienes ExifTool, indícalo.                                       |
| **Figuras de contraste**           | Al menos las figuras del RGB, de una banda visible (azul, verde o rojo) y del NIR, con sus histogramas.                                 |
| **Comparación de gamma**           | Una figura o comentario que compare los valores de gamma que probaste en una misma imagen.                                              |

### 4.5 Análisis

Interpreta los resultados, no solo los describas. Usa los números de tus tablas y lo que ves en los histogramas para responder las preguntas de la sección 5.



---

## 5. Preguntas

Contesta con tus resultados. Una respuesta sin números ni referencia a una figura se considera incompleta.


1. ¿Cuántos archivos tiene un disparo completo del dron y de qué tipo es cada uno (JPG o TIFF)?
2. ¿Qué tamaño en píxeles tienen las imágenes? Escribe el ancho, el alto y los megapíxeles que aparecen en la tabla.
3. ¿Cuál de las bandas tiene la **media más alta** y cuál la **más baja**? Escribe los dos valores.
4. Mira el histograma de la imagen original de una banda. ¿Los valores están concentrados en una zona o repartidos en todo el rango? ¿Qué te dice eso sobre su contraste?
5. Elige la técnica de contraste que, en tu opinión, dejó mejor la imagen de una banda. ¿Cuál fue y qué cambió en su histograma comparado con el original?
    

---

## 6. Criterios de evaluación

| Criterio                                                        | Puntos  |
| --------------------------------------------------------------- | ------- |
| Reporte completo y ordenado (secciones 4.1 a 4.5)               | 20      |
| Tablas y figuras numeradas, con título y referencia en el texto | 20      |
| Identificación correcta de las bandas, con evidencia            | 20      |
| Respuestas a las 5 preguntas, con datos de sus resultados       | 30      |
| Conclusiones propias                                            | 10      |
| **Total**                                                       | **100** |


## Notas relacionadas

- [[VisionArtificial/README|Índice de Visión Artificial]] — Navegación de la materia y sus temas.
- [[VisionArtificial/UNIDAD 3/TAREAS/Tarea 3.3-fotos|Tarea 3.3-fotos · Segmentación y morfología]] — Continúa del contraste a la segmentación sobre las fotografías del dron.
- [[VisionArtificial/UNIDAD 1/Proyecto_Avance/Investigacion_Hydrilla_Vision_Artificial|Investigación de Hydrilla]] — Procesamiento RGB y multibanda relacionado con el monitoreo de vegetación.
