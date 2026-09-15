# Hydrilla y visión artificial: revisión de evidencia y propuesta de proyecto

## Resumen ejecutivo

La evidencia disponible demuestra que la detección automática de *Hydrilla verticillata* es viable, pero todavía no responde de manera completa a la pregunta sobre generalización entre cuerpos de agua. El estudio más directo entrenó EfficientDet con casi 12 000 imágenes subacuáticas y reportó 81.2% de mAP@0.5, 58.2% de mAP@[0.5:0.95] y latencias de 50–100 ms.[^1] Otro estudio específico de Hydrilla combinó satélite, UAV y sonar: obtuvo 83% de exactitud global en la clasificación satelital, 96% de sensibilidad y 87% de valor predictivo positivo, pero no pudo diferenciar Hydrilla sumergida de *Najas guadalupensis* mediante un índice RGB.[^2] Estos resultados muestran dos cosas: alcanzar alrededor de 80% es realista, pero el desempeño depende mucho de la métrica, el sensor, el estado emergente o sumergido de la planta y el lugar donde se adquirieron las imágenes.

La pregunta preliminar debe precisar la tarea visual, las especies confundibles y la forma de validación. La opción más sólida para un proyecto académico es evaluar una arquitectura multietarea que primero separe vegetación del fondo y después clasifique Hydrilla frente a especies morfológicamente semejantes. La prueba principal debe ser por cuerpo de agua no visto —leave-one-waterbody-out— y no una división aleatoria de fotogramas, porque imágenes consecutivas del mismo video pueden producir fuga de información y una exactitud artificialmente alta.

Una meta de pasar de 80% a 85% es razonable como hipótesis experimental, no como resultado garantizado. La mejora debe expresarse como cinco puntos porcentuales absolutos en una misma métrica —preferentemente macro-F1 o F1 de Hydrilla— sobre el mismo conjunto externo. Hay antecedentes de mejoras de 2.7 puntos de mAP50 mediante módulos específicos para plantas acuáticas,[^3] y de diferencias mucho mayores al incorporar atención, aprendizaje multietarea o aprendizaje activo en problemas relacionados; sin embargo, esos incrementos no pueden sumarse de forma aritmética ni trasladarse automáticamente a Hydrilla.

## Pregunta de investigación recomendada

**¿Qué desempeño de generalización externa, medido mediante macro-F1, sensibilidad de Hydrilla y mAP o IoU según la tarea, alcanza una arquitectura de aprendizaje profundo al diferenciar *Hydrilla verticillata* de otras especies de vegetación acuática sumergida, cuando se entrena con imágenes de varios cuerpos de agua y se evalúa en un cuerpo de agua no visto?**

Versión más delimitada para un proyecto realizable:

**¿Una arquitectura multietarea con segmentación de vegetación y clasificación de especie mejora al menos cinco puntos porcentuales el macro-F1 de un modelo base al diferenciar *Hydrilla verticillata* de *Egeria densa*, *Elodea* spp., *Najas* spp. y *Vallisneria* spp. en imágenes RGB subacuáticas de un cuerpo de agua no visto?**

Esta formulación mejora la pregunta preliminar porque:

- Define que el interés no es memorizar un lago, sino generalizar a otro.
- Convierte “precisión” en métricas inequívocas. En aprendizaje automático, *precision* es una métrica específica y no es sinónimo de exactitud.
- Nombra los negativos difíciles. USGS reconoce que Hydrilla es morfológicamente similar a *Egeria densa*, *Elodea nuttallii* y *Elodea canadensis*; incluso rasgos como el número de hojas por verticilo, el margen serrado y los dientes del nervio medio pueden variar con edad, sitio y calidad del agua.[^4]
- Establece una mejora comprobable: 0.80 → 0.85 en macro-F1 externo, en lugar de prometer “mayor precisión” sin comparación controlada.
- Permite que la arquitectura sea evaluada mediante ablaciones: base, base + segmentación, base + atención, base + adaptación de dominio y modelo completo.

## Revisión crítica de los artículos reunidos

La lista local contiene nueve referencias, mientras que el archivo de resúmenes desarrolla ocho.[^5] Antes de entregar una bibliografía definitiva conviene recuperar y validar el texto completo del registro faltante. Además, dos entradas de la lista —un número especial de MDPI y un blog de Ultralytics— son fuentes de contexto, no estudios experimentales revisados por pares; no deben recibir el mismo peso que un artículo con datos, protocolo y métricas.

