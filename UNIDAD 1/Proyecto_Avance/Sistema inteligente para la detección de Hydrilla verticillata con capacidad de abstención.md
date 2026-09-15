## Pregunta de investigación

¿Cuál es el nivel de precisión de una arquitectura de aprendizaje profundo al diferenciar Hydrilla verticillata de otras especies de vegetación acuática sumergida utilizando imágenes adquiridas en distintos cuerpos de agua e incorporando un mecanismo de abstención ante predicciones de baja confianza?

Esta pregunta busca evaluar la capacidad de un modelo de visión artificial para reconocer Hydrilla verticillata bajo diferentes condiciones ambientales y distinguirla de especies visualmente similares. El uso de imágenes provenientes de distintos cuerpos de agua permitirá analizar si el sistema mantiene su rendimiento ante variaciones en la iluminación, turbidez, profundidad, fondo acuático, calidad de la cámara y etapa de crecimiento de la vegetación.

La arquitectura incorporará un mecanismo de abstención que evitará forzar una clasificación cuando la evidencia visual sea insuficiente. En estos casos, el sistema mostrará el resultado “requiere revisión” en lugar de asignar una especie potencialmente incorrecta. Esto permitirá priorizar la confiabilidad de las predicciones y disminuir el número de falsos positivos.

|Elemento|Descripción|
|---|---|
|Objeto de estudio|Hydrilla verticillata|
|Problema|Similitud visual con otras especies de vegetación acuática sumergida|
|Entrada|Imágenes obtenidas en diferentes cuerpos de agua|
|Método|Arquitectura de aprendizaje profundo|
|Salidas|Hydrilla, otra vegetación o requiere revisión|
|Diferenciador|Capacidad de abstención ante baja confianza|
|Evaluación principal|Precisión, sensibilidad, macro-F1, cobertura y riesgo|

## Propuesta de proyecto

Se propone desarrollar un sistema, orientado a la identificación asistida de Hydrilla verticillata mediante imágenes. El usuario podrá cargar una fotografía desde una computadora o dispositivo móvil, y el sistema procesará la imagen utilizando un modelo de aprendizaje profundo entrenado para localizar la vegetación y determinar si corresponde a Hydrilla o a otra clase de vegetación acuática.

El sistema no se limitará a presentar una etiqueta. También mostrará el nivel de confianza, la región de la imagen utilizada para tomar la decisión y una recomendación relacionada con el resultado. Cuando la imagen sea borrosa, oscura, turbia, contenga vegetación demasiado pequeña o produzca resultados ambiguos, el sistema indicará que se requiere una revisión humana.

|Componente|Función|
|---|---|
|Carga de imágenes|Permitir al usuario seleccionar o capturar una fotografía|
|Control de calidad|Detectar imágenes borrosas, oscuras o con poca información|
|Modelo de visión artificial|Localizar y clasificar la vegetación acuática|
|Estimación de confianza|Cuantificar la seguridad de la predicción|
|Mecanismo de abstención|Rechazar predicciones poco confiables|
|Explicación visual|Mostrar cuadros, máscaras o mapas de atención|
|Registro de resultados|Guardar predicciones, confianza y decisión del usuario|
|Interfaz web|Facilitar la interacción sin requerir conocimientos técnicos|

## Funcionamiento del sistema

El proceso comenzará con la adquisición de una imagen. Posteriormente, se aplicará una revisión automática de calidad para determinar si la fotografía contiene suficiente información visual. Si la imagen supera esta revisión, será procesada por el modelo de aprendizaje profundo.

El modelo devolverá probabilidades para las clases definidas. Estas probabilidades se compararán con umbrales de confianza previamente calibrados. Una predicción se aceptará solamente cuando el sistema alcance un nivel de confianza suficiente y cuando la separación entre la primera y la segunda clase sea clara. De lo contrario, la imagen será enviada a revisión.

|Etapa|Proceso|Resultado|
|---|---|---|
|1|Captura o carga de la imagen|Imagen disponible para análisis|
|2|Verificación de calidad|Imagen aceptada o solicitud de una nueva fotografía|
|3|Preprocesamiento|Ajuste de tamaño, color y normalización|
|4|Detección o segmentación|Localización de la vegetación|
|5|Clasificación|Probabilidades por clase|
|6|Evaluación de confianza|Comparación con los umbrales calibrados|
|7|Decisión|Hydrilla, otra vegetación o requiere revisión|
|8|Presentación|Resultado, confianza y explicación visual|

