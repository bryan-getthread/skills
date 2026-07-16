---
name: Enrutamiento del buzón catchall
description: Identificar el cliente y el contacto correctos de un ticket que cayó en un buzón catchall o sin empresa — incluidos correos reenviados y alertas de proveedores — y reasignarlo o devolver los candidatos. (Spanish version of Catchall Routing — evidence-ladder routing of no-company tickets.)
category: Localized
tools: [search_tickets, search_clients, search_contacts, assign_contact, update_ticket, add_ticket_note, create_ticket]
---

# Enrutamiento del buzón catchall

Versión en español de `skills/triage-and-routing/catchall-routing` — kept in sync manually.

Enruta un ticket que llegó sin empresa (o sobre un contacto catchall) hacia el cliente y el contacto reales, usando una escalera de evidencia explícita — o lo deja tal cual cuando la evidencia no alcanza.

## Cuándo usar

- Un ticket no tiene empresa asignada o está sobre un contacto catchall/sin-empresa.
- Un correo entró reenviado ("FW:" / "RV:") por un técnico o un cliente y el ticket quedó atribuido al reenviante, no al remitente original.
- Una alerta de proveedor o de monitorización cayó en el catchall y hay que extraer su cliente del cuerpo de la alerta.
- Barrido periódico del tablero catchall/de entrada para remapear tickets mal atribuidos.

## Pasos

1. Lee el ticket con `search_tickets`: título, descripción, primer mensaje y las cabeceras completas o el texto citado si existen.
2. Prefiltro de spam: si el mensaje es claramente marketing no solicitado o correo basura automatizado, sin contenido que identifique a un cliente y sin señales de que un humano lo reenviara por algún motivo, detente y márcalo como spam para revisión en lugar de enrutarlo.
3. Extrae pistas de identidad en este orden de fuerza (la escalera de evidencia):
   a. Dominio del correo del remitente verdadero (la más fuerte),
   b. Un nombre de empresa declarado explícitamente en el cuerpo o en los campos de la alerta,
   c. El nombre de una persona a secas (la más débil — nunca suficiente por sí sola).
4. Si el asunto empieza por "FW:" o "RV:", o el cuerpo contiene un mensaje original citado, parsea la línea "From:" / "De:" original y usa a ese remitente — no al reenviante — como fuente de identidad.
5. Si el ticket es una alerta de proveedor/monitorización, extrae los campos estructurados (nombre de sede, organización, dispositivo, tenant) del cuerpo de la alerta y úsalos como pista de empresa.
6. Resuelve la empresa con `search_clients` sobre el dominio o el nombre extraído; después localiza el contacto con `search_contacts` acotado a esa empresa. Si el remitente no tiene ficha de contacto, propón la coincidencia más cercana o señala que hay que crear una — no lo cuelgues de un parecido.
7. Si en este tenant hay que limpiar primero la asignación errónea antes de aplicar la correcta, desasigna primero (devuelve el ticket a sin-empresa/catchall) y aplica después la empresa y el contacto correctos.
8. Aplica la coincidencia con `update_ticket` y `assign_contact` solo cuando encaje exactamente una empresa candidata con fuerza de dominio o de nombre explícito. En caso contrario, presenta los candidatos y pregunta.
9. Si la sincronización PSA del tenant rechaza cambios de empresa en un ticket existente, usa la vía de cerrar-y-recrear: `create_ticket` bajo la empresa correcta con el texto original y una nota en texto plano que cruce ambos números de ticket, y cierra el original — solo con confirmación en uso atendido.
10. Publica una nota interna en texto plano con `add_ticket_note` indicando qué se casó, qué peldaño de evidencia se usó y qué cambió.

## Salvaguardas

- Nunca asignes por similitud de nombre a secas. Un nombre y apellido coincidentes sin dominio ni confirmación de empresa explícita es confianza baja → no hagas cambios.
- Ambigüedad (dos o más empresas plausibles) → ningún cambio; lista los candidatos.
- Un ticket sincronizado con un PSA debe tener siempre empresa; nunca dejes sin empresa un ticket ligado a un PSA — si no hay coincidencia posible, enrútalo al cliente interno/catchall designado según la política del desk y márcalo.
- Cerrar-y-recrear roza lo destructivo: en uso atendido, confirma antes de hacerlo; conserva el texto original y cruza ambos números en texto plano.
- Las notas destinadas al PSA van en texto plano — sin markdown ni emojis.
- No inventes empresas, contactos ni números de ticket. Si las búsquedas pueden haber tocado límites de resultados, dilo.

## Convenciones locales (español)

- Los clientes de correo en español marcan los reenvíos como "RV:" o "RV: FW:" y las respuestas como "RE:"; trata "RV:" con la misma regla que "FW:" y busca la línea "De:" además de "From:" en el texto citado.
- Los dos apellidos multiplican los falsos parecidos de nombre: refuerza aún más la regla de que un nombre de persona nunca basta.
- Cuidado con los dominios genéricos regionales (gmail.com, hotmail.es, outlook.es): un dominio personal no es evidencia de empresa — trátalo como peldaño c (débil), no como peldaño a.
- La nota interna puede ir en español, pero conserva el prefijo marcador en inglés `CATCHALL ROUTING:` — otras ejecuciones lo usan para detectar que el ticket ya fue procesado.

## Variante desatendida (Flows)

- Actúa solo con fuerza de coincidencia de dominio o de empresa explícita; ante cualquier cosa más débil, no hagas cambios y publica una única nota interna en texto plano: `CATCHALL ROUTING: sin coincidencia fiable. Evidencia encontrada: <clues>. Se deja sin asignar para revisión humana.`
- Nunca uses la vía de cerrar-y-recrear en modo desatendido.
- Rama de spam: no cierres en desatendido; marca solo mediante nota.
- Un remapeo por ticket y por ejecución; si el ticket ya lleva una nota de enrutamiento de esta skill, detente.
- Tu respuesta completa es la nota interna — sin narración, sin preguntas.

## Consolida

Skills de mapeo catchall-a-contacto, reparación de catchall, remapeos con desasignación previa, prefiltro de spam en la entrada, parseo del remitente original en cabeceras FW:, extracción de campos de alertas de proveedor, identificación de empresa/contacto, enriquecimiento sin-empresa, remapeos por cerrar-y-recrear y reenrutamiento al cliente correcto (28 variantes minadas).