| Fuente reunida | Aporte real al proyecto | Resultado relevante | Límite para la pregunta actual |
|---|---|---|---|
| Jeong et al., detector subacuático de Hydrilla | Evidencia directa de detección en tiempo real | EfficientDet, ~12 000 imágenes; mAP50 81.2%, mAP50–95 58.2%; 50–100 ms[^1] | Los datos no son públicos y el resumen publicado no demuestra una evaluación leave-one-waterbody-out |
| Glomb et al., satélite–UAV–sonar | Evidencia directa de monitoreo de Hydrilla y complementariedad de sensores | Clasificación satelital 83%; sensibilidad 96%; predicción positiva 87%[^2] | Funciona mejor cuando la planta alcanza máxima longitud; RGB no separó Hydrilla sumergida de *Najas guadalupensis* |
| Número especial “Remote Sensing and AI in Inland Waters” | Orienta temas y vocabulario de búsqueda | Integra teledetección, GeoAI y aguas continentales | No es un experimento ni aporta una métrica de detección |
| Blog de Ultralytics sobre calidad del agua | Contexto divulgativo y herramientas | Explica detección, segmentación y despliegue | No es evidencia científica para sustentar efectividad |
| Everitt et al., Bajo Río Grande | Base histórica de teledetección, GPS y SIG | Fotografía/videografía aérea y verificación de campo; la vegetación sumergida pierde detectabilidad con profundidad/turbidez[^6] | Tecnología previa al aprendizaje profundo y centrada en mapeo, no en generalización moderna por especie |
| Amarakoon y Wijesinghe, remoción de nutrientes | Contexto ecológico de Hydrilla | Evalúa nitrato y fosfato; Hydrilla no fue la especie con mayor remoción[^7] | No estudia visión artificial ni reconocimiento de especies |
| Mizuno et al., monitoreo acústico DIDSON | Muestra que el sonar conserva información en agua turbia | Seguimiento acústico de biota y observación de pérdida de plantas[^8] | No presenta un clasificador visual Hydrilla–otras especies |
| Cama flotante mejorada en Dianchi | Contexto de uso ecológico de Hydrilla | Reporta remoción de COD, nitrógeno y fósforo en un sistema experimental[^5] | No aporta datos a la detección visual |

Conclusión de la revisión local: solo los dos primeros artículos atacan directamente el núcleo tecnológico. Los trabajos ecológicos sirven para justificar la importancia de Hydrilla y los sensores acústicos sirven para diseñar una extensión multimodal, pero no deben incluirse en un promedio de precisión de visión artificial.

## Artículos adicionales confiables y de acceso abierto o sencillo

