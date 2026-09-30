# Lucidframes-product-photos
Automated product photo background removal and cropping script powered by PyTorch and the Lucida model. Converts batch raw images into transparent PNGs and clean, margin-aligned e-commerce JPGs on a white background.

# Lucidglasses Product Photos



**Aplicación de escritorio para eliminar fondos y preparar fotografías de productos por lotes.**



Lucidglasses permite seleccionar una carpeta y transformar sus fotografías en dos resultados: imágenes PNG con transparencia e imágenes JPG recortadas, con fondo blanco y un margen uniforme. Está orientada a comercios, equipos de diseño y personas que preparan imágenes para catálogos y tiendas en línea.



El programa combina el modelo de segmentación Lucida con un flujo automático de recorte y conversión. El procesamiento de las fotografías se realiza en el equipo del usuario.



## Qué problema resuelve



Preparar muchas fotografías de productos suele requerir eliminar fondos, recortar espacios vacíos, añadir fondo blanco y exportar cada imagen por separado. Fotos Limpias reúne estas tareas en un proceso por lotes y organiza los resultados en carpetas separadas, conservando los archivos originales.



## Funciones principales

- Selección de la carpeta de fotografías mediante una interfaz gráfica.
- Configuración automática de las dependencias en un entorno de Python, o reutilización de un entorno compatible existente.
- Eliminación del fondo con Lucida.
- Exportación de imágenes PNG con transparencia.
- Recorte según los límites del objeto detectado.
- Adición de un margen de 20 píxeles por cada lado.
- Conversión a JPG con fondo blanco y calidad configurada en 95.
- Visualización del progreso, registro de errores y cancelación del proceso.
- Reutilización de PNGs vigentes para continuar trabajos interrumpidos.
- Botón para abrir la carpeta de resultados finales.

## Cómo se utiliza

1. Abrir `Procesar_fotos.py` con Python, o el ejecutable de Windows una vez generado.
2. Elegir la carpeta que contiene las fotografías originales.
3. Pulsar **Procesar fotos**.
4. Esperar la configuración inicial, si es necesaria, y el procesamiento.
5. Abrir **Limpias** para consultar los JPG finales.

La primera configuración necesita internet para descargar las dependencias y el modelo. La aplicación comprueba el entorno automáticamente; no exige activar manualmente un entorno virtual.

## 📸 Ejemplo de Procesamiento (Antes y Después)

El programa procesa las fotografías por lotes y genera las carpetas de salida automáticas:

| 1. Foto Original (Entrada) | 2. Carpeta `/PNGs` (Transparencia) | 3. Carpeta `/Limpias` (JPG Fondo Blanco) |
| :---: | :---: | :---: |
| ![Original](./assets/20012%20(4)%20Sucia.JPG) | ![PNG Transparente](./assets/20012%20(4).png) | ![JPG Limpio](./assets/20012%20(4).jpg) |

> **Nota:** El algoritmo recorta el objeto respetando bordes complejos y añade un margen uniforme de 20px sobre fondo blanco puro.

## Entradas y resultados

| Elemento | Descripción |
| --- | --- |
| Fotografías originales | Archivos ubicados directamente en la carpeta seleccionada. |
| Formatos de entrada | JPG, JPEG, PNG, WEBP, BMP, TIF y TIFF. |
| Carpeta `PNGs` | Imágenes procesadas por Lucida con canal de transparencia. |
| Carpeta `Limpias` | Imágenes recortadas y convertidas a JPG con fondo blanco. |

Ambas carpetas se crean dentro de la carpeta seleccionada. El programa no recorre sus subcarpetas ni modifica las fotografías originales. Si dos entradas comparten nombre base y tienen extensiones distintas, diferencia los nombres de salida para evitar colisiones.

Los PNGs existentes se reutilizan cuando son válidos y su fecha de modificación es igual o posterior a la del original. Los JPGs correspondientes se regeneran durante la segunda etapa.

## Información técnica básica

| Componente | Tecnología o configuración |
| --- | --- |
| Lenguaje | Python. |
| Interfaz | Tkinter y Ttk. |
| Modelo de eliminación de fondo | `egeorcun/lucida`, cargado mediante Hugging Face Transformers. |
| Inferencia | PyTorch, en CPU por defecto y con precisión `float32`. |
| Preprocesamiento | Torchvision; entrada al modelo de 1024 × 1024 píxeles. |
| Manipulación y exportación | Pillow. |
| Recorte | Máscara alfa con umbral de 15 sobre 255. |
| Margen final | 20 píxeles por cada lado del objeto recortado. |
| Exportación JPEG | Calidad 95, sin submuestreo de color (`subsampling=0`) y guardado optimizado. |
| Distribución Windows | Generación de un ejecutable con PyInstaller. |

El modelo analiza una copia redimensionada de la imagen. La máscara resultante se ajusta a las dimensiones originales antes de crear el PNG transparente. El JPG final conserva el tamaño en píxeles del objeto recortado y añade el margen configurado.

