# Sky2Travel - Asistente de Viajes

Sky2Travel es un asistente virtual diseñado para ayudar a los usuarios a buscar vuelos y planificar viajes. Utilizando una combinación de APIs y modelos de lenguaje avanzados, el proyecto ofrece una experiencia interactiva y en tiempo real que incluye procesamiento de lenguaje natural, traducción automática, generación de respuestas estructuradas y síntesis de voz.

## Funcionalidades Clave

- **Interfaz Conversacional:**  
  El asistente interactúa con el usuario a través de mensajes de texto y voz. Al iniciar, saluda al usuario y, conforme se desarrollan las consultas, utiliza respuestas generadas por voz para hacer la experiencia más dinámica.

- **Procesamiento del Lenguaje Natural:**  
  Se utiliza el modelo generativo de Google Gemini para interpretar las consultas del usuario. El mensaje se envía junto con un *prompt* cuidadosamente diseñado que:
  - Define el rol del asistente como experto en viajes.
  - Especifica que solo se deben tratar temas relacionados con vuelos y viajes.
  - Indica al modelo que extraiga información esencial (origen, destino, fecha, cantidad de pasajeros y aerolínea) en formato JSON.

- **Manejo de Datos y Validación:**  
  Una vez recibido el JSON del modelo, el código:
  - Valida y procesa la respuesta.
  - Si la respuesta no es un JSON válido, se utiliza una extracción manual con expresiones regulares para obtener los campos necesarios.
  - Si el origen es desconocido, se solicita al usuario que lo ingrese manualmente.

- **Integración de APIs Externas:**
  - **Traducción de Ciudades:**  
    Se usa la API de Google Translate para convertir nombres de ciudades al inglés, asegurando una búsqueda adecuada en las APIs de aeropuertos.
  - **Obtención de Códigos IATA:**  
    La API de Air-Port-Codes se utiliza para obtener el código IATA y el nombre del aeropuerto, tanto de origen como de destino.
  
- **Síntesis de Voz:**  
  La biblioteca gTTS (Google Text-to-Speech) se integra para convertir respuestas de texto a archivos de audio, proporcionando mensajes de bienvenida, confirmación y error a través de voz.

- **Filtrado de Contenidos Inapropiados:**  
  Se implementa un sistema de verificación para evitar respuestas sobre temas no relacionados (como armas o violencia). Si se detecta alguno de estos términos, el asistente responde indicando que solo puede ayudar en temas de viajes.

## Detalle del *Prompt* al Modelo

El *prompt* enviado al modelo de Google Gemini es esencial para estructurar la respuesta. Este *prompt*:

- Define al asistente como un experto en viajes, especificando claramente que solo debe tratar temas relacionados con vuelos.
- Instruye al modelo para que extraiga información crítica en un formato JSON específico:
  - **origen:** Ciudad o país de origen. Si no se menciona, se intenta inferir o se devuelve "Desconocido".
  - **destino:** Ciudad o país de destino.
  - **fecha:** Fecha del viaje en formato `dd-mm-yyyy` o `null` si no se menciona.
  - **cantidad:** Número de pasajeros (por defecto 1).
  - **aerolinea:** Nombre de la aerolínea o `null` si no se menciona.
  
Esta estructura permite que, independientemente de cómo se formule la pregunta del usuario, se extraigan y organicen los datos necesarios para continuar el proceso de búsqueda y confirmación del viaje.

## Flujo de Trabajo

### Inicio y Bienvenida:
Al iniciar, el asistente saluda tanto por texto como por voz, invitando al usuario a hacer su consulta.

### Consulta del Usuario y Procesamiento:
- El usuario ingresa una consulta de viaje.
- Se verifica si el mensaje contiene temas no permitidos. Si es así, se notifica al usuario.
- La consulta se envía al modelo de Google Gemini junto con el prompt definido.
- Se procesa la respuesta JSON para extraer los datos relevantes.

### Validación y Obtención de Datos Externos:
- Si el origen no está definido, se solicita al usuario que lo ingrese.
- Se traducen las ciudades al inglés para realizar búsquedas en la API de aeropuertos.
- Se obtienen los códigos IATA y nombres de los aeropuertos.

### Confirmación y Respuesta:
El asistente confirma la búsqueda del vuelo, proporcionando detalles como:
- Aeropuerto de origen y destino (con sus códigos IATA).
- Fecha del viaje.
- Número de pasajeros.
- Aerolínea (inferida en caso de no ser especificada).

### Ciclo de Consulta:
Tras completar una búsqueda, se pregunta al usuario si desea realizar otra consulta o salir.




