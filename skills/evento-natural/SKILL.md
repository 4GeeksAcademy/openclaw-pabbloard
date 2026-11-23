---
name: evento-natural
description: Crea un evento en Google Calendar a partir de una frase en lenguaje natural, revisando antes si hay conflictos de horario. Úsala cuando el usuario pida agendar, reservar o bloquear tiempo, por ejemplo una sesión de estudio el jueves por la tarde.
---

# evento-natural

Antes de actuar, aplica lo definido en SOUL.md (tono), USER.md (contexto), AGENTS.md (cuándo parar y preguntar) y TOOLS.md (zona horaria, duraciones y calendario por defecto).

## Entrada
- Una frase que describe el evento: qué, cuándo y, si se dice, cuánto dura.
- Si la hora es ambigua (por ejemplo por la tarde), propón una hora concreta y déjala explícita en la confirmación.

## Pasos
1. Interpreta la frase en zona America/Santiago. Si no hay duración, usa 60 minutos; para bloques de estudio o trabajo profundo, 2 horas.
2. Si el evento invita a otras personas, detente y pregunta antes de seguir.
3. Revisa conflictos con google_calendar_find_events en la franja del evento, en el calendario principal, acotando con start_time y end_time. Si hay un evento que se cruza, avísalo y pregunta si lo creas igual.
4. Escribe una frase clara con título, día, hora de inicio y hora de término, y créala con google_calendar_quick_add_event. Ejemplo de frase: Sesión de estudio 4Geeks jueves 15:00 a 17:00.
5. Verifica con google_calendar_find_events que el evento aparece a la hora esperada.
6. Responde por Telegram en máximo 5 líneas: qué se creó, día y hora, y si había algún conflicto.

## Cómo llamar a Zapier
- Usa mcporter con el servidor zapier. Primero inspect_zapier_actions con el tool_name para ver los parámetros exactos y luego execute_zapier_read_action o execute_zapier_write_action.
- El parámetro calendarid es dinámico: resuélvelo con inspect_zapier_actions usando enum_property y elige el calendario principal.
- Nunca adivines nombres de acciones ni de parámetros.

## Límites
- Solo Calendar y Telegram. No uses nada que no esté conectado.
- Quick Add no permite recordatorios ni editar o borrar eventos: si te lo piden, dilo en vez de inventarlo.
- Nunca uses discover_zapier_actions, enable_zapier_action, manage_zapier_connections, auto_provision_mcp ni write_code_action.
- Si Zapier falla, informa el error exacto y reintenta una sola vez.