| Artículo | Datos y método | Resultado reportado | Utilidad para el proyecto | Acceso |
|---|---|---|---|---|
| APNet-YOLOv8s (2024) | Dataset DS-AP, 12 plantas y cuatro formas de vida; YOLOv8s con campo receptivo global, atención y cabeza rápida | mAP50 75.3%, +2.7 puntos sobre YOLOv8s; 30.5 FPS[^3] | Baseline de detección rápida y evidencia de que módulos específicos pueden producir mejoras pequeñas pero medibles | Artículo abierto en ScienceDirect |
| Clasificación no supervisada de vegetación subacuática (2025) | ConvNeXt preentrenado, UMAP y votación de varios algoritmos; prueba en Erhai y Wuhan East Lake | 97.32% en dataset público; 92.43% y 96.15% en dos datasets privados; ~20 imágenes etiquetadas para alinear miles de puntos[^9] | Referente principal para adaptación por lago, reducción del etiquetado y construcción de datasets sin sesgo de sitio | Texto completo de fácil consulta en ScienceDirect |
| AqUavplant (2024) | 197 imágenes UAV 4K, 31 especies, 9 sitios y máscaras binarias/multiclase | En split por ubicación, DeepLabV3 preentrenado obtuvo 79.59% de accuracy, 77.66% de F1 ponderado y 36.89% de Dice multiclase; el descenso frente al split estratificado revela el problema de dominio[^10] | Demuestra por qué accuracy sola puede ocultar una segmentación pobre y por qué el split por lugar es indispensable | PDF abierto en Scientific Data/PMC |
| Res-U-Net en cuatro lagos interiores (2024) | Sentinel-2, ~930 muestras de cuatro lagos y tres formas de vegetación | 90% de exactitud global, pero solo 59% para vegetación sumergida; flotante 78% y emergente 91%[^11] | Evidencia directa de que la clase sumergida es la más difícil, aunque el promedio parezca alto | PDF abierto en Remote Sensing |
| Multi-species Seagrass Detection (2020) | CNN sobre parches subacuáticos; cinco clases; datos de nueve praderas | 92.4% de exactitud global; código, pesos y dataset publicados[^12] | Buen punto de partida para transferencia, etiquetado semiautomático y comparación con especies visualmente similares | PDF en arXiv/congreso y repositorio GitHub |
| Subtidal Seagrass Detector (2023) | Tres modelos para presencia, cobertura y complejidad del fondo en hábitats diversos | 97%, 80.7% y 97.9%, respectivamente[^13] | Apoya una arquitectura jerárquica: calidad/fondo, presencia y cobertura antes de especie | PDF CC BY en Frontiers/JCU |
| SAM para vegetación subacuática (2023) | Imágenes aéreas y satelitales; prompts positivos/negativos; comparación con RF y regresión logística | En imagen aérea, F1 86.5% ± 4.1% con solo 40 pares de píxeles, frente a 54.0% de RF[^14] | Útil para acelerar máscaras y estudiar few-shot; no resuelve por sí mismo la especie Hydrilla | PDF abierto en Remote Sensing |
| MFSCNet para estanques (2026) | UAV multiespectral, 5576 pares imagen–etiqueta, cuatro especies, incluyendo Hydrilla y *Elodea nuttallii* | mIoU 0.9044; Hydrilla mantuvo IoU alto en pruebas por año/altura y el estudio reporta dos zonas de adquisición[^15] | Es la evidencia más cercana a segmentación multiclase Hydrilla–Elodea y justifica combinar información espectral, espacial y de frecuencia | Artículo y PDF abiertos en Remote Sensing |
| Aprendizaje multietarea con atención (2026) | Segmentación binaria + clasificación de 14 especies en imágenes UAV | Dice 0.7344, mIoU 0.6904; accuracy de clasificación 98.77% y macro-F1 0.9874[^16] | Justifica compartir encoder y usar atención, pero sus cifras deben validarse en un sitio externo antes de transferirlas a Hydrilla | PDF abierto en Scientific Reports |
| ResNet-50 para plantas acuáticas invasoras (2026) | 863 imágenes, transferencia y Raspberry Pi 5; incluye “Water Thyme”/Hydrilla | 86.08% de accuracy de prueba, pero precisión 80.1% y recall de validación 44.35%[^17] | Caso ideal para enseñar que una accuracy alta puede coexistir con demasiados falsos negativos | Artículo CC BY en MDPI Proceedings |
| Aprendizaje activo contrastivo (2024) | Selección de ejemplos informativos con aprendizaje contrastivo y k-means en larvas invasoras | Con 100 imágenes iniciales + 100 consultadas logró 78% de balanced accuracy, 27 puntos sobre muestreo aleatorio[^18] | Método transferible para decidir qué fotogramas de Hydrilla debe revisar un experto; no es una expectativa cuantitativa directa | PDF oficial abierto en CVF |
| Reflectancia hiperespectral de seis malezas acuáticas (2011) | 50 bandas de 405–895 nm, incluyendo Hydrilla y especies semejantes | Bandas 585–615 y 685–725 nm fueron especialmente útiles para separar varias especies, incluida Hydrilla[^19] | Fundamenta una extensión multiespectral/hiperespectral cuando RGB no separe especies sumergidas | PDF de autor/referencia académica disponible |

### Síntesis de la evidencia

No es correcto calcular una “media de precisión” combinando los porcentajes anteriores. mAP50, mAP50–95, accuracy, macro-F1, Dice e IoU responden preguntas distintas. Tampoco son comparables un detector de cajas, una segmentación por píxel, un clasificador de parches y una clasificación satelital. La comparación válida debe usar el mismo conjunto de prueba externo, las mismas clases y la misma métrica.

La literatura sí permite establecer un rango de expectativas:

- **Dentro del mismo dominio:** varios estudios de clasificación o detección se sitúan entre 75% y más de 90%, dependiendo de la tarea y la dificultad.
- **Vegetación sumergida:** el desempeño puede bajar con fuerza; en el estudio de cuatro lagos la exactitud de esta clase fue 59%, aunque la global fue 90%.[^11]
- **Cambio de ubicación:** AqUavplant muestra una caída marcada en F1/Dice cuando el split es por sitio.[^10]
- **Hydrilla frente a especies semejantes:** RGB puede ser insuficiente; Glomb et al. encontraron que Hydrilla sumergida y *Najas guadalupensis* eran prácticamente indistinguibles con su índice de color.[^2]
- **Oportunidad:** no se identificó un estudio que combine de forma completa Hydrilla, negativos morfológicamente cercanos, imágenes subacuáticas RGB y validación leave-one-waterbody-out. Ese es un vacío de investigación defendible.

## Arquitectura propuesta

### Baseline reproducible

Entrenar dos líneas base para evitar atribuir la mejora a una sola elección:

1. **YOLO de segmentación, versión pequeña:** produce máscara, clase, confianza y caja; es apropiado para tiempo real y permite medir mAP de cajas y máscaras.[^20]
2. **DeepLabV3+ o U-Net con encoder preentrenado:** baseline de segmentación semántica. La transferencia reduce la necesidad de entrenar desde cero cuando el dataset propio es limitado.[^21]