## Resultados que puede mostrar el sistema

|Resultado|Interpretación|Recomendación|
|---|---|---|
|Hydrilla detectada|Existe evidencia visual suficiente de Hydrilla verticillata|Verificar el área señalada y registrar la observación|
|Otra vegetación|La vegetación detectada no presenta evidencia suficiente para considerarse Hydrilla|Conservar el registro para comparación|
|Requiere revisión|La predicción es ambigua o presenta baja confianza|Solicitar revisión humana o capturar una nueva imagen|
|Imagen inadecuada|La calidad visual no permite realizar el análisis|Mejorar iluminación, enfoque o distancia|

## Detector con capacidad de abstención

La principal característica del sistema será su capacidad para reconocer cuándo no dispone de evidencia suficiente. En un clasificador tradicional, el modelo siempre selecciona una clase, incluso si las probabilidades son similares o muy bajas. Esto puede provocar identificaciones incorrectas y generar una falsa sensación de seguridad.

El detector con abstención introducirá una tercera respuesta operativa: “requiere revisión”. Esta opción no representa una especie adicional, sino una decisión de seguridad. Su finalidad será reducir los errores en las imágenes que se encuentren fuera de las condiciones conocidas durante el entrenamiento.

Una regla inicial podría considerar la probabilidad más alta y la diferencia entre las dos clases más probables. Los valores definitivos deberán establecerse mediante experimentos de calibración con datos de validación.

|Condición de ejemplo|Respuesta|
|---|---|
|Confianza igual o superior al 80 % y separación clara entre clases|Aceptar predicción|
|Confianza entre 55 % y 80 %|Requiere revisión|
|Confianza menor al 55 %|Resultado inconcluso|
|Imagen borrosa, oscura o con vegetación poco visible|Solicitar una nueva imagen|
|Probabilidades muy similares entre Hydrilla y otra vegetación|Requiere revisión|

Estos porcentajes son valores iniciales y no deben considerarse definitivos. El umbral deberá seleccionarse mediante la curva riesgo-cobertura, buscando un equilibrio entre la cantidad de imágenes clasificadas automáticamente y la proporción de errores cometidos en las predicciones aceptadas.

## Arquitectura propuesta

Se propone utilizar una arquitectura basada en modelos de detección o segmentación profunda. Un modelo de detección, como YOLO, puede localizar rápidamente las zonas donde aparece vegetación. Cuando se requiera delimitar con mayor detalle la superficie ocupada, puede emplearse una variante de segmentación.

El entrenamiento puede iniciarse con pesos preentrenados para reducir la cantidad de imágenes y recursos computacionales necesarios. Después, el modelo se ajustará con imágenes de Hydrilla y otras especies acuáticas. Las técnicas de aumento de datos permitirán simular variaciones de iluminación, turbidez, color, desenfoque y escala.

|Componente técnico|Propuesta|
|---|---|
|Modelo inicial|YOLO con detección o segmentación|
|Estrategia de entrenamiento|Transfer learning|
|Clases iniciales|Hydrilla y otra vegetación|
|Salida adicional|Requiere revisión|
|Aumento de datos|Brillo, contraste, color, desenfoque, ruido y recorte|
|Explicabilidad|Cuadros, máscaras de segmentación o mapas de atención|
|Despliegue|Aplicación web|
|Optimización|Exportación a ONNX para inferencia eficiente|

## Conjunto de datos

El conjunto de datos deberá contener imágenes representativas de Hydrilla verticillata y otras especies de vegetación acuática sumergida. También será necesario registrar el cuerpo de agua de procedencia, las condiciones de captura y, cuando sea posible, la especie observada.

La división de los datos deberá realizarse por cuerpo de agua y no únicamente de manera aleatoria. Si fotografías muy similares del mismo sitio aparecen en entrenamiento y prueba, el modelo podría memorizar el fondo, el color del agua o las condiciones de iluminación. Esto produciría resultados artificialmente altos.

|Grupo de datos|Uso|
|---|---|
|Entrenamiento|Ajustar los parámetros del modelo|
|Validación|Seleccionar el modelo y calibrar los umbrales|
|Prueba interna|Medir el desempeño en condiciones conocidas|
|Prueba externa|Evaluar imágenes de un cuerpo de agua no utilizado en el entrenamiento|
|Imágenes ambiguas|Evaluar el funcionamiento de la abstención|
|Imágenes de baja calidad|Evaluar el control automático de calidad|

