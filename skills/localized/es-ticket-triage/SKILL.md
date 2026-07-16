---
name: Triaje de tickets
description: Clasificar un ticket nuevo o sin asignar, calibrar su gravedad, detectar duplicados y recomendar adónde debe ir antes de entrar en la cola. (Spanish version of Ticket Triage — first-pass classification, severity, duplicates, and routing proposal.)
category: Localized
tools: [search_tickets, list_boards, list_ticket_statuses, list_ticket_priorities, search_clients, update_ticket]
---

# Triaje de tickets

Versión en español de `skills/triage-and-routing/ticket-triage` — kept in sync manually.

Primera lectura de un ticket nuevo o sin asignar: clasificarlo, forzar la gravedad correcta, detectar duplicados y proponer tablero, estado y prioridad — y aplicarlo solo tras confirmación.

## Cuándo usar

- Acaba de llegar un ticket nuevo y necesita una primera pasada antes del despacho.
- "Haz triaje de este ticket" / "clasifica esto" / "¿adónde debería ir?"
- Barrido matinal de tickets sin asignar en el tablero de entrada.
- Un dispatcher quiere una propuesta de tablero, estado y prioridad con un resumen de cola de una línea.

## Pasos

1. Lee el hilo completo con `search_tickets`, incluido el primer mensaje y cada respuesta. No clasifiques ni enrutes a partir del título solo — los títulos suelen estar desactualizados, autogenerados o directamente equivocados.
2. Clasifica el asunto en las categorías de tu desk (p. ej. accesos, hardware, software, red, correo, seguridad). Basa la decisión en el cuerpo de la solicitud, no en palabras clave del asunto.
3. Aplica las reglas de gravedad forzada antes de cualquier juicio propio: todo lo relacionado con seguridad (compromiso, phishing, malware, actividad inesperada de MFA o inicio de sesión) o que afecte a varios usuarios o a una sede completa es prioridad alta por defecto. Solo las propias palabras del solicitante restándole gravedad, confirmadas con el técnico, pueden rebajarla.
4. Comprueba duplicados usando identificadores fuertes, no similitud de redacción: mismo ID de alerta, mismo nombre de dispositivo o activo, misma referencia de monitorización, o mismo contacto + mismo error específico en una ventana reciente. Lanza una búsqueda `search_tickets` separada por identificador; si alguna búsqueda puede haber tocado el límite de resultados, dilo.
5. Busca al cliente con `search_clients` para confirmar que la empresa del ticket coincide con el solicitante.
6. Propón: tablero, estado, prioridad, categoría y un resumen de cola de una línea. Enumera los duplicados sospechosos con sus números de ticket y el porqué de la coincidencia.
7. Solo después de que el revisor confirme la propuesta, aplícala con `update_ticket`. Si cambia algo, aplica su versión, no la tuya.

## Salvaguardas

- Nunca enrutes ni clasifiques por el título solo; lee siempre el hilo primero.
- Nunca modifiques el ticket antes de que la clasificación propuesta esté confirmada. Esta skill recomienda; aplica una persona (o una skill desatendida configurada aparte).
- Seguridad o impacto multiusuario fuerzan prioridad alta — no dejes que un tono tranquilo en el mensaje te convenza de lo contrario.
- Declarar un duplicado exige coincidencia en un identificador fuerte. La redacción parecida es una pista que mencionar, nunca motivo para fusionar o cerrar.
- Si los nombres de tablero/estado/prioridad de este tenant son ambiguos, recupéralos con `list_boards` / `list_ticket_statuses` / `list_ticket_priorities` en lugar de adivinar etiquetas.
- No inventes números de ticket, clientes ni categorías. Si el ticket no puede clasificarse con confianza, dilo y déjalo para un humano.

## Convenciones locales (español)

- Los tenants hispanohablantes suelen mezclar etiquetas en español e inglés en tableros y estados ("En progreso" junto a "Waiting on client"); por eso, recupera siempre las etiquetas reales con las herramientas de listado — no traduzcas nombres de estado por tu cuenta.
- En el resumen de cola de una línea usa fechas DD/MM y formato de 24 horas.
- Ojo con los nombres compuestos y los dos apellidos al casar contactos y duplicados: "José García" y "José García López" pueden ser la misma persona; el nombre nunca basta por sí solo (regla ya vigente), y en español todavía menos.
- Mantén los términos del producto en inglés (board, status, priority) cuando cites campos de herramienta; usa español natural en todo lo demás.

## Consolida

Skills de triaje matinal, asistencia de triaje de despacho, triaje P1, entrada con detección de duplicados y "clasifícame este ticket" genérico.