### Arquitectura mejorada recomendada

Una red multietarea con un encoder compartido y tres salidas:

1. **Control de calidad:** imagen utilizable, demasiado turbia, desenfocada o sobreexpuesta.
2. **Segmentación:** vegetación frente a agua/fondo y máscara de cada parche vegetal.
3. **Clasificación fina:** Hydrilla, *Egeria*, *Elodea*, *Najas*, *Vallisneria*, otra vegetación y desconocida.

El encoder puede ser ConvNeXt-Tiny, EfficientNetV2-S o ResNet-50 preentrenado; el decoder, DeepLabV3+ o U-Net con atención. La clase “desconocida/no concluyente” es esencial: en campo es preferible remitir un caso ambiguo a revisión que asignarlo forzosamente a Hydrilla. Para video, la predicción final debe usar consenso temporal de varios fotogramas y no una decisión aislada.

La arquitectura puede incorporar, de forma gradual:

- Pérdida combinada Dice + focal o cross-entropy ponderada para clases desbalanceadas.
- Atención espacial/canal para ignorar reflejos, sedimento y fondo.
- Aumentos específicos del agua: atenuación de rojo, cambio de temperatura de color, neblina/turbidez, partículas, desenfoque de movimiento, reflejo y compresión.
- Preentrenamiento autosupervisado con video sin etiquetar de todos los cuerpos de agua.
- Adaptación de dominio o normalización específica por sitio, sin utilizar etiquetas del sitio de prueba para ajustar el resultado final.
- Grad-CAM/Grad-CAM++ y revisión de errores para confirmar que la red observa hojas, verticilos y márgenes, no burbujas, reglas, fondos o marcas de la cámara.

## Cómo plantear una mejora de 80% a 85%

El objetivo debe escribirse así:

> **Hipótesis:** la arquitectura multietarea incrementará el macro-F1 externo de 0.80 a por lo menos 0.85, es decir, cinco puntos porcentuales absolutos, respecto al mejor baseline, manteniendo una latencia compatible con el escenario de uso.

No debe escribirse “mejorará 5%” sin especificar si es relativo o absoluto. Pasar de 0.80 a 0.85 equivale a +5 puntos absolutos y +6.25% relativo.

| Intervención | Mecanismo esperado | Prioridad | Cómo comprobarla |
|---|---|---:|---|
| Separar por cuerpo de agua, día, transecto y video | Elimina fuga de información y obliga a generalizar | Crítica | Comparar split aleatorio vs. leave-one-waterbody-out; el segundo es la cifra oficial |
| Banco de negativos difíciles | Reduce confusión con *Elodea*, *Egeria*, *Najas*, *Vallisneria*, algas, reflejos y sedimento | Muy alta | F1 de Hydrilla y matriz de confusión por especie |
| Segmentación + clasificación conjunta | Hace que la red aprenda contorno y semántica de especie | Muy alta | Ablación: clasificador solo vs. multietarea |
| Aumentos físicos del agua | Simula cambios de turbidez, profundidad, luz y cámara | Alta | Evaluar por niveles de turbidez y por cámara |
| Aprendizaje activo | Prioriza los casos inciertos para anotación experta | Alta | Curva de macro-F1 contra número de etiquetas |
| Preentrenamiento autosupervisado | Aprovecha videos sin anotar de todos los lugares | Media-alta | Mismo número de etiquetas, con y sin preentrenamiento |
| Consenso temporal | Reduce decisiones espurias de un fotograma | Media-alta | F1 por frame vs. F1 por clip/transecto |
| Multiespectral/hiperespectral | Añade bandas discriminantes cuando RGB colapsa | Alta, costo alto | Mismo sitio y fecha: RGB vs. bandas adicionales |
| Fusión con sonar | Mantiene cobertura cuando la visibilidad óptica falla | Extensión | Comparar disponibilidad/detección por nivel de turbidez |

Estas contribuciones no son aditivas. La única forma válida de afirmar +5 puntos es una ablación acumulativa sobre el mismo protocolo. APNet mostró +2.7 puntos de mAP50 sobre su base,[^3] lo que hace plausible una mejora pequeña; MFSCNet y el trabajo multietarea sugieren que atención y fusión de características pueden aportar más,[^15][^16] pero el tamaño real del efecto en Hydrilla solo se conocerá con datos propios.

## Diseño experimental

### Datos mínimos

