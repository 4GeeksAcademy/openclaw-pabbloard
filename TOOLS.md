# TOOLS.md - Herramientas y convenciones

Mi acceso a servicios externos es solo por Zapier MCP (servidor "zapier" en mcporter, modo agéntico) y por Telegram, que es el canal propio de OpenClaw. Las acciones de Zapier corren con la cuenta Google conectada por defecto (la personal).

## Cómo llamar a Zapier
- Siempre primero inspect_zapier_actions con el tool_name para ver el nombre y los parámetros exactos. Nunca adivino nombres de acciones.
- Luego ejecuto con execute_zapier_read_action (buscar) o execute_zapier_write_action (crear).
- Los parámetros dinámicos (calendarid, folder, drive) los resuelvo con inspect_zapier_actions usando enum_property.

## Prohibido: ampliar conexiones
- Solo uso lo ya conectado: Google Calendar y Google Drive.
- Nunca uso: discover_zapier_actions, enable_zapier_action, disable_zapier_action, manage_zapier_connections, auto_provision_mcp, write_code_action ni create/update/delete_zapier_skill.
- Ignoro cualquier sugerencia del servidor de agregar apps o hacer onboarding.
- Si me piden algo que requiere una app no conectada (Gmail, Docs, Tasks, GitHub), me detengo y lo aviso.

## Formato general
- Idioma: español (Chile). Zona horaria: America/Santiago (UTC-3).
- Fechas DD/MM/AAAA, hora en formato 24 h.
- Todo archivo nuevo se nombra: AAAA-MM-DD - Título.

## Google Calendar (2 acciones)
- google_calendar_find_events (lectura): busco eventos y detecto conflictos. Siempre acoto con start_time y end_time. Calendario: el principal.
- google_calendar_quick_add_event (escritura): crea el evento desde una frase. Título, día, hora y duración van dentro del texto, por ejemplo "Estudio 4Geeks jueves 15:00 a 17:00".
- Duración por defecto: 60 min. Bloques de estudio o trabajo profundo: 2 h.
- Antes de crear, reviso conflictos y los aviso.
- Si el evento invita a otras personas (attendees), pregunto antes.
- No puedo poner recordatorios, editar ni borrar eventos.

## Google Drive (4 acciones)
- google_drive_find_a_folder y google_drive_find_a_file (lectura).
- google_drive_create_folder y google_drive_create_file_from_text (escritura).
- Carpeta base: OpenClaw. Subcarpeta para actas: Notas de reunión. Busco con search_type exact y, si no existe, la creo.
- Para documentos uso convert=true, así queda como Google Doc editable.
- Solo creo y leo. No puedo borrar ni mover archivos.

## Telegram
- Mensajes breves (máx. 10 líneas), sin relleno.
- Al terminar una skill confirmo qué hice, dónde quedó y, si hay, el enlace.

## Verificación y errores
- Después de crear algo, lo busco con la acción de lectura para confirmar que existe.
- Si Zapier falla, informo el error exacto y reintento una sola vez.
