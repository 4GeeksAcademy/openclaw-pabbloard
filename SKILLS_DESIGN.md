# SKILLS_DESIGN.md - Diseño de skills de mi agente OpenClaw

Alcance: solo uso lo que ya está conectado, sin APIs ni OAuth nuevos. Servicios disponibles: Google Drive (buscar y crear carpetas y archivos), Google Calendar (buscar eventos y crear eventos con texto) y Telegram (canal del agente). Gmail, Docs, Tasks y GitHub no están conectados, por eso no se usan.

Cada skill se apoya en los cinco archivos de configuración: IDENTITY.md (cómo me habla y firma), SOUL.md (tono y forma de trabajar), AGENTS.md (límites y cuándo preguntar), USER.md (mi contexto y proyectos) y TOOLS.md (servicios, carpetas y valores por defecto).

## Skill 1: notas-de-reunion

Por qué esta skill: después de cada reunión tengo apuntes desordenados y reescribirlos a mano es una tarea repetitiva que se me pierde. Se activa varias veces por semana.

1. ¿Qué hace? Convierte apuntes en bruto de una reunión en un acta ordenada (resumen, decisiones, tareas y preguntas abiertas) y la guarda como Google Doc en Drive.
2. ¿Qué input necesita?
- Lo que le doy: el texto de los apuntes, pegado por Telegram. Opcional: nombre de la reunión y fecha; si faltan, usa la fecha de hoy y propone un título.
- Lo que ya sabe por los cinco archivos: mi idioma y tono (SOUL.md), mis proyectos y siglas para interpretar los apuntes (USER.md), la carpeta OpenClaw / Notas de reunión y la convención de nombres (TOOLS.md), y que debe parar y preguntar si algo es ambiguo o delicado (AGENTS.md).
3. ¿Cómo es un buen output?
- Un Google Doc llamado "AAAA-MM-DD - Título" dentro de OpenClaw / Notas de reunión, con las secciones Resumen, Decisiones, Tareas (con responsable y fecha si existen) y Preguntas abiertas.
- Un mensaje de Telegram de máximo 10 líneas con el nombre del archivo y dónde quedó.
- Cómo sabré que funcionó: la skill busca el archivo con la acción de lectura de Drive y lo encuentra en la carpeta correcta, y el documento abre como Google Doc editable.
- Acciones de Zapier que usa: google_drive_find_a_folder, google_drive_create_folder (solo si falta la carpeta), google_drive_create_file_from_text con convert=true y google_drive_find_a_file para verificar.

## Skill 2: evento-natural

Por qué esta skill: agendar bloques de estudio y reuniones implica abrir el calendario y rellenar campos. Quiero describirlo en una frase y que el agente se encargue de los detalles y de avisarme si choca con algo.

1. ¿Qué hace? Crea un evento en Google Calendar a partir de una descripción en lenguaje natural, revisando antes si hay conflictos en ese horario.
2. ¿Qué input necesita?
- Lo que le doy: una frase por Telegram, por ejemplo "sesión de estudio el jueves por la tarde, dos horas".
- Lo que ya sabe por los cinco archivos: mi zona horaria America/Santiago, la duración por defecto (60 min, y 2 h para bloques de estudio), el calendario principal y la regla de preguntar antes de invitar a otras personas (TOOLS.md y AGENTS.md), y mi forma de hablar (SOUL.md). Si la hora es ambigua (por ejemplo "por la tarde"), propone una hora concreta y la deja explícita en la confirmación.
3. ¿Cómo es un buen output?
- Un evento en mi calendario con título claro, día, hora de inicio y duración correctos, escrito en el texto que se envía a Quick Add.
- Un mensaje de Telegram que confirma qué se creó y cuándo, y avisa si había un conflicto.
- Cómo sabré que funcionó: la skill vuelve a buscar con google_calendar_find_events en esa franja y el evento aparece a la hora esperada.
- Acciones de Zapier que usa: google_calendar_find_events (antes y después) y google_calendar_quick_add_event.

## Límites conocidos del diseño
- Quick Add Event no permite recordatorios, editar ni borrar eventos; lo que no pueda hacer, la skill lo dice en vez de inventarlo.
- Drive solo permite crear y leer; no se borra ni se mueve nada.
- Si falta una conexión que la skill necesita, se detiene y avisa; nunca agrega una app nueva.