- **Cuerpos de agua:** tres como mínimo; cuatro o cinco fortalecen la generalización. Cada uno debe tener sesiones en días distintos.
- **Clases:** Hydrilla, al menos tres especies confundibles presentes en la región, otra vegetación y fondo/no vegetación.
- **Modalidad principal:** RGB subacuático. Registrar también profundidad aproximada, turbidez, hora, iluminación, cámara, distancia, fecha y estado fenológico.
- **Unidad independiente:** clip, transecto o evento de muestreo, no el fotograma. Extraer cuadros separados en el tiempo y eliminar casi duplicados.
- **Verdad de terreno:** identificación por persona con competencia botánica. En una submuestra de casos ambiguos, conservar ejemplares o usar confirmación molecular si el presupuesto y los permisos lo permiten; USGS documenta que la similitud morfológica puede causar identificaciones erróneas.[^4]
- **Etiquetado:** máscara de la planta, especie, calidad visual, oclusión, profundidad/turbidez y nivel de certeza del experto.

Como regla inicial, conviene buscar cientos de ejemplos independientes por clase y sitio, no miles de fotogramas consecutivos. Para estimar una proporción cercana a 0.85 con un intervalo aproximado de ±3 puntos al 95% se requieren alrededor de 545 observaciones independientes; la correlación entre cuadros de un mismo video aumenta esa necesidad. El tamaño final debe ajustarse después de un piloto usando la unidad de muestreo real.

### División y validación

Aplicar leave-one-waterbody-out:

- Fold 1: entrenar en A+B+C, probar en D.
- Fold 2: entrenar en A+B+D, probar en C.
- Repetir hasta que cada cuerpo de agua haya sido prueba externa.

Dentro del entrenamiento, separar por día y video. Nunca colocar fotogramas contiguos de un mismo clip en entrenamiento y prueba. Si solo se consiguen tres lugares, reportar los tres folds y el peor sitio, no solo el promedio.

### Métricas

**Primaria:** macro-F1 externo. Da el mismo peso a cada especie y evita que una clase abundante domine el resultado.

**Secundarias:**

- F1, precisión y sensibilidad de Hydrilla.
- Balanced accuracy y PR-AUC cuando haya desbalance.
- mAP50 y mAP50–95 para detección o segmentación de instancias.
- mIoU/Dice para segmentación.
- Tasa de “desconocido”, error de calibración y confiabilidad de la probabilidad.
- Latencia, FPS, memoria y consumo energético en el dispositivo objetivo.
- Resultados por cuerpo de agua, especie, turbidez, profundidad, cámara y estación.

Reportar intervalos de confianza del 95% mediante bootstrap agrupado por video/transecto. Para comparar baseline y modelo mejorado, usar las mismas observaciones y una prueba pareada o bootstrap de la diferencia. La mejora de cinco puntos se considera respaldada cuando la diferencia es positiva de forma consistente entre sitios y su intervalo no depende de unos pocos clips.

## Lluvia de ideas priorizada

### Foco recomendado: generalización RGB entre cuerpos de agua

Es la opción con mejor equilibrio entre novedad, costo y aporte académico. El producto sería un dataset propio por sitio, dos baselines, una arquitectura multietarea y una evaluación externa. La contribución no sería “inventar otra red”, sino demostrar qué decisiones permiten —o no— sostener un aumento de cinco puntos fuera del sitio de entrenamiento.

### Ideas de alto valor y bajo o medio costo

1. Crear un **atlas de negativos difíciles** Hydrilla–*Elodea*–*Egeria*–*Najas* con acercamientos de hojas y tallos.
2. Añadir una salida **“no concluyente”** y medir cobertura–riesgo: qué exactitud se alcanza al abstenerse del 5%, 10% o 20% de los casos más inciertos.
3. Comparar **imagen individual vs. consenso de cinco fotogramas** del mismo clip.
4. Construir un **índice de dificultad** por turbidez, oclusión y distancia, y reportar curvas de desempeño.
5. Usar **aprendizaje activo** para seleccionar los 50–100 ejemplos que más información aportan en cada nueva campaña.
6. Entrenar primero con videos sin etiquetas mediante **aprendizaje autosupervisado**, luego ajustar con el dataset pequeño.
7. Hacer una prueba **cámara barata vs. cámara de acción/ROV** para saber si la mejora proviene del modelo o del sensor.
8. Evaluar **corrección de color** como aumento durante entrenamiento, no solo como filtro fijo; un realce visual puede introducir artefactos.
9. Incluir **detección abierta**: si la especie no está en el catálogo, marcar “otra/desconocida” en vez de forzar Hydrilla.
10. Publicar tarjetas de modelo con **errores típicos**, ejemplos límite, sitio de adquisición y condiciones donde no debe usarse.
11. Integrar GPS y generar un **mapa de probabilidad** con QGIS, sin confundir probabilidad visual con presencia confirmada.
12. Crear un tablero de **revisión humana** que muestre los casos más inciertos y permita corregirlos para la siguiente versión.

### Extensiones de mayor costo

