
---

## 1. Objetivo

Segmentar una foto RGB tomada con el dron usando bordes, regiones, color y textura, y aplicar operaciones morfológicas sobre la máscara de vegetación para limpiarla, contar objetos y calcular el porcentaje de cobertura vegetal.

## 2. Material

- Las mismas fotografias de la actividad anterior.

## 3. Desarrollo

1. Ejecuta el script con los valores por defecto:
    
    ```bash
    python 03_segmentacion_morfologia.py foto_rgb.JPG
    ```
    
2. Revisa los números impresos y las dos figuras guardadas en la carpeta `resultados_segmentacion`: `figura_segmentacion.png` y `figura_morfologia.png`.
3. Cambia un parámetro a la vez y vuelve a ejecutar. Anota qué cambió en los números y en las imágenes:
    
    ```bash
    python 03_segmentacion_morfologia.py foto_rgb.JPG --canny 30,90python 03_segmentacion_morfologia.py foto_rgb.JPG --kernel 3python 03_segmentacion_morfologia.py foto_rgb.JPG --kernel 9python 03_segmentacion_morfologia.py foto_rgb.JPG --area-min 600python 03_segmentacion_morfologia.py foto_rgb.JPG --hsv 30,30,30,95,255,255
    ```
    
4. Guarda las figuras de cada ejecución con otro nombre de carpeta (`--salida resultados_kernel9`, por ejemplo) para no sobrescribirlas.

---

## 4. Elementos que debe incluir el reporte

### 4.1 Portada

Nombre de la práctica, materia, nombre del alumno o del equipo, fecha y nombre del profesor.


### 4.2 Material y método

- La foto que usaste (nombre exacto) y una descripción breve de la escena: qué hay, a qué hora se tomó y cómo estaba la luz.
- Los comandos que ejecutaste y los valores de cada parámetro.
- Versión de Python, OpenCV y NumPy.

### 4.3 Resultados

| Elemento                    | Qué debe contener                                                                                                                |
| --------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| **Figura de segmentación**  | La figura con bordes (Sobel y Canny), regiones (Otsu y umbral adaptativo), color (HSV, ExG y k-means) y textura.                 |
| **Valores de segmentación** | Umbral de Otsu sobre gris y sobre ExG, y porcentaje de píxeles detectados como vegetación con HSV y con ExG.                     |
| **Figura de morfología**    | La figura con máscara cruda, erosión, dilatación, apertura, cierre, apertura + cierre y los objetos sobre la foto.               |
| **Tabla de morfología**     | Objetos, área media y cobertura de la máscara cruda y de la máscara limpia.                                                      |
| **Tabla de variaciones**    | Una tabla con cada parámetro que probaste (`--canny`, `--kernel`, `--area-min`, `--hsv`), su valor y los números que resultaron. |

Cada figura y cada tabla debe llevar un número, un título y una referencia en el texto.



---

## 5. Preguntas

Contesta con tus resultados. Una respuesta sin números ni referencia a una figura se considera incompleta.

1. ¿Qué valor de umbral de Otsu salió sobre la imagen en gris? ¿Y sobre el índice ExG?
    
2. ¿Qué porcentaje de la foto se detectó como vegetación con HSV y cuál con ExG? ¿Se parecen o son muy distintos?
    
3. Mira la figura de bordes. ¿Qué detecta mejor los contornos de los objetos, Sobel o Canny? Describe en una frase qué ves en cada uno.
    
4. En la tabla de morfología, escribe cuántos objetos había en la máscara cruda y cuántos quedaron en la máscara limpia. ¿Por qué cambió el número?
    
5. Cambia el valor de `--kernel` (por ejemplo a 3 y a 9) y vuelve a ejecutar. ¿Qué pasó con la cantidad de objetos y con la cobertura?
    

---

## 6. Criterios de evaluación

| Criterio                                                        | Puntos  |
| --------------------------------------------------------------- | ------- |
| Reporte completo y ordenado                                     | 20      |
| Tablas y figuras numeradas, con título y referencia en el texto | 20      |
| Tabla de variaciones con al menos tres parámetros probados      | 15      |
| Respuestas a las 5 preguntas, con datos de sus resultados       | 35      |
| Conclusiones propias                                            | 10      |
| **Total**                                                       | **100** |


## Notas relacionadas

- [[VisionArtificial/README|Índice de Visión Artificial]] — Navegación de la materia y sus temas.
- [[VisionArtificial/UNIDAD 1/Proyecto_Avance/Sistema inteligente para la detección de Hydrilla verticillata con capacidad de abstención|Proyecto Hydrilla con abstención]] — Aplicación de detección de vegetación acuática relacionada con estas técnicas.
- [[VisionArtificial/UNIDAD 3/TAREAS/Tarea 3.2|Tarea 3.2 · Contraste RGB y TIFF]] — Actividad previa con las mismas fotografías y análisis de contraste.
