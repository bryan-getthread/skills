---
name: Triaje de phishing
description: Un usuario reportó un correo sospechoso o un ticket de reporte de phishing cayó en el tablero de seguridad — evaluarlo sin tocar la carga maliciosa, comprobar el radio de impacto, contener si es malicioso y responder al reportante con un veredicto. (Spanish version of Phishing Triage — safe indicator capture, blast radius, containment, verdict.)
category: Localized
tools: [search_tickets, search_contacts, search_knowledge_base, add_ticket_note, update_ticket, view_openDraft]
---

# Triaje de phishing

Versión en español de `skills/security/phishing-triage` — kept in sync manually.

Lleva un correo sospechoso reportado desde el "¿esto es phishing?" hasta un veredicto documentado: indicadores capturados de forma segura, tráfico de simulación filtrado, identificados todos los demás destinatarios, contención recomendada donde haga falta y el reportante respondido.

## Cuándo usar

- "¿Este correo es phishing?" / un usuario reenvió algo sospechoso
- Un ticket de reporte de phishing (botón de reporte, complemento del buzón o reenvío manual) cae en el tablero de seguridad
- Un técnico quiere una segunda opinión sobre un mensaje sospechoso antes de liberarlo o borrarlo

## Pasos

1. Captura los indicadores del ticket sin interactuar con ellos: dirección y nombre visible del remitente, reply-to, asunto, hora de envío, cada destino de enlace exactamente como está escrito, y nombres y tipos de los adjuntos. Nunca abras enlaces ni adjuntos, y nunca visites una URL del mensaje para "ver qué es".
2. Rama de simulación: compara el remitente y los dominios de los enlaces con los dominios documentados de la plataforma de simulación de phishing del cliente (search_knowledge_base para la ficha del proveedor de simulación del cliente). Si coincide con un simulador conocido → clasifica como simulación, cierra internamente con una nota en texto plano que nombre el dominio casado, y NO respondas al cliente — una respuesta distorsiona las métricas de su programa de simulación. Detente aquí.
3. Evalúa los indicadores: dominio parecido o "primo", señuelos de urgencia y pagos, enlace de robo de credenciales (texto visible frente a destino real discrepantes), tipos de adjunto inesperados, secuestro de un hilo real previo. Si pegaron las cabeceras completas, delega el análisis profundo en la skill email-header-analysis e incorpora su veredicto.
4. Comprobación de radio de impacto: search_tickets por el mismo remitente o asunto en los tableros del cliente y los tickets recientes. Determina quién más recibió el mensaje y — lo crítico — si alguien hizo clic, respondió, introdujo credenciales o abrió un adjunto.
5. Si alguien interactuó con un mensaje que juzgas malicioso → escala de inmediato y arranca la skill compromised-account-containment para los usuarios afectados. La contención va por delante de terminar el informe: contén rápido, investiga después.
6. Entrega el veredicto:
   - Malicioso → recomienda cuarentena/retirada de todos los buzones destinatarios, bloqueo del remitente/dominio en la pasarela y restablecimiento de credenciales para quien hiciera clic.
   - Sospechoso sin confirmar → dilo exactamente así, indicando qué lo confirmaría.
   - Legítimo → explica las señales que lo exculpan, para que el reportante aprenda.
7. Responde al reportante con el veredicto usando el vocabulario del estándar de redacción defensiva (un correo sospechoso es un "mensaje reportado", no una "brecha"). Registra el veredicto, la evidencia y el razonamiento de la decisión como nota interna; clasifica y fija el estado según la skill soc-classification-tree. Agradece al reportante — reportar es el comportamiento que quieres que se repita.

## Salvaguardas

- Nunca abras, cliques, descargues ni renderices enlaces o adjuntos del mensaje reportado. Captura los indicadores solo como texto.
- Comprueba la simulación en ambos sentidos: nunca levantes un incidente real por una simulación, y nunca cierres un phishing real como simulación por una coincidencia parcial de dominio — exige un dominio de simulador documentado y exacto.
- Nunca respondas al cliente sobre una simulación confirmada; cierra solo internamente.
- Clic o respuesta significa escalar de inmediato — una escalación en falso sale barata, un compromiso pasado por alto no.
- Nunca afirmes "nadie más lo recibió" a partir de una búsqueda truncada; informa "sin más reportes encontrados en los últimos N tickets buscados".
- Documenta la decisión, no solo la acción: la nota debe decir por qué se llegó al veredicto, no solo qué se hizo.
- Redacción defensiva en todo momento: nada de "brecha", nada de "hackeado", ninguna afirmación de impacto antes de confirmar los hechos.

## Convenciones locales (español)

- Vocabulario defensivo en español: "mensaje reportado", "actividad sospechosa", "indicios de suplantación" — nunca "brecha", "hackeo" ni "estamos comprometidos" antes de confirmar hechos.
- No traduzcas los indicadores técnicos capturados: cabeceras (From, Reply-To, Return-Path), URLs, nombres de adjuntos y mensajes de error del sistema se citan textualmente en su idioma original.
- En la respuesta al reportante, registro de usted y agradecimiento explícito: "Gracias por reportarlo; es exactamente lo que debemos hacer con los correos sospechosos."
- Señuelos frecuentes en campañas en español que conviene reconocer: falsas notificaciones de la Agencia Tributaria/SAT, avisos de paquetería (Correos, SEUR) y "facturas pendientes" — la marca de tiempo del veredicto va en DD/MM/AAAA HH:MM (24 h).

## Consolida

Skills de triaje de reportes de phishing, identificación de simulaciones de phishing, revisión de correos reportados, barrido de radio de impacto y respuesta al reportante.