La interfaz coordina la preparación y el trabajo en segundo plano. El procesamiento utiliza un proceso separado, ejecutado con el Python del entorno preparado. Esto evita depender de las bibliotecas instaladas en el Python asociado al doble clic del archivo.

Las imágenes se procesan secuencialmente: primero se generan los PNGs y después los JPGs. Cada resultado se guarda inicialmente en un archivo temporal y reemplaza su destino al finalizar el guardado correctamente.



## Requisitos e instalación



Instalación para usuarios (Suscripción Pro):Adquiere tu suscripción activa en nuestra tienda oficial de Lemon Squeezy. Descarga el ejecutable Fotos_Limpias.exe y ejecútalo en tu equipo Windows.   Ingresa tu clave de licencia cuando el programa la solicite.¡Listo! El programa estará activado para procesar lotes durante el periodo de tu suscripción.



### Ejecución del archivo Python



- Python de 64 bits con Tkinter; Python 3.13 es la versión recomendada para este proyecto.

- Permiso de lectura y escritura en la carpeta de fotografías.

- Conexión a internet para la configuración inicial y la descarga del modelo.

- Espacio disponible para las dependencias, el modelo y los resultados.



Las dependencias utilizadas incluyen `torch`, `torchvision`, `pillow`, `transformers` de la rama 4, `timm`, `einops`, `kornia`, `numpy`, `scipy`, `huggingface-hub` y `safetensors`.



El entorno de trabajo propio y los registros se guardan en `%LOCALAPPDATA%\FotosLucida` en Windows. Las fotografías de salida se guardan en la carpeta elegida por el usuario.



Las dependencias y los pesos del modelo no se incluyen en el ejecutable: se descargan durante la preparación inicial.



## Privacidad y conexiones externas



La aplicación procesa las fotografías localmente y guarda los resultados en el equipo. Su flujo implementado no incorpora una función para subir las fotografías a un servidor de inferencia.



Durante la preparación se conecta a repositorios de paquetes, al servicio de descarga del modelo en Hugging Face y, cuando corresponde, a `python.org` para descargar Python. La carga de Lucida utiliza `trust_remote_code=True`, por lo que también descarga y ejecuta el código de arquitectura suministrado por el repositorio del modelo.



La aplicación requiere una clave de licencia activa (emitida tras la compra de la suscripción mensual) para la primera ejecución y para verificaciones periódicas de estado. Las fotografías y los datos procesados nunca salen de la computadora del usuario; únicamente se envían metadatos cifrados a la API de Lemon Squeezy para validar la vigencia de la licencia



## Alcance y estado del proyecto



La implementación incluye interfaz gráfica, preparación del entorno, procesamiento por lotes y herramientas para generar el ejecutable. Se verificaron la sintaxis y partes del flujo, incluyendo recorte, conversión, conservación de originales, reanudación, selección del entorno y cancelación de subprocesos.



Las pruebas de eliminación de fondo emplearon una salida de Lucida simulada. La inferencia real, la interfaz en Windows y el ejecutable generado están pendientes de validación en un equipo Windows. La documentación describe la implementación disponible; no acredita una versión de producción ya validada.



La precisión de la eliminación de fondo depende de la imagen y del modelo. Los resultados deben revisarse, especialmente en objetos transparentes, bordes finos o fondos complejos. La velocidad depende del procesador, la memoria y la cantidad de fotografías. El programa no aplica un límite de peso en KB a los JPG exportados.



## Naturaleza del producto para revisión comercial



Fotos Limpias es un proyecto de software de escritorio cuya distribución propuesta es un producto digital descargable. Su función es automatizar la preparación de fotografías de productos mediante una aplicación que el comprador ejecuta en su propio equipo.



El valor de la aplicación está en integrar la interfaz, la configuración del entorno, la eliminación de fondo y la organización de los archivos finales en un flujo de trabajo por lotes. El usuario selecciona sus fotografías y el software realiza el procesamiento automáticamente.



La publicación comercial, el precio, las condiciones de uso y el canal de soporte deben corresponder a la oferta que establezca el titular del producto. Esta documentación no establece esas condiciones ni afirma que el proyecto haya sido aprobado por Lemon Squeezy.



## Licencia, Términos de Suscripción y Propiedad Intelectual



**© 2026 Fotos Limpias. Todos los derechos reservados.**



- **Modelo de Licenciamiento:** Fotos Limpias es software propietario distribuido bajo un modelo de **suscripción mensual**. Queda estrictamente prohibida la copia, redistribución, modificación, ingeniería inversa o descompilación del ejecutable sin una clave de licencia válida emitida por el titular.

- **Cancelación:** Si la suscripción mensual se cancela o no se renueva, la clave de licencia quedará desactivada y la aplicación dejará de procesar nuevos lotes de imágenes hasta que la suscripción sea reactivada.

- **Atribución de componentes:** El modelo de IA Lucida (`egeorcun/lucida`) es un componente de terceros utilizado bajo su licencia original MIT. Las dependencias de código abierto conservan sus respectivas licencias de distribució