## Herramientas de software

|Herramienta|Uso propuesto|Costo|
|---|---|---|
|Python|Lenguaje principal de desarrollo|Gratuito|
|PyTorch|Entrenamiento del modelo de aprendizaje profundo|Gratuito|
|Ultralytics YOLO|Detección y segmentación de vegetación|Gratuito para investigación según su licencia aplicable|
|OpenCV|Procesamiento y control de calidad de imágenes|Gratuito|
|Albumentations|Aumento de datos|Gratuito|
|CVAT|Etiquetado de cuadros y máscaras|Gratuito y de código abierto|
|Roboflow|Administración y etiquetado opcional del conjunto de datos|Plan gratuito limitado|
|scikit-learn|Métricas, calibración y análisis de resultados|Gratuito|
|Streamlit|Construcción de la aplicación web|Gratuito|
|FastAPI|Creación de una API para el modelo|Gratuito|
|pandas|Organización de resultados|Gratuito|
|Matplotlib o Plotly|Gráficas de evaluación|Gratuito|
|Google Colab|Entrenamiento con GPU en la nube|Plan gratuito disponible|
|Git y GitHub|Control de versiones y respaldo|Gratuito|
|ONNX Runtime|Ejecución optimizada del modelo|Gratuito|

## Stack de programación

|Capa|Tecnología|
|---|---|
|Lenguaje|Python|
|Procesamiento visual|OpenCV y Albumentations|
|Aprendizaje profundo|PyTorch y YOLO|
|Etiquetado|CVAT o Roboflow|
|Análisis de métricas|scikit-learn, pandas y NumPy|
|Interfaz web|Streamlit|
|API opcional|FastAPI|
|Visualización|Matplotlib y Plotly|
|Base de datos inicial|SQLite|
|Despliegue|Computadora local, Streamlit Community Cloud o servicio similar|
|Optimización|ONNX Runtime|

## Herramientas de hardware

El sistema puede desarrollarse utilizando equipo accesible. Una computadora personal será suficiente para organizar los datos, programar la aplicación y ejecutar modelos pequeños. Para el entrenamiento puede utilizarse una GPU gratuita o limitada en Google Colab.

Las imágenes pueden obtenerse con un teléfono celular o una cámara convencional. Para capturas bajo el agua se puede emplear una funda impermeable económica. Una tarjeta de referencia de color permitirá corregir parcialmente las variaciones producidas por el agua y la iluminación.

|Hardware|Función|Prioridad|
|---|---|---|
|Computadora personal|Desarrollo y ejecución del sistema|Necesario|
|Teléfono celular o cámara|Captura de imágenes|Necesario|
|Google Colab con GPU|Entrenamiento del modelo|Recomendado|
|Funda impermeable|Captura cercana o subacuática|Opcional|
|Tarjeta de referencia de color|Normalización de color|Recomendado|
|Lámpara LED|Mejorar la iluminación|Opcional|
|Raspberry Pi|Ejecución portátil del sistema|Etapa posterior|
|Cámara de acción|Captura continua de video|Etapa posterior|

## Interfaz de la aplicación web

La aplicación presentará una interfaz sencilla. El usuario podrá cargar una imagen, ejecutar el análisis y observar el resultado. Además de la clasificación, se mostrará la confianza estimada y la región identificada por el modelo.

Cuando la predicción sea rechazada, la aplicación explicará la causa probable y recomendará repetir la captura. También podrá incluir una opción para que una persona confirme o corrija el resultado, permitiendo almacenar casos difíciles para mejorar entrenamientos posteriores.

|Sección|Contenido|
|---|---|
|Inicio|Descripción del sistema e instrucciones|
|Carga de imagen|Selección o captura de fotografía|
|Resultado|Clase estimada y nivel de confianza|
|Explicación visual|Cuadro o máscara sobre la vegetación|
|Estado de decisión|Predicción aceptada o requiere revisión|
|Recomendación|Acción sugerida al usuario|
|Historial|Registro de análisis anteriores|
|Corrección|Confirmación o modificación humana del resultado|

## Propuesta de evaluación