- UAV multiespectral o hiperespectral para explotar bandas de borde rojo y NIR.
- Sonar de barrido descendente para presencia/biovolumen en agua turbia, con cámara para confirmar especie.
- ROV con ruta de muestreo repetible y registro sincronizado de profundidad, turbidez, temperatura y ubicación.
- Fusión tardía de RGB, sonar y variables ambientales; cada sensor produce una probabilidad que se combina al final.
- Despliegue en edge con cuantización y prueba de autonomía energética.

### Tres posibles enfoques de tesis/proyecto

| Enfoque | Pregunta central | Ventaja | Riesgo |
|---|---|---|---|
| Generalización RGB multisitio | ¿El modelo distingue Hydrilla en un lago no visto? | Novedad clara y costo moderado | Requiere acceso a varias especies y lugares |
| Segmentación multiespectral UAV | ¿Las bandas extra separan Hydrilla de *Elodea/Najas*? | Alta precisión cartográfica potencial | Sensor, permisos y procesamiento más costosos |
| Fusión cámara–sonar | ¿La fusión mantiene detección bajo turbidez? | Gran valor operativo | Integración y verdad de terreno complejas |

## Herramientas y stack de programación

### Stack recomendado para el MVP académico

| Etapa | Herramientas | Uso |
|---|---|---|
| Captura y preparación | Python, OpenCV, FFmpeg, ExifTool | Extraer cuadros, corregir metadatos, medir desenfoque, deduplicar y sincronizar video |
| Anotación | CVAT | Cajas, polígonos y máscaras; exportación COCO y máscaras de segmentación[^22] |
| Organización | pandas, NumPy, Parquet, DVC | Catálogo de imágenes y versiones del dataset/experimentos; DVC registra métricas, parámetros y artefactos junto al código[^23] |
| Aumentos | Albumentations/Kornia | Turbidez simulada, color, ruido, desenfoque, oclusión y geometría |
| Modelado | PyTorch, torchvision/timm, Ultralytics, segmentation-models-pytorch | Transferencia, YOLO-seg, DeepLabV3+, U-Net y encoders preentrenados |
| Evaluación | scikit-learn, TorchMetrics, pycocotools, FiftyOne | Matrices de confusión, F1, PR-AUC, mAP, error por sitio y galería de falsos positivos/negativos |
| Experimentos | MLflow o Weights & Biases | Registrar semilla, configuración, métricas, curvas y artefactos |
| Interpretabilidad | Captum, Grad-CAM/Grad-CAM++ | Confirmar regiones usadas por el modelo |
| Geoespacial opcional | QGIS, GDAL, Rasterio, GeoPandas | Ortofotos, mosaicos, georreferenciación y mapas de probabilidad |
| Despliegue | ONNX, ONNX Runtime, OpenVINO o TensorRT | Exportar desde PyTorch y ejecutar en CPU, GPU, web o edge; ONNX Runtime ofrece proveedores para varios tipos de hardware[^24] |
| Demo | Streamlit o Gradio; FastAPI si hay backend | Cargar imagen/video, mostrar máscara, especie, confianza y “revisión requerida” |

### Configuración por presupuesto

**Sin compra inicial:** teléfono o cámara disponible con carcasa, tarjeta de color/escala, GPS del teléfono, medición sencilla de turbidez y entrenamiento en Colab. Es suficiente para validar el método y descubrir si existe señal visual útil.

**Nivel intermedio:** cámara de acción con carcasa y luces controladas, soporte de distancia fija, mini-PC o Raspberry Pi 5 para una demostración. El artículo de ResNet-50 confirma la viabilidad de un prototipo de campo, aunque su recall bajo obliga a no confundir despliegue con confiabilidad.[^17]

**Nivel avanzado:** ROV, UAV multiespectral/hiperespectral, sonar y equipo de posicionamiento. Solo se justifica después de comprobar que RGB falla de forma consistente en los negativos difíciles y de definir presupuesto, permisos y objetivo operativo.

## Plan de trabajo recomendado

1. **Definir la tarea:** decidir si el producto necesita presencia, caja, máscara, cobertura o mapa. Para monitoreo de superficie ocupada, segmentación es más útil que clasificación simple.
2. **Inventario biológico:** confirmar qué especies confundibles existen realmente en los cuerpos de agua disponibles.
3. **Piloto:** recolectar pocas sesiones en tres sitios y medir calidad, balance y dificultad antes de comprometer una arquitectura.
4. **Dataset v1:** anonimizar si aplica, deduplicar por clip, etiquetar con certeza y construir el split por sitio.
5. **Baselines:** YOLO-seg pequeño y DeepLabV3+/U-Net preentrenado.
6. **Modelo mejorado:** multietarea + atención + aumentos físicos; después aprendizaje activo y autosupervisado.
7. **Ablaciones:** agregar un componente a la vez y registrar diferencia, intervalo y costo computacional.
8. **Prueba externa:** congelar el modelo antes de abrir etiquetas del cuerpo de agua de prueba.
9. **Demo y mapa:** exportar a ONNX, medir latencia y mostrar predicción con opción “desconocida/revisión”.
10. **Entrega científica:** dataset documentado, código reproducible, matriz de errores, limitaciones y protocolo de campo.

