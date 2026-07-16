---
name: Resumen diario
description: Un técnico pide un resumen de sus tickets abiertos — qué espera respuesta, qué es urgente, qué está agendado para hoy — legible en menos de un minuto, con una variante ultracorta de 3 líneas. (Spanish version of Daily Digest — skimmable morning summary of the requesting member's open tickets.)
category: Localized
tools: [search_tickets, search_members]
---

# Resumen diario

Versión en español de `skills/personal-productivity/daily-digest` — kept in sync manually.

La lectura de la mañana: todo lo que tienes sobre la mesa, ordenado según lo que te necesita ahora mismo. Pensado para leerse en menos de un minuto y para nombrar la primera tarea concreta del día. Es también el patrón que más partners ejecutan como skill programada de inicio de jornada.

## Cuándo usar

- "Dame un resumen de mis tickets abiertos" / "¿qué necesita respuesta? ¿hay algo urgente?"
- "Resumen de la mañana" / "¿cómo pinta mi día?"
- "La versión corta" — la variante de 3 líneas
- Integrado en un Flow como briefing programado de inicio de jornada

## Pasos

1. Recupera con search_tickets todos los tickets abiertos asignados al miembro que lo solicita. Si un límite de resultados puede haber truncado la lista, dilo desde el principio ("muestro 50 — puede haber más") en lugar de presentar el resumen como completo.
2. Clasifica cada ticket en exactamente un bloque, en este orden de prioridad (el ticket cae en el primer bloque para el que califica):
   - **Espera tu respuesta** — el cliente respondió el último y está esperándote. Ordena por cuánto tiempo lleva esperando.
   - **Urgente / en riesgo** — prioridad alta, SLA al borde o ya incumplido, o hilos con sentimiento negativo. Ordena por gravedad.
   - **Agendado para hoy** — tickets con una entrada de agenda o un seguimiento comprometido para hoy, en orden horario.
   - **Esperando a terceros** — el cliente, un proveedor o una escalación tienen la pelota. Marca los que llevan 3 o más días en silencio (candidatos a un recordatorio), pero no dejes que este bloque acapare la parte alta.
   - **Todo lo demás** — solo el recuento y una caracterización de una línea; sin líneas por ticket.
3. Cada ticket ocupa una sola línea: número, cliente (abreviado), estado en 5–8 palabras y la siguiente acción ("#1234 <cliente> — impresora sin conexión 2 días, el cliente respondió ayer → confirmar que la solución funciona").
4. Encabeza el resumen con **Empieza por aquí:** el ticket más importante y una frase con el porqué (la respuesta de cliente que más lleva esperando gana a una urgencia difusa; un SLA incumplido gana a todo).
5. Cierra con la forma del día: totales por bloque y una frase ("2 respuestas y 1 urgente antes de tu visita agendada de las 10:00 despejan casi toda la presión").
6. Si piden la versión corta (o "3 líneas"), devuelve exactamente tres líneas: (1) Empieza por aquí + el porqué, (2) recuentos — X esperan respuesta / Y urgentes / Z agendados, (3) lo único que te va a morder hoy si lo ignoras. Sin encabezados ni preámbulos.
7. Ofrece la continuación natural: "¿Quieres borradores de respuesta para los que te están esperando?"

## Salvaguardas

- Limita el alcance estrictamente a los tickets del miembro que lo pide; nunca incluyas las colas de otros técnicos salvo petición explícita (eso es un informe de responsable, no un resumen personal).
- Solo lectura: el resumen no cambia nada — ni estados, ni notas, ni recordatorios — salvo que el miembro lo pida como continuación.
- Nunca inventes números de ticket, plazos de SLA ni respuestas de cliente; si el dato de última actividad es ambiguo, coloca el ticket en el bloque más seguro (Espera tu respuesta) en vez de esconderlo en Esperando.
- Cada línea de bloque debe ser accionable — un estado sin siguiente acción es una línea desperdiciada.
- Mantén el resumen completo legible de un vistazo: si no cabe en una pantalla, aprieta las líneas; nunca rellenes.

## Convenciones locales (español)

- Fechas en formato DD/MM/AAAA; en texto corrido, escribe la fecha completa ("martes 15 de julio") para evitar ambigüedades entre regiones.
- Horas en formato de 24 horas ("a las 14:30"); si el desk atiende a clientes latinoamericanos acostumbrados al formato de 12 horas, añade el matiz ("14:30 / 2:30 p. m.").
- El resumen se dirige al propio técnico: el tuteo es natural y correcto aquí. Reserva el usted para los borradores dirigidos a clientes.
- Signos de apertura obligatorios (¿ ¡) en cualquier pregunta o exclamación del resumen.

## Variante desatendida (Flows)

- Tu respuesta completa se entrega literalmente como el briefing de la mañana — sin narración, sin preguntas, sin "avísame si...".
- Incluye siempre la línea Empieza por aquí y los recuentos por bloque; omite el ofrecimiento de continuación.
- Si la cola está vacía, devuelve exactamente: "No tienes tickets abiertos asignados. Disfruta de la bandeja limpia."
- Si la búsqueda de tickets falla o devuelve un vacío inverosímil para un técnico activo, devuelve una sola línea indicando que el resumen no pudo generarse — nunca un resumen inventado.