La exactitud global no será suficiente para evaluar el sistema, especialmente si existen más imágenes de una clase que de otra. Por ello, se utilizarán métricas que permitan estudiar tanto la identificación de Hydrilla como el comportamiento del mecanismo de abstención.

|Métrica|Qué mide|
|---|---|
|Precisión de Hydrilla|Proporción de detecciones de Hydrilla que son correctas|
|Sensibilidad o recall|Proporción de casos reales de Hydrilla encontrados|
|F1-score|Equilibrio entre precisión y sensibilidad|
|Macro-F1|Rendimiento equilibrado entre las clases|
|mAP|Calidad general de las detecciones|
|IoU|Coincidencia entre la región predicha y la región real|
|Tasa de abstención|Porcentaje de imágenes enviadas a revisión|
|Cobertura|Porcentaje de imágenes respondidas automáticamente|
|Riesgo selectivo|Error cometido solamente en las predicciones aceptadas|
|Error de calibración|Diferencia entre confianza estimada y precisión observada|
|Tiempo de inferencia|Rapidez del análisis por imagen|

## Comparación experimental

Para determinar el efecto de la abstención, se compararán dos configuraciones del mismo modelo. La primera estará obligada a responder todas las imágenes, mientras que la segunda podrá enviar los casos inciertos a revisión.

|Configuración|Funcionamiento|
|---|---|
|Clasificador convencional|Siempre selecciona una clase|
|Clasificador con abstención|Rechaza predicciones de baja confianza|
|Comparación principal|Errores en imágenes aceptadas|
|Comparación secundaria|Precisión, macro-F1, cobertura y falsos positivos|
|Resultado esperado|Menor cantidad de errores en las decisiones automáticas|

## Propuesta de resultado

Se espera obtener un sistema web capaz de recibir una imagen, identificar las zonas con vegetación acuática y generar una de tres respuestas: “Hydrilla detectada”, “otra vegetación” o “requiere revisión”. Cada resultado deberá incluir un nivel de confianza y una explicación visual.

Si los modelos estudiados presentan valores cercanos al 80 % en métricas comparables, el proyecto puede establecer como objetivo experimental alcanzar aproximadamente un 85 %, lo que equivaldría a una mejora de cinco puntos porcentuales. Sin embargo, esta mejora deberá plantearse como una meta y no como un resultado garantizado.

La mejora puede buscarse mediante transferencia de aprendizaje, aumento de datos, corrección de color, inclusión de imágenes difíciles, calibración de confianza y evaluación por cuerpo de agua. También será importante comprobar que el aumento en una métrica no produzca una reducción excesiva de la cobertura.

|Resultado esperado|Evidencia|
|---|---|
|Identificación de Hydrilla|Etiqueta, confianza y región detectada|
|Diferenciación de otras plantas|Predicción de otra vegetación|
|Reducción de falsas identificaciones|Comparación con el clasificador convencional|
|Manejo de casos ambiguos|Respuesta “requiere revisión”|
|Explicación de la decisión|Cuadro, máscara o mapa visual|
|Generalización|Resultados en un cuerpo de agua no usado para entrenamiento|
|Funcionamiento accesible|Aplicación web ejecutable desde un navegador|
|Registro de resultados|Historial exportable en formato CSV|

## Criterios de éxito

|Criterio|Meta propuesta|
|---|---|
|Macro-F1|Igual o superior al modelo base|
|Precisión de Hydrilla|Priorizar la reducción de falsos positivos|
|Sensibilidad|Mantener una detección adecuada de casos reales|
|Riesgo selectivo|Menor que el error del clasificador obligado|
|Cobertura|Clasificar automáticamente una proporción útil de imágenes|
|Calibración|La confianza debe aproximarse a la precisión observada|
|Tiempo de respuesta|Pocos segundos por imagen|
|Usabilidad|Resultado comprensible para usuarios no especializados|

## Ventaja principal

La ventaja del sistema no será únicamente obtener una precisión mayor, sino proporcionar resultados más confiables. En aplicaciones ambientales, una clasificación incorrecta puede producir decisiones equivocadas sobre inspección, muestreo o control de vegetación.

La opción “requiere revisión” permite que el sistema reconozca sus limitaciones y solicite intervención humana cuando encuentra imágenes ambiguas. De esta manera, se propone una herramienta de apoyo para el monitoreo de Hydrilla verticillata y no un reemplazo absoluto de la validación realizada por especialistas.