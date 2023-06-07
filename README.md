# Proyecto final Aprendizaje Automático 2023 METPOL
Repositorio para el curso del proyecto final de Machine Learning con Python, de la maestría en Métodos para el Análisis de Políticas Públicas. El proyecto consiste en clasificar imágenes que presentas mutación de cáncer de mama.

## Motivación del proyecto

El carcinoma ductal invasivo (IDC, por sus siglas en inglés) es, con aproximadamente el 80% de los casos, uno de los tipos más comunes de cáncer de mama. Es maligno y capaz de formar metástasis, lo que lo hace especialmente peligroso. A menudo, se realiza una biopsia para extraer pequeñas muestras de tejido. Luego, un patólogo debe decidir si un paciente tiene IDC, otro tipo de cáncer de mama o está sano. Además, es necesario localizar las células enfermas para determinar cuán avanzada está la enfermedad y qué grado debe asignarse. Esto debe hacerse de forma manual y es un proceso que consume mucho tiempo. Además, la decisión depende de la experiencia del patólogo y de su equipo. Por lo tanto, el aprendizaje profundo podría ser de gran ayuda para detectar y localizar automáticamente las células del tejido tumoral y acelerar el proceso. Para aprovechar todo su potencial, se podría construir un flujo de trabajo utilizando grandes cantidades de datos de imágenes de tejidos de diversos hospitales que hayan sido evaluados por diferentes expertos. De esta manera, se podría superar la dependencia del patólogo, lo cual sería especialmente útil en regiones donde no hay expertos disponibles.

La base de datos fue tomada de https://www.kaggle.com/datasets/paultimothymooney/breast-histopathology-images

Se propone utilizar una red neuronal convolucional para resolver el problema de clasificación.

## Descripción de los datos.

Las imágenes de este proyecto corresponden a cuadros tomados de muestras de microscopio de tejido de mama de distintas pacientes con tumores benignos o malignos. Para poder trabajar las imágenes utilizando aprendizaje profundo, tenemos una serie de retos relacionadas con el formato de las imágenes, la selección del conjunto de aprendizaje y el conjunto de prueba. 

Los datos están estructurados de la siguiente forma en el directorio de trabajo: Cada paciente cuenta con una carpeta propia, la cual contiene dos subcarpetas (llamadas 1 y 0), las cuales tienen las imágenes de interés. Las imágenes en las carpetas 1 corresponden a tumores malignos, mientras que las imágenes en las carpetas 0 corresponden a tumores benignos.

## Selección del conjunto de aprendizaje y del conjunto de prueba.

Dada la estructura de datos descrita anteriormente, consideramos factible creer que existe un cierto grado de correlación entre las imágenes pertenecientes a un mismo paciente, debido a que pertenecen a una misma muestra de tejido. Por lo tanto, decidimos que para dividir los datos entre conjuntos de entrenamiento y prueba, haríamos una aleatorización inicial a nivel de paciente, con el fin de que la red tenga datos completamente nuevos para probar sus resultados.

Consideramos que esta aleatorización además coincide con una dinámica de la vida real, pues si la red tuviera que realizar nuevas predicciones, los haría sobre imágenes de pacientes que jamás ha visto. Para realizar la aleatorización, primero obtuvimos la lista de pacientes, calculamos el número equivalente al 80% de los pacientes para seleccionar el conjunto de entrenamiento, y realizamos una muestra aleatoria de los pacientes para tener las identidades del conjunto de entrenamiento. El conjunto de prueba fueron los pacientes no seleccionados en el conjunto de entrenamiento.

## Procesamiento de las imágenes.

El preprocesamiento de las imágenes consistió en cargar las imágenes a un objeto numpy, volverla compatible con la red neuronal convolucionada convirtiéndolo en un tensor, y preparar las etiquetas correspondientes a cada imágen.

Para cargar las imágenes, realizamos un ciclo for que extrajo cada imagen en las carpetas de los pacientes y las almacenó en un array numpy. Luego, aplanamos las imágenes dividiéndolas entre 255, lo que permitió "normalizar los colores" presentes en los pixeles, y después convertimos el objeto array en un tensor de dimensiones $(n,w,h,c)$

Las etiquetas las creamos utilizando un ciclo for donde extrajimos si la imagen en cuestión pertenecía a una subcarpeta 1 o 0. Luego, este vector lo convertimos en un array de dimensiones $(n,2)$, donde cada entrada se podía clasificar como $[1,0]$ si no presentaba cancer, o $[0,1]$ si presentaba cancer.

## Red neuronal convolucional.

Para este proyecto, diseñamos una red neuronal convolucional propia, con el objetivo de tener pocos parámetros (menos de 200,000) para no que fuera replicable en equipos de poder computacional medio.

La red que diseñamos alcanzó un accuracy de 0.85 al trabajar en el conjunto de prueba, por lo que consideramos que es una red viable para instituciones que no cuenten con computadoras especializadas, pero tienen suficiente poder computacional para procesar imágenes similares a las que trabajamos.
