---
name: Cadencia de seguimiento de tickets inactivos
description: Ejecutar una cadencia de seguimiento configurable (por ejemplo 24/48/72 horas, tres intentos) sobre tickets a la espera de respuesta del cliente — redactar cada recordatorio amable, contar los intentos documentados y omitir los tickets en esperas legítimas. (Spanish version of Stale Ticket Follow-Up Cadence.)
category: Localized
tools: [search_tickets, add_ticket_note, update_ticket, schedule_ticket]
---

# Cadencia de seguimiento de tickets inactivos

Versión en español de `skills/qa-and-closure/stale-ticket-followup-cadence` — kept in sync manually.

Mantiene en movimiento los tickets a la espera del cliente con un ritmo predecible. Cuenta los intentos ya documentados en el hilo, redacta el siguiente recordatorio con una voz amable de cara al cliente, y sabe cuándo un ticket espera por un buen motivo y hay que dejarlo tranquilo.

## Cuándo usar

- "Haz seguimiento de mis tickets en espera" / "¿quién no nos ha respondido?"
- Ejecutar la cadencia estándar del desk (24/48/72 horas, o la variante de la casa) sobre una cola.
- Un técnico quiere redactado el siguiente recordatorio para un ticket concreto que está en silencio.
- Integrado en un Flow con programación horaria o con temporizador de estado en espera (ver variante desatendida).

## Pasos

1. Confirma la cadencia a aplicar. Por defecto: primer seguimiento 24 horas después del último mensaje saliente visible por el cliente, segundo a las 48, tercero a las 72 — tres intentos en total, y después traspaso a la skill No-Response Closure Sequence. Respeta una cadencia propia del desk si el usuario la indica ("3 días / 3 intentos / 3 correos" es una regla de la casa frecuente).
2. Encuentra candidatos con `search_tickets`: tickets abiertos en estados de espera-del-cliente donde el último mensaje es saliente y más antiguo que el paso de cadencia vigente.
3. Para cada candidato, cuenta los **intentos documentados**: mensajes de seguimiento visibles por el cliente desde la última respuesta del cliente. Solo cuentan los mensajes que el cliente pudo ver de verdad — las notas internas no son intentos.
4. Omite las esperas legítimas: una cita futura agendada (fecha de `schedule_ticket`), un plazo declarado por el cliente ("vuelvo de vacaciones el lunes"), una dependencia de proveedor o tercero, o una aprobación pendiente. No persigas a quien ya te dijo cuándo va a responder.
5. Redacta el seguimiento de cada ticket restante en la voz de cara al cliente: amable, breve, con una línea que recuerda el asunto original, indicando qué se necesita de él y ofreciendo una salida fácil ("si ya está resuelto, díganoslo y lo cerramos"). Sube el tono con suavidad entre intentos — el tercer intento avisa de que el ticket se cerrará pronto sin respuesta, y nunca suena molesto.
6. Presenta los borradores para revisión y, tras la aprobación, publica cada seguimiento como nota visible por el cliente con `add_ticket_note` y fija el estado de espera adecuado con `update_ticket`.
7. Devuelve una tabla resumen: ticket, cliente, número de intento recién enviado (u omitido y por qué), y la fecha en que vence el siguiente paso de la cadencia.

## Salvaguardas

- Nunca superes el número de intentos: si el hilo ya muestra tres intentos documentados, deriva a la secuencia de cierre en lugar de insistir una cuarta vez.
- Cuenta solo lo documentado — nunca des por hecho un intento telefónico salvo que una nota lo registre.
- Los borradores de cara al cliente no llevan jerga interna, ni texto enlatado del sistema de tickets, ni detalles inventados sobre el asunto.
- La lista de omisiones es sagrada: perseguir por error a un cliente de vacaciones o a la espera de una visita agendada erosiona la confianza. Si la legitimidad de la espera no está clara, omite y marca en vez de enviar.
- Nunca envíes seguimientos en bloque sin que el usuario revise los borradores.
- Lenguaje sencillo y localizable; escribe en el idioma del cliente cuando el hilo no esté en español.

## Convenciones locales (español)

- Registro de usted en todos los recordatorios de cara al cliente; refleja el tuteo solo si el cliente lo usa de forma consistente en el hilo.
- Fórmulas naturales de recordatorio: "Le escribimos para retomar su consulta sobre..." o "¿Ha podido revisar nuestro último mensaje?" — nunca calcos como "Solo hago seguimiento".
- En el tercer intento, la advertencia de cierre en positivo: "Si no recibimos respuesta, cerraremos el ticket en los próximos días; si algo sigue pendiente, basta con responder a este correo y el ticket se reabre."
- Fechas en los borradores: "el lunes 20 de julio", no 20/7 a secas; en la tabla resumen interna, DD/MM está bien.
- Cuenta los plazos en días laborables cuando la política del desk lo indique; agosto y las fiestas locales alargan las esperas legítimas — ante la duda, omite y marca.

## Variante desatendida (Flows)

- Disparador: programación (p. ej. barrido horario) o temporizador de antigüedad en estado de espera.
- Tu respuesta completa es el mensaje de seguimiento de cara al cliente, publicado literal — sin narración, sin contadores de intentos, sin comentario interno. No escribas nada más.
- Reglas deterministas: envía solo cuando (a) el ticket está en un estado de espera-del-cliente, (b) el último mensaje es saliente, (c) el paso de cadencia ha vencido y (d) los intentos documentados están por debajo del máximo. Si falla cualquier condición, o la espera parece legítima, no produzcas salida y detente.
- Nunca redactes desatendido el tercer intento o posteriores si el desk exige visto bueno humano antes de los avisos finales — comprueba el techo de intentos configurado y detente por debajo.