## Decisiones pendientes antes de cerrar la arquitectura

La elección final no debe hacerse solo por el porcentaje publicado. Es necesario confirmar con el profesor o el equipo:

- Producto esperado: clasificación, detección, segmentación, cobertura o mapa.
- Número y acceso real a cuerpos de agua.
- Especies negativas disponibles y quién validará su identidad.
- Tipo de imagen permitido: subacuática, superficie, UAV, satélite o combinación.
- Requisito de tiempo real y dispositivo de despliegue.
- Presupuesto, permisos de vuelo/muestreo y capacidad de cómputo.
- Métrica de aprobación: macro-F1, sensibilidad, mAP, IoU o exactitud.
- Si la investigación exige superar un baseline por cinco puntos o simplemente estimar el desempeño.

## Conclusión

El proyecto tiene un vacío de investigación defendible si se concentra en la **generalización de Hydrilla entre cuerpos de agua** y no solo en obtener una cifra alta con imágenes mezcladas. La referencia directa de 81.2% mAP50 muestra viabilidad, pero el 58.2% mAP50–95 evidencia que la localización precisa sigue siendo difícil.[^1] Los trabajos multisitio muestran que la vegetación sumergida y los cambios de dominio son los principales cuellos de botella.[^10][^11] Por ello, la propuesta más efectiva es una arquitectura multietarea RGB, entrenada con negativos difíciles y evaluada leave-one-waterbody-out, con una meta explícita de macro-F1 externo ≥0.85 frente a un baseline de 0.80. Multiespectral y sonar deben quedar como extensiones condicionadas a que el piloto demuestre que RGB no contiene suficiente señal.

## Fuentes

