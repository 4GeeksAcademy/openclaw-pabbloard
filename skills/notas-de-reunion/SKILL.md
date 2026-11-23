---
name: notas-de-reunion
description: Convierte apuntes en bruto de una reunión en un acta (resumen, decisiones, tareas y preguntas abiertas) y la guarda como Google Doc en Drive. Úsala cuando el usuario pegue apuntes de una reunión o pida guardar las notas de una reunión.
---

# notas-de-reunion

Antes de actuar, aplica lo definido en SOUL.md (tono), USER.md (contexto y siglas), AGENTS.md (límites y cuándo preguntar) y TOOLS.md (carpetas, formato y convenciones).

## Entrada
- Los apuntes en texto (obligatorio).
- Nombre de la reunión y fecha (opcionales). Si faltan, usa la fecha de hoy en America/Santiago y propón un título corto según el contenido.

## Pasos
1. Ordena el contenido en cuatro secciones: Resumen (2 o 3 líneas), Decisiones, Tareas (con responsable y fecha solo si aparecen en los apuntes) y Preguntas abiertas. No inventes decisiones, responsables ni fechas. Lo ambiguo va a Preguntas abiertas.
2. Nombra el archivo: AAAA-MM-DD - Título.
3. Resuelve la carpeta OpenClaw y dentro de ella Notas de reunión con google_drive_find_a_folder (search_type exact). Si falta alguna, créala con google_drive_create_folder.
4. Crea el documento con google_drive_create_file_from_text: file con el acta en texto plano, title con el nombre, folder con la carpeta resuelta y convert en true, para que quede como Google Doc editable.
5. Verifica con google_drive_find_a_file que el archivo existe con ese nombre.
6. Responde por Telegram en máximo 10 líneas: nombre del archivo, carpeta y un resumen de decisiones y tareas.

## Cómo llamar a Zapier
- Usa mcporter con el servidor zapier. Primero inspect_zapier_actions con el tool_name para ver los parámetros exactos y luego execute_zapier_read_action o execute_zapier_write_action.
- Los valores de folder y drive son dinámicos: resuélvelos con inspect_zapier_actions usando enum_property.
- Nunca adivines nombres de acciones ni de parámetros.

## Límites
- Solo Drive y Telegram. No uses nada que no esté conectado.
- No borres ni muevas archivos.
- Nunca uses discover_zapier_actions, enable_zapier_action, manage_zapier_connections, auto_provision_mcp ni write_code_action.
- Si Zapier falla, informa el error exacto y reintenta una sola vez.
