<div align="center">

<h1>Interfaz Web de Conversión de Voz Basada en Recuperación</h1>
Un marco de cambio de voz fácil de usar basado en VITS<br><br>

[![madewithlove](https://img.shields.io/badge/made_with-%E2%9D%A4-red?style=for-the-badge&labelColor=orange)](https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI)

<img src="https://counter.seku.su/cmoe?name=rvc&theme=r34" /><br>

[![Open In Colab](https://img.shields.io/badge/Colab-F9AB00?style=for-the-badge&logo=googlecolab&color=525252)](https://colab.research.google.com/github/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/blob/main/Retrieval_based_Voice_Conversion_WebUI.ipynb)
[![Licencia](https://img.shields.io/badge/LICENSE-MIT-green.svg?style=for-the-badge)](https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/blob/main/LICENSE)
[![Huggingface](https://img.shields.io/badge/🤗%20-Spaces-yellow.svg?style=for-the-badge)](https://huggingface.co/lj1995/VoiceConversionWebUI/tree/main/)

[![Discord](https://img.shields.io/badge/RVC%20Developers-Discord-7289DA?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/HcsmBBGyVk)

[**Registro de Cambios**](https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/blob/main/docs/Changelog_CN.md) | [**Preguntas Frecuentes**](https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/wiki/%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98%E8%A7%A3%E7%AD%94) | [**AutoDL·Entrenamiento de AI Cantantes por 5 Centavos**](https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/wiki/Autodl%E8%AE%AD%E7%BB%83RVC%C2%B7AI%E6%AD%8C%E6%89%8B%E6%95%99%E7%A8%8B) | [**Registro de Experimentos Comparativos**](https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/wiki/%E5%AF%B9%E7%85%A7%E5%AE%9E%E9%AA%8C%C2%B7%E5%AE%9E%E9%AA%8C%E8%AE%B0%E5%BD%95) | [**Demostración en Línea**](https://modelscope.cn/studios/FlowerCry/RVCv2demo)

[**English**](./docs/en/README.en.md) | [**中文简体**](./README.md) | [**日本語**](./docs/jp/README.ja.md) | [**한국어**](./docs/kr/README.ko.md) ([**韓國語**](./docs/kr/README.ko.han.md)) | [**Français**](./docs/fr/README.fr.md) | [**Türkçe**](./docs/tr/README.tr.md) | [**Português**](./docs/pt/README.pt.md) | [**Español**](./docs/es/README.es.md)

</div>

> El modelo base se entrenó utilizando un conjunto de datos de alta calidad de VCTK con aproximadamente 50 horas, sin preocupaciones de derechos de autor, así que siéntase libre de usarlo.

> Espere el modelo base RVCv3, con parámetros más grandes, más datos y mejores resultados, manteniendo una velocidad de inferencia básica, y requiriendo menos datos de entrenamiento.

<table>
   <tr>
		<td align="center">Interfaz de Entrenamiento e Inferencia</td>
		<td align="center">Interfaz de Cambio de Voz en Tiempo Real</td>
	</tr>
  <tr>
		<td align="center"><img src="https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/assets/129054828/092e5c12-0d49-4168-a590-0b0ef6a4f630"></td>
    <td align="center"><img src="https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/assets/129054828/730b4114-8805-44a1-ab1a-04668f3c30a6"></td>
	</tr>
	<tr>
		<td align="center">go-web.bat</td>
		<td align="center">go-realtime-gui.bat</td>
	</tr>
  <tr>
    <td align="center">Puede elegir libremente la operación que desea realizar.</td>
		<td align="center">Hemos logrado una latencia de 170 ms de extremo a extremo. Si utiliza dispositivos de entrada y salida ASIO, se puede lograr una latencia de 90 ms de extremo a extremo, pero depende mucho del soporte de los controladores de hardware.</td>
	</tr>
</table>

## Introducción
Este repositorio tiene las siguientes características:
+ Utiliza la búsqueda top1 para reemplazar las características de la fuente de entrada con las características del conjunto de entrenamiento para evitar la fuga de timbre.
+ Puede entrenar rápidamente incluso en tarjetas gráficas relativamente malas.
+ Puede obtener buenos resultados con una pequeña cantidad de datos de entrenamiento (se recomienda recopilar al menos 10 minutos de datos de voz con bajo ruido de fondo).
+ Puede cambiar el timbre mediante la fusión de modelos (con la opción de fusión de ckpt en la pestaña de procesamiento de ckpt).
+ Interfaz web fácil de usar.
+ Puede llamar al modelo UVR5 para separar rápidamente voces y acompañamientos.
+ Utiliza el algoritmo de extracción de tono de voz más avanzado [InterSpeech2023-RMVPE](#proyectos-de-referencia) para abordar problemas de sonido sordo. Ofrece los mejores resultados (significativamente) pero es más rápido y consume menos recursos que crepe_full.
+ Soporte para aceleración en tarjetas gráficas A e I.

¡Haga clic aquí para ver nuestro [video de demostración](https://www.bilibili.com/video/BV1pm4y1z7Gm/)!

## Configuración del Entorno
Las siguientes instrucciones deben ejecutarse en un entorno con Python versión superior a 3.8.  

### Métodos Universales para Windows/Linux/MacOS
Puede elegir cualquiera de los siguientes métodos.
#### 1. Instalar dependencias a través de pip
1. Instalar Pytorch y sus dependencias principales, si ya están instaladas, omita este paso. Referencia: https://pytorch.org/get-started/locally/
```bash
pip install torch torchvision torchaudio
```
2. Si es un sistema Windows + arquitectura Nvidia Ampere (RTX30xx), según la experiencia de #21, debe especificar la versión de cuda correspondiente a pytorch.
```bash
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu117
```
3. Instalar las dependencias correspondientes según su tarjeta gráfica.
- Tarjeta N
```bash
pip install -r requirements.txt
```
- Tarjeta A/I
```bash
pip install -r requirements-dml.txt
```
- Tarjeta A ROCM (Linux)
```bash
pip install -r requirements-amd.txt
```
- Tarjeta I IPEX (Linux)
```bash
pip install -r requirements-ipex.txt
```

#### 2. Instalar dependencias a través de Poetry
Instalar la herramienta de gestión de dependencias Poetry, si ya está instalada, omita este paso. Referencia: https://python-poetry.org/docs/#installation
```bash
curl -sSL https://install.python-poetry.org | python3 -
```

Al instalar dependencias a través de Poetry, se recomienda usar Python versiones 3.7-3.10, ya que las demás versiones pueden causar conflictos al instalar llvmlite==0.39.0.
```bash
poetry init -n
poetry env use "ruta a tu python.exe"
poetry run pip install -r requirments.txt
```

### MacOS
Puede instalar dependencias a través de `run.sh`.
```bash
sh ./run.sh
```

## Preparación de Otros Modelos Preentrenados
RVC necesita algunos modelos preentrenados adicionales para inferencia y entrenamiento.

Puede descargar estos modelos desde nuestro [espacio de Hugging Face](https://huggingface.co/lj1995/VoiceConversionWebUI/tree/main/).

### 1. Descargar assets
A continuación se presenta una lista de todos los modelos preentrenados y otros archivos necesarios para RVC. Puede encontrar scripts para descargarlos en la carpeta `tools`.

- ./assets/hubert/hubert_base.pt

- ./assets/pretrained 

- ./assets/uvr5_weights

Si desea utilizar el modelo de versión v2, necesitará descargar adicionalmente:

- ./assets/pretrained_v2

### 2. Instalar ffmpeg
Si ffmpeg y ffprobe ya están instalados, omita este paso.

#### Usuarios de Ubuntu/Debian
```bash
sudo apt install ffmpeg
```
#### Usuarios de MacOS
```bash
brew install ffmpeg
```
#### Usuarios de Windows
Descargue y colóquelo en el directorio raíz.
- Descargar [ffmpeg.exe](https://huggingface.co/lj1995/VoiceConversionWebUI/blob/main/ffmpeg.exe)

- Descargar [ffprobe.exe](https://huggingface.co/lj1995/VoiceConversionWebUI/blob/main/ffprobe.exe)

### 3. Descargar archivos necesarios para el algoritmo de extracción de tono RMVPE

Si desea utilizar el algoritmo de extracción de tono RMVPE más reciente, deberá descargar los parámetros del modelo de extracción de tono y colocarlos en el directorio raíz de RVC.

- Descargar [rmvpe.pt](https://huggingface.co/lj1995/VoiceConversionWebUI/blob/main/rmvpe.pt)

#### Descargar el entorno dml de rmvpe (opcional, para usuarios de A/I)

- Descargar [rmvpe.onnx](https://huggingface.co/lj1995/VoiceConversionWebUI/blob/main/rmvpe.onnx)

### 4. Tarjetas gráficas AMD Rocm (opcional, solo Linux)

Si desea ejecutar RVC en un sistema Linux basado en la tecnología Rocm de AMD, primero instale los controladores necesarios [aquí](https://rocm.docs.amd.com/en/latest/deploy/linux/os-native/install.html).

Si está utilizando Arch Linux, puede instalar los controladores necesarios con pacman:
````
pacman -S rocm-hip-sdk rocm-opencl-sdk
````
Para algunos modelos de tarjetas gráficas, es posible que deba configurar las siguientes variables de entorno (por ejemplo, RX6700XT):
````
export ROCM_PATH=/opt/rocm
export HSA_OVERRIDE_GFX_VERSION=10.3.0
````
Asegúrese también de que su usuario actual esté en los grupos `render` y `video`:
````
sudo usermod -aG render $USERNAME
sudo usermod -aG video $USERNAME
````

## Comenzar a Usar
### Iniciar Directamente
Utilice el siguiente comando para iniciar la WebUI:
```bash
python infer-web.py
```

Si anteriormente utilizó Poetry para instalar dependencias, puede iniciar la WebUI de la siguiente manera:
```bash
poetry run python infer-web.py
```

### Usar el Paquete Integrado
Descargue y descomprima `RVC-beta.7z`.
#### Usuarios de Windows
Haga doble clic en `go-web.bat`.
#### Usuarios de MacOS
```bash
sh ./run.sh
```
### Para usuarios de tarjetas I que necesitan utilizar tecnología IPEX (solo Linux)
```bash
source /opt/intel/oneapi/setvars.sh
```

## Proyectos de Referencia
+ [ContentVec](https://github.com/auspicious3000/contentvec/)
+ [VITS](https://github.com/jaywalnut310/vits)
+ [HIFIGAN](https://github.com/jik876/hifi-gan)
+ [Gradio](https://github.com/gradio-app/gradio)
+ [FFmpeg](https://github.com/FFmpeg/FFmpeg)
+ [Ultimate Vocal Remover](https://github.com/Anjok07/ultimatevocalremovergui)
+ [audio-slicer](https://github.com/openvpi/audio-slicer)
+ [Extracción de tono vocal: RMVPE](https://github.com/Dream-High/RMVPE)
   + El modelo preentrenado fue entrenado y probado por [yxlllc](https://github.com/yxlllc/RMVPE) y [RVC-Boss](https://github.com/RVC-Boss).

## Agradecimientos a Todos los Contribuyentes
<a href="https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/graphs/contributors" target="_blank">
   <img src="https://contrib.rocks/image?repo=RVC-Project/Retrieval-based-Voice-Conversion-WebUI" />
</a>
