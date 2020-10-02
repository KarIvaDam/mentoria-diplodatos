# Practico 3 - Mentoria - DiploDatos Famaf

Contenidos:

- notebook creación del dataset `dataset_creation.ipynb`

- notebook practico 3 + informe

# Informe Final
En esta investigación se presenta como caso de estudio la clasificación de temas musicales en diferentes géneros. Para poder llevar a cabo el estudio se recolectaron datos de una playlist compartida, almacenada en la plataforma de spotify.

El objetivo central de esta investigación es introducirnos en el mundo de Ciencia de Datos e Inteligencia Artificial, aplicando los conocimientos adquiridos durante el cursado de la Diplomatura de Ciencia de Datos e Inteligencia Artificial y sus aplicaciones.
Para el proceso de recolección de datos se recurrió a la librería de spotipy, mediante el cual se descargaron todo los temas de la lista de reproducción junto con datos característicos de cada tema musical, tales como, nombre, género, artista, etc. También se utilizó una librería llamada lyrics para obtener las letras de cada canción.
Para llevar a cabo esta investigación se seleccionó como valores representativos las siguientes variables: 
* song_name: cadena de caracteres que representan el nombre del tema musical.
* song_id: identificador único del tema dentro de la plataforma de spotify, representado mediante una cadena de caracteres alfanuméricos.
* artists: nombres de los artistas que cantaron e interpretaron el tema, representado mediante una lista de cadenas de caracteres.
* artists_id: identificador único de los artistas cantaron e interpretaron el tema, representado mediante un lista de valores alfanuméricos.
* album_name: nombre del álbum musical  donde fue lanzado el tema. Representado mediante una cadena de caracteres.
* album_id: identificador único del álbum donde fue lanzado el tema. Representado mediante una valor alfanumérico.
* audio_features: información referente a las características técnicas del tema musical, tales como nivel de energía, ritmos, presencia instrumental, etc. Representado mediante una estructura de diccionario.
* genres: lista de los géneros del tema musical relacionado a través del o los artistas.
* lyrics: letras de los temas musicales obtenidas mediante la api de lyrics, usadas posteriormente para hacer un procesamiento de lenguaje natural y obtener una feature para nuestros modelos.

En este proceso de recolección de datos nos encontramos con diferentes problemas, los cuales, pusieron a la vista las diferentes situaciones que un profesional en ciencia de datos pueden encontrar durante toda una investigación, llevando el mismo por diferentes rumbos. Entre los problemas encontrados en este proceso fueron:
* Un tamaño de muestra no representativo de la población.
* Una muestra desbalanceada.
* Dificultad de poder obtener todas las letras de los temas en español.
* Interpretación de las características técnicas de los temas. Conocimiento del entorno.

Uno de los resultados deseados de nuestra investigación, es obtener modelos de clasificación de géneros musicales adecuados, para realizar clasificaciones de temas deseados. Para ello realizamos un proceso de selección de features para entrenar nuestros modelos, los cuales se detallan a continuación:
* Key : denota la tonalidad en la que suena el tema musical. Sus valores corresponden a pitch class
* mode: indica el modo musical (mayor o menor) en el que suena el tema musical.
* energy: define que tan intenso/enérgico es el tema. Los valores rondan entre 0 y 1, valor 0 para el menos intenso.
* tempo: corresponde a la velocidad del tema musical, un valor mayor a 110 es considerado un tema alegre, rápido.
* acousticness: valores entre 0 y 1, un valor 1 para el tema más acústico.
* danceability: define que tan acorde es el tema para bailar. Presenta valores entre 0 y 1, el valor cero para el menor apto para bailar.
* instrumentalness: identifica que presencia instrumental tiene el tema. Con un valor cercano a 1 mayor será la probabilidad que el tema no tenga contenido vocal.
* valence: esta variable describe la positividad del tema musical. Un valor cercano a 1 definiría al tema como alegre y positivo. 
* liveness: identifica el grado de participación de audiencia en la grabación. Un valor cercano a 1, mayor probabilidad a un grabación del tema en vivo.
* loudness: The overall loudness of a track in decibels (dB). Loudness values are averaged across the entire track and are useful for comparing relative loudness of tracks. Loudness is the quality of a sound that is the primary psychological correlate of physical strength (amplitude). Values typical range between -60 and 0 db.
* speechiness: Speechiness detects the presence of spoken words in a track. The more exclusively speech-like the recording (e.g. talk show, audio book, poetry), the closer to 1.0 the attribute value. Values above 0.66 describe tracks that are probably made entirely of spoken words. Values between 0.33 and 0.66 describe tracks that may contain both music and speech, either in sections or layered, including such cases as rap music. Values below 0.33 most likely represent music and other non-speech-like tracks.
* lyrics_sentiment: esta feature se obtuvo de procesar las letras de los temas para ellos se aplicaron la metodologías de Procesamiento de Lenguaje Natural junto con análisis de sentimientos, el cual es un tarea de clasificación masiva de documentos en función de la connotación positiva o negativa del lenguaje ocupado en el momento.

Para poder entrenar nuestros modelos durante la investigación se llevó a cabo, previamente, las tareas de recolección, limpieza y curación de los datos. Durante este proceso se identificaron diferentes problemas previamente mencionados. 
Durante la recolección nos encontramos con nuestro primer problema, dificultad para obtener el 100% de las letras de los temas de la playlist, solo pudimos obtener el 80% del mismo. Intentamos completar el dataset recurriendo a otras librerías como Musixmatch pero por cuestiones de licencia decidimos trabajar con el 80% de los datos.
Otro problema con el que tuvimos que lidiar fue con el desbalanceo de los datos y uno de planteos para trabajarlos fue eliminar los géneros que presentan mayor frecuencia (Argentine Rock, Latin and Cuarteto), con lo cual se logró reducir levemente el desbalanceo como se muestra en los barplots.
Una de las situaciones interesantes al momento de comenzar con el entrenamiento de los modelos, fue que nuestro target (género)  modelaba un problema multilabel, un tema puede pertenecer a más de un género. Dado este escenario, y a modo de adquirir más conocimientos,  se decidió entrenar  modelos multiclass y multilabel.
Para poder trabajar con los modelos multicast se tomó como heurística, asignar como único género de un tema al primer género de la lista de géneros. Se dividió el dataset original en 2 grupos, test y entrenamiento. Del grupo de entrenamiento se separó un grupo de validación para validar nuestro modelos antes de obtener el accurancy con nuestro grupo de test. 
Previo a entrenar nuestros modelos, graficamos la correlación entre la feature del dataset en un hetmap. Nos resulto una herramienta muy interesanque para comprender la relación entre las variables, muy util a la hora de selección de fueature para los modelos.
 se uso una herramienta muy interesante que no permitio entender la relaciones entre la variables,
Se entrenaron los siguientes modelos: 
* XGBOOST
* Decision Tree Classifier
* SGDClassifier
* Random Forest Classifier
* KNeighborsClassifier
* SVM.LinearSVC
Para poder identificar el mejor modelo se utilizaron diferentes scoring tales como accurrancy, precision score, recall score y F1 score.