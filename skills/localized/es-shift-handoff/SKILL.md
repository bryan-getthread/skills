---
name: Relevo de turno
description: Producir un resumen de una página, legible de un vistazo, del trabajo abierto al final del turno o de la jornada, agrupado por estado, para el turno entrante o para un técnico receptor concreto. (Spanish version of Shift Handoff — end-of-shift one-pager of open work.)
category: Localized
tools: [search_tickets, add_ticket_note, log_time_entry, search_members]
---

# Relevo de turno

Versión en español de `skills/escalation/shift-handoff` — kept in sync manually.

Construye un relevo de una página que se lee en un minuto: cada hilo abierto agrupado por estado, con qué se hizo, qué toca ahora, qué lo bloquea y su riesgo. Se acota a un único técnico receptor cuando se nombra a uno.

## Cuándo usar

- Final de turno o de jornada: "pasa mi trabajo abierto al turno de noche."
- "Escribe un relevo para <member> — mañana cubre mi cola."
- Un responsable quiere el trabajo abierto del equipo resumido antes del cambio de turno.

## Pasos

1. Establece el alcance. Si se nombra a un único técnico receptor, traspasa solo lo que esa persona necesita para actuar (los tickets abiertos/en curso del técnico saliente, más lo marcado explícitamente para ella) — no la cola de todo el equipo. En caso contrario, cubre el trabajo abierto del equipo para el turno entrante.
2. Recupera los tickets abiertos y en curso con search_tickets. Si alguna búsqueda toca el límite de resultados, dilo en la salida en lugar de presentar la lista como completa; divide las búsquedas por tablero o por estado cuando barras en ancho.
3. Agrupa la página por estado (p. ej. En curso / Esperando al cliente / Esperando al proveedor / Agendado / Nuevo-sin tocar), de forma que el lector vea de un vistazo qué es accionable y qué está aparcado.
4. Para cada hilo, usa la entrada fija de cuatro líneas: **Trabajo** (qué se hizo este turno), **Siguiente** (la única acción siguiente y su responsable), **Esperando** (quién o qué lo bloquea, y desde cuándo), **Riesgo** (proximidad de SLA, sentimiento, VIP, o "ninguno").
5. Encabeza la página con una sección destacada: todo lo crítico en las próximas horas, tickets cerca del SLA y llamadas de vuelta prometidas.
6. Al final de la jornada, ofrece registrar el tiempo pendiente con log_time_entry y publicar notas de relevo por ticket (texto plano) para lo no resuelto — publica solo tras confirmación.

## Salvaguardas

- Optimiza para velocidad de lectura: quien recibe debe saber qué coger en menos de un minuto. Agrega; no pegues hilos de tickets.
- Nunca omitas bloqueos ni elementos en riesgo de SLA por acortar el resumen.
- Declara los límites de resultados — no presentes una búsqueda truncada como la cola completa.
- Las notas de ticket van en texto plano; la página de relevo es salida de chat salvo que pidan otra cosa.
- Para traspasar una llamada viva en curso, usa la skill Live Call Transfer Brief — esta skill es para relevos de turno y de fin de jornada.

## Convenciones locales (español)

- Horas en formato de 24 horas siempre ("llamada prometida a las 18:00"); un relevo nocturno con horas ambiguas de 12 horas es un riesgo operativo.
- Fechas en DD/MM; para relevos que cruzan la medianoche, escribe fecha y hora completas ("16/07 02:00").
- Trato entre colegas: tuteo directo y profesional en toda la página de relevo; el usted se reserva para el contenido de cara al cliente.
- Si el desk opera en varias zonas horarias (España peninsular/Canarias, o equipos en LatAm), indica la zona junto a cada hora crítica ("18:00 CET").

## Consolida

Skills de resumen de relevo de turno, cierre de fin de jornada y briefing para el turno siguiente.
