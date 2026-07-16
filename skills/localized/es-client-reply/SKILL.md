---
name: Respuesta al cliente
description: Redactar una respuesta externa al cliente con la voz y el formato de la casa — actualizaciones de resolución, notas de estado, mensajes de cierre o cualquier correo de cara al cliente sobre un ticket. (Spanish version of Client Reply — drafts on-brand client-facing ticket replies.)
category: Localized
tools: [search_tickets, view_openDraft, add_ticket_note]
---

# Respuesta al cliente

Versión en español de `skills/communication/client-reply` — kept in sync manually.

**Cuándo usar:** un técnico necesita enviar un mensaje de cara al cliente y lo quiere alineado con la marca y listo para enviar.

**Qué obtienes:** un borrador que empieza por la respuesta, cumple los estándares de voz de la casa y es seguro enviar tal cual.

## Cuándo usar

- "Redacta una respuesta al cliente en este ticket."
- "Dile a <user> que su cuenta está desbloqueada."
- "Escribe una respuesta explicando que seguimos trabajando con el proveedor."
- Cualquier correo de cara al cliente donde el tono, el formato y la exactitud importan.

## Pasos

1. Lee el hilo completo del ticket antes de escribir una sola palabra — cada mensaje previo, nota y cambio de estado. Nunca redactes solo a partir del último mensaje.
2. Redacta la respuesta empezando por la respuesta en sí o por el estado actual, y después el detalle de apoyo.
3. Aplica los estándares de voz de la casa:
   - Saluda al cliente por su nombre de pila solo en el **primer** mensaje del hilo; omite el saludo en las respuestas siguientes del mismo hilo.
   - Sin frases de relleno ("Espero que este correo le encuentre bien", "Me pongo en contacto para..."). Empieza con sustancia.
   - Sin jerga técnica salvo que el cliente haya usado el término primero; si un término técnico es inevitable, añade una explicación en lenguaje llano.
   - 1–2 párrafos cortos. Si necesita más, probablemente necesita una llamada.
   - Estructura según la skill Email Baseline Standard (communication/email-baseline-standard).
4. Si el miembro tiene configurada una lista de palabras preferidas o prohibidas (una skill de contexto personal o una nota de estilo de la casa), aplícala: sustituye las palabras prohibidas por sus reemplazos designados y prioriza las alternativas listadas.
5. Si es una respuesta de cierre, termina el cuerpo con la línea de reapertura: "Si algo no quedó resuelto, solo tiene que responder a este correo: al responder, el ticket se reabre automáticamente."
6. Firma: si el espacio de trabajo aplica una firma gestionada de forma automática, termina tras la última frase del cuerpo — NO añadas una despedida ("Saludos cordiales, ...") que quedaría duplicada. Añade despedida solo cuando no exista firma gestionada.
7. Presenta la respuesta como borrador abierto con view_openDraft para que el técnico la revise. No la envíes.

## Salvaguardas

- Nunca inventes detalles: nada de marcas de tiempo, pasos realizados, números de ticket, enlaces ni promesas fabricadas. Si el hilo no establece un hecho, omítelo o márcalo como <confirmar con el técnico>.
- El borrador sigue siendo borrador hasta que un humano lo apruebe. Nunca envíes de forma autónoma.
- Nunca expongas notas internas, credenciales, precios ni datos de otros clientes en una respuesta de cara al cliente.
- Ajusta el idioma al del cliente: si el hilo está en otro idioma, redacta en ese idioma (véase communication/multilingual-reply para el flujo de retrotraducción).
- Degradación: view_openDraft solo está disponible dentro de la aplicación. Por MCP externo, entrega el borrador terminado en el chat, claramente etiquetado como "BORRADOR — revisar antes de enviar".

## Convenciones locales (español)

- Registro por defecto: **usted** en toda comunicación comercial con clientes. Si el cliente tutea de forma consistente en el hilo (habitual en startups y en parte de la cultura empresarial de algunos países), refleja su registro — pero nunca seas tú quien pase primero al tuteo.
- Despedidas apropiadas: "Saludos cordiales" o "Un saludo" (habitual), "Atentamente" (muy formal). Evita calcos del inglés como "Mejores saludos".
- Fechas en el cuerpo del correo: "el martes 15 de julio", nunca 07/15. Horas en formato de 24 horas.
- Trata de "usted" pero saluda por el nombre de pila ("Estimada María:" o "Hola, María:") — es el equilibrio estándar en el soporte técnico en español. Recuerda que muchos clientes usan dos apellidos; no recortes ni combines apellidos al dudar.
- Equivalente local de la línea de reapertura: "Si algo no quedó resuelto, solo tiene que responder a este correo: al responder, el ticket se reabre automáticamente."

## Consolida

Skills de redacción/envío de respuestas por correo, formato estándar de respuesta al cliente, respuesta con una voz determinada, tono, palabras prohibidas y firmas.
