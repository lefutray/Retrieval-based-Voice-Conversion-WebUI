## Preguntas Frecuentes (FAQ)

### P1: Error de ffmpeg / error de utf8.
Es probable que no sea un problema de FFmpeg, sino más bien un problema de la ruta de audio;

FFmpeg puede encontrar un error al leer rutas que contienen caracteres especiales como espacios y (), lo que puede causar un error de FFmpeg; y cuando el audio del conjunto de entrenamiento contiene rutas en chino, escribirlo en filelist.txt puede causar un error utf8.

### P2: No se puede encontrar el archivo de índice después de "Entrenamiento con un clic".
Si muestra "El entrenamiento ha finalizado. El programa se cierra," entonces el modelo se ha entrenado con éxito, y los errores posteriores son falsos;

La falta de un archivo de índice 'agregado' después del entrenamiento con un clic puede deberse a que el conjunto de entrenamiento es demasiado grande, lo que provoca que la adición del índice se quede atascada; esto se ha resuelto utilizando procesamiento por lotes para agregar el índice, lo que soluciona el problema de sobrecarga de memoria al agregar el índice. Como solución temporal, intente hacer clic nuevamente en el botón "Entrenar índice".

### P3: No se puede encontrar el modelo en "Inferencia de timbre" después del entrenamiento.
Haga clic en "Actualizar lista de timbres" y verifique nuevamente; si aún no es visible, verifique si hay errores durante el entrenamiento y envíe capturas de pantalla de la consola, la interfaz web y los registros/experiment_name/*.log a los desarrolladores para un análisis más detallado.

### P4: ¿Cómo compartir un modelo / Cómo usar modelos de otros?
Los archivos pth almacenados en rvc_root/logs/experiment_name no están destinados a compartir o inferencia, sino para almacenar los puntos de control del experimento para reproducibilidad y entrenamiento adicional. El modelo que se debe compartir debe ser el archivo pth de más de 60 MB en la carpeta de pesos;

En el futuro, weights/exp_name.pth y logs/exp_name/added_xxx.index se fusionarán en un solo archivo weights/exp_name.zip para eliminar la necesidad de entrada manual del índice; así que comparta el archivo zip, no el archivo pth, a menos que desee continuar entrenando en otra máquina;

### P5: Error de conexión.
Es posible que haya cerrado la consola (ventana de línea de comandos negra).

### P6: Ventana emergente de WebUI 'Esperando valor: línea 1 columna 1 (carácter 0)'.
Desactive el proxy LAN del sistema/proxy global y luego actualice.

### P7: ¿Cómo entrenar e inferir sin la WebUI?
Script de entrenamiento:
Puede ejecutar el entrenamiento en WebUI primero, y las versiones de línea de comandos de la preprocesamiento de datos y el entrenamiento se mostrarán en la ventana de mensajes.

### P8: Error de Cuda / Cuda fuera de memoria.
Hay una pequeña posibilidad de que haya un problema con la configuración de CUDA o que el dispositivo no sea compatible; es más probable que no haya suficiente memoria (fuera de memoria).

### P9: ¿Cuántos total_epoch son óptimos?
Si la calidad del audio del conjunto de entrenamiento es deficiente y el nivel de ruido es alto, 20-30 épocas son suficientes. Establecerlo demasiado alto no mejorará la calidad del audio de su conjunto de entrenamiento de baja calidad.

### P10: ¿Cuánto tiempo de duración del conjunto de entrenamiento se necesita?
Un conjunto de datos de alrededor de 10 a 50 minutos se recomienda.

### P11: ¿Para qué es la tasa de índice y cómo ajustarla?
Si la calidad tonal del modelo preentrenado y la fuente de inferencia es más alta que la del conjunto de entrenamiento, pueden mejorar la calidad tonal del resultado de la inferencia, pero a costa de un posible sesgo tonal hacia el modelo subyacente / fuente de inferencia en lugar del tono del conjunto de entrenamiento, que generalmente se refiere como "fuga de tono".

### P12: ¿Cómo elegir la GPU al inferir?
En el archivo config.py, seleccione el número de tarjeta después de "dispositivo cuda:".

### P13: ¿Cómo usar el modelo guardado en medio del entrenamiento?
Guarde a través de la extracción de modelos en la parte inferior de la pestaña de procesamiento de ckpt.

### P14: Error de archivo/memoria (al entrenar)?
Demasiados procesos y su memoria no son suficientes. Puede solucionarlo:

1. Reducir la entrada en el campo "Hilos de CPU".
2. Pre-cortar el conjunto de entrenamiento a archivos de audio más cortos.

### P15: ¿Cómo continuar entrenando usando más datos?
Paso 1: coloque todos los datos wav en path2.
Paso 2: exp_name2+path2 -> procesar conjunto de datos y extraer características.
Paso 3: copie el último archivo G y D de exp_name1 (su experimento anterior) en la carpeta exp_name2.
Paso 4: haga clic en "entrenar el modelo", y continuará entrenando desde el principio de su modelo de experimento anterior epoch.

### P16: error sobre llvmlite.dll
OSError: No se pudo cargar el archivo de objeto compartido: llvmlite.dll

### P17: RuntimeError: El tamaño expandido del tensor (17280) debe coincidir con el tamaño existente (0) en la dimensión no singleton 1. Tamaños objetivo: [1, 17280]. Tamaños de tensor: [0]

### P18: RuntimeError: El tamaño del tensor a (24) debe coincidir con el tamaño del tensor b (16) en la dimensión no singleton 2
