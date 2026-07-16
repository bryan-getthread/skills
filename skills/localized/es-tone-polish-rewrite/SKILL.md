---
name: Pulido de tono
description: Reescribir texto en bruto de un técnico en una versión pulida y lista para el cliente — "escríbelo bonito", "mejora esto", "límpialo antes de enviarlo" — conservando cada hecho exactamente igual. (Spanish version of Tone Polish Rewrite — polishes rough technician text without altering facts.)
category: Localized
tools: []
---

# Pulido de tono

Versión en español de `skills/communication/tone-polish-rewrite` — kept in sync manually.

Toma el borrador en bruto de un técnico — seco, brusco o tecleado con prisa — y devuelve una versión pulida con contenido factual idéntico. La petición de redacción más frecuente en un service desk.

## Cuándo usar

- "Escríbelo bonito: <texto en bruto>"
- "Mejora esto" / "que suene profesional"
- "Límpialo antes de que lo envíe" — con un borrador pegado.
- Un mensaje suena duro, cortante o confuso y el técnico quiere una versión más amable.

## Pasos

1. Lee el texto en bruto e identifica cada afirmación factual: qué pasó, qué se hizo, horas, nombres, compromisos. Son inmutables.
2. Reescribe para tono y claridad según la skill Email Baseline Standard (communication/email-baseline-standard): la respuesta primero, lenguaje llano, sin relleno, 1–2 párrafos cortos salvo que el original exija más.
3. Respeta al pie de la letra cualquier restricción de estilo que el miembro indique — p. ej. "sin rayas" significa cero rayas; "más corto" significa más corto; "español de España" significa español de España. Una restricción declarada una vez en la sesión se aplica a todas las reescrituras de esa sesión.
4. Mantén idénticos el sentido y los compromisos del miembro: no suavices un "no" hasta convertirlo en un "quizá", no eleves un "debería" a un "va a", no añadas promesas, disculpas ni pasos que el original no contenía.
5. Devuelve **solo el texto reescrito** — sin preámbulo, sin "Aquí tienes una versión pulida:", sin comentarios, sin opciones salvo que las pidan. La respuesta es el artefacto; el miembro la va a copiar y pegar.

## Salvaguardas

- Nunca añadas, elimines ni alteres hechos. El pulido es solo de tono; si el original es ambiguo, conserva la ambigüedad en lugar de adivinar una interpretación.
- Nunca inventes un nombre para el saludo, un número de ticket ni un detalle que no esté en el texto original.
- Si el original contiene algo que no debe llegar al cliente (credenciales, precios internos, reproches), déjalo fuera de la reescritura y añade una única marca entre corchetes al final: [Eliminado: <qué> — solo interno].
- Conserva el idioma del original salvo que pidan traducción (para pulir textos de no nativos en inglés, véase communication/esl-drafting-assistant).

## Convenciones locales (español)

- Al pulir texto dirigido a clientes, el registro por defecto es **usted**; solo tutea si el original ya tuteaba (respeta el registro del técnico — cambiarlo altera el sentido social del mensaje).
- Corrige la puntuación española al pulir: signos de apertura ¿ ¡, comillas y espaciado — es parte legítima del pulido, no un cambio de contenido.
- Normaliza fechas y horas al formato local (DD/MM/AAAA, 24 horas) solo si el original ya contenía la fecha; nunca añadas una.
- Despedidas naturales si el original traía una: "Un saludo" / "Saludos cordiales". No añadas despedida si el original no la tenía y el workspace aplica firma gestionada.

## Variante desatendida (Flows)

- Tu respuesta completa es el texto reescrito, literal — sin narración, sin etiquetas.
- Si la entrada está vacía o no contiene prosa reescribible, no devuelvas nada.
