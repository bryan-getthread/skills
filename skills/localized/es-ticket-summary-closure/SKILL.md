---
name: Resumen y nota de cierre
description: Producir un resumen limpio de lo ocurrido en un ticket — nota de resolución, nota de cierre o resumen de traspaso P1/P2 con plantilla — en el formato y el punto de vista solicitados. (Spanish version of Ticket Summary & Closure Note — resolution, closure, and handoff summaries.)
category: Localized
tools: [search_tickets, add_ticket_note]
---

# Resumen y nota de cierre

Versión en español de `skills/documentation/ticket-summary-and-closure-note` — kept in sync manually.

Resume el hilo de un ticket en la nota exacta que la situación exige: nota de resolución o cierre, recapitulación de "qué se hizo", o resumen de traspaso de escalación con plantilla.

## Cuándo usar

- "Escribe una nota de cierre para este ticket" / "resume qué se hizo."
- "Dame una nota de resolución en nuestro formato estándar."
- Un P1/P2 pasa a otro técnico o a un equipo fuera de horario y necesita un resumen de traspaso estructurado.
- "Resúmelo en primera persona para mi registro de tiempo / nota del PSA."

## Pasos

1. Lee el hilo completo con `search_tickets`: respuestas, notas internas y registros de tiempo. Establece problema → acciones realizadas → resultado → pendientes.
2. Elige el formato que pide la solicitud:
   - **Nota de resolución / cierre** — Problema, Acciones realizadas, Resultado y (solo si queda trabajo) un único Próximo paso con responsable y fecha límite.
   - **Resumen de traspaso P1/P2** — plantilla fija: Estado actual; Impacto (quién/qué está caído); Cronología de eventos clave con marcas de tiempo; Acciones realizadas hasta ahora; Qué se ha descartado; Próxima acción + responsable; Estado de la comunicación con el cliente (a quién se le dijo qué y cuándo).
   - **Recapitulación simple** — prosa breve para un mensaje de cierre visible por el cliente.
3. Aplica el punto de vista solicitado: por defecto tercera persona, o **primera persona del técnico** ("Restablecí el perfil y verifiqué el inicio de sesión") cuando lo pidan — escribe como el técnico asignado, no como una IA.
4. Aplica las reglas de formato de la skill **note-format-standard** del desk si está cargada. Si la nota se sincronizará con un PSA, usa **solo texto plano**: sin markdown, sin emojis, sin comillas tipográficas, sin rayas.
5. Muestra la nota en el chat para revisión. Publícala con `add_ticket_note` solo cuando lo pidan explícitamente, y publica los resúmenes de traspaso/QA como notas **internas** salvo indicación contraria.

## Salvaguardas

- No inventes detalle que el hilo no respalde — nada de confirmaciones, marcas de tiempo ni causas raíz fabricadas. Si el resultado no está claro, escribe "resultado no confirmado en el hilo" en lugar de afirmar la resolución.
- Sigue el formato solicitado con precisión, incluidas restricciones como "sin viñetas" o límites de palabras.
- La primera persona cambia solo la voz — nunca añadas trabajo que el técnico no dejó registrado.
- En los resúmenes de traspaso, separa explícitamente hecho de hipótesis ("Confirmado:" frente a "Sospecha:").
- Nunca incluyas en un resumen credenciales ni códigos de un solo uso encontrados en el hilo.

## Convenciones locales (español)

- Marcas de tiempo en la cronología: DD/MM/AAAA HH:MM en formato de 24 horas ("14/07/2026 09:45"). Si intervienen varias zonas horarias, indícala ("09:45 CET").
- En notas visibles por el cliente, registro de usted; en notas internas y traspasos, el tono profesional directo (tuteo entre colegas) es correcto.
- Para texto plano destinado al PSA, evita también los caracteres que algunos PSA antiguos estropean: mantén ¿ ¡ y tildes (son ortografía, no adorno), pero nada de comillas tipográficas ni guiones largos.
- Etiquetas de plantilla en español coherentes en todo el desk ("Confirmado:" / "Sospecha:"; "Próxima acción:"); no alternes con las etiquetas inglesas dentro de una misma nota.

## Consolida

Peticiones de nota de cierre, resúmenes de resolución, plantillas de traspaso P1/P2 y skills de resumen situacional de "próximo paso".