[^1]: Jeong, H., et al. “[Identifying *Hydrilla verticillata* in Real Time With a Machine Learning–Based Underwater Object Detection Program](https://onlinelibrary.wiley.com/doi/10.1002/aqc.70054).” *Aquatic Conservation: Marine and Freshwater Ecosystems* 35, e70054, 2025.
[^2]: Glomb, J. C., Lowe, R. C., Shelton, J. L., y Hamel, M. J. “[A Multi-Prong Approach for Monitoring Hydrilla in Lakes and Reservoirs](https://onlinelibrary.wiley.com/doi/full/10.1002/aff2.70018).” *Aquaculture, Fish and Fisheries* 4(6), e70018, 2024. Open Access.
[^3]: “[APNet-YOLOv8s: A Real-Time Automatic Aquatic Plants Recognition Algorithm for Complex Environments](https://www.sciencedirect.com/science/article/pii/S1470160X24010549).” *Ecological Indicators* 167, 112597, 2024. Open Access.
[^4]: U.S. Geological Survey. “[Species Profile: *Hydrilla verticillata*](https://nas.er.usgs.gov/queries/GreatLakes/FactSheet.aspx?Species_ID=6).” Nonindigenous Aquatic Species Database.
[^5]: Archivos locales “[Articulos Vision Artificial.md](Articulos%20Vision%20Artificial.md)” y “[Resumen Articulos Vision Artificial.md](Resumen%20Articulos%20Vision%20Artificial.md).” Materiales del proyecto, consultados para la revisión inicial.
[^6]: Everitt, J. H., Alaniz, M. A., y Davis, M. R. “[Using Spatial Information Technologies to Detect and Map Waterhyacinth and Hydrilla Infestations in the Lower Rio Grande](https://apms.org/wp-content/uploads/japm-41-02-093.pdf).” *Journal of Aquatic Plant Management* 41, 93–98, 2003. PDF abierto.
[^7]: Amarakoon, A. M. S. N., y Wijesinghe, W. M. N. M. “[Nitrogen and Phosphorus Removal Capacity of Water Lettuce, Water Hyacinth and Hydrilla in Domestic Wastewater of Sri Lankan Households](https://icue2024.ait.ac.th/wp-content/uploads/sites/67/2023/11/ICUE-2022-AbstractBooklet_22October2022.pdf).” ICUE 2022, DOI 10.1109/ICUE55325.2022.10113462. Resumen abierto; artículo IEEE sujeto a disponibilidad institucional.
[^8]: Mizuno, K., et al. “[Observation of Aquatic Biota in Eutrophied Pond Using Stationary Acoustic Monitoring System](https://doi.org/10.1109/UT.2013.6519813).” IEEE Underwater Technology Symposium, 2013.
[^9]: Liu, L., Bao, Z., Liang, Y., y Deng, H. “[Unsupervised Learning for Lake Underwater Vegetation Classification: Constructing High-Precision, Large-Scale Aquatic Ecological Datasets](https://www.sciencedirect.com/science/article/pii/S0048969724080525).” *Science of the Total Environment* 958, 177895, 2025.
[^10]: “[AqUavplant Dataset: A High-Resolution Aquatic Plant Classification and Segmentation Image Dataset Using UAV](https://pmc.ncbi.nlm.nih.gov/articles/PMC11661991/).” *Scientific Data* 11, 1411, 2024. [PDF del editor](https://www.nature.com/articles/s41597-024-04155-6.pdf), CC BY 4.0.
[^11]: “[Deep-Learning-Based Automatic Extraction of Aquatic Vegetation from Sentinel-2 Images—A Case Study of Lake Honghu](https://www.mdpi.com/2072-4292/16/5/867).” *Remote Sensing* 16(5), 867, 2024. [PDF abierto](https://mdpi-res.com/d_attachment/remotesensing/remotesensing-16-00867/article_deploy/remotesensing-16-00867.pdf).
[^12]: Raine, S., Marchant, R., Moghadam, P., Maire, F., Kettle, B., y Kusy, B. “[Multi-Species Seagrass Detection and Classification from Underwater Images](https://arxiv.org/abs/2009.09924).” 2020. [Código y datos](https://github.com/csiro-robotics/deepseagrass).
[^13]: Langlois, L. A., Collier, C. J., y McKenzie, L. J. “[Subtidal Seagrass Detector](https://researchonline.jcu.edu.au/79434/).” *Frontiers in Marine Science* 10, 1197695, 2023. PDF CC BY.
[^14]: “[Mapping Underwater Aquatic Vegetation Using Foundation Models With Air- and Space-Borne Images: The Case of Polyphytos Lake](https://www.mdpi.com/2072-4292/15/16/4001).” *Remote Sensing* 15(16), 4001, 2023. Open Access.
[^15]: “[Aquatic Vegetation Classification in Crab Ponds Using UAV Multispectral Imagery and a Multi-Scale Frequency-Spatial Collaborative Model](https://www.mdpi.com/2072-4292/18/14/2269).” *Remote Sensing* 18(14), 2269, 2026. Open Access.
[^16]: Rahman, A., et al. “[Attention-Enhanced Multi-Task Learning for Binary Segmentation and Fine-Grained Aquatic Plant Classification in UAV Imagery](https://www.nature.com/articles/s41598-026-51881-5).” *Scientific Reports*, 2026. Open Access CC BY.
[^17]: Reyes, J., Velasco, J., y Villaverde, J. “[Deep Learning-Based Identification of Invasive Aquatic Plant Species Using Residual Network-50](https://www.mdpi.com/2673-4591/134/1/45).” *Engineering Proceedings* 134(1), 45, 2026. CC BY.
[^18]: Chowdhury, S., Hamerly, G., y McGarrity, M. “[Active Learning Strategy Using Contrastive Learning and K-Means for Aquatic Invasive Species Recognition](https://openaccess.thecvf.com/content/WACV2024W/MaCVi/html/Chowdhury_Active_Learning_Strategy_Using_Contrastive_Learning_and_K-Means_for_Aquatic_WACVW_2024_paper.html).” WACV Workshops, 2024. PDF oficial abierto.
[^19]: “[Evaluation of Hyperspectral Reflectance Data for Discriminating Six Aquatic Weeds](https://apirs.plants.ifas.ufl.edu/entries/355468/).” Registro bibliográfico de University of Florida/IFAS; copia de autor de fácil acceso.
[^20]: Ultralytics. “[Instance Segmentation with YOLO](https://docs.ultralytics.com/tasks/segment).” Documentación oficial; incluye máscaras, métricas mAP y exportación ONNX.
[^21]: PyTorch. “[Transfer Learning for Computer Vision Tutorial](https://docs.pytorch.org/tutorials/beginner/transfer_learning_tutorial.html).” Documentación oficial.
[^22]: CVAT. “[Creating Masks](https://docs.cvat.ai/docs/manual/advanced/annotation-with-polygons/creating-mask/)” y “[COCO Format](https://docs.cvat.ai/docs/manual/advanced/formats/format-coco/).” Documentación oficial.
[^23]: DVC. “[ML Experiments as Code](https://dvc.org/blog/ml-experiment-versioning/).” Documentación oficial.
[^24]: ONNX Runtime. “[Inference with ONNX Runtime](https://onnxruntime.ai/docs/tutorials/accelerate-pytorch/pytorch.html)” y “[Execution Providers](https://onnxruntime.ai/docs/execution-providers/).” Documentación oficial.
