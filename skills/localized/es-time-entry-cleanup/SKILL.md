---
name: Limpieza de registros de tiempo
description: Convertir notas de tiempo en bruto en registros de tiempo limpios y estandarizados — versión de cara al cliente y versión interna — registrando únicamente el trabajo que el ingeniero declaró explícitamente haber realizado. (Spanish version of Time Entry Cleanup — zero-assumption time entry normalization.)
category: Localized
tools: [search_tickets, log_time_entry, add_ticket_note]
---

# Limpieza de registros de tiempo

Versión en español de `skills/documentation/time-entry-cleanup` — kept in sync manually.

Normaliza notas de tiempo desordenadas al formato estándar de registro del desk, separando la redacción de cara al cliente del detalle interno, sin inventar ni engordar jamás el trabajo realizado.

## Cuándo usar

- Un técnico pega taquigrafía en bruto ("rst pw, probado ok, avisado usr") y quiere un registro de tiempo como es debido.
- "Límpiame las notas de tiempo de este ticket."
- Fin de jornada: varios registros en bruto necesitan estandarizarse antes de la revisión de facturación.
- El cierre de una llamada tiene que convertirse en un registro facturable con su duración.

## Pasos

1. Lee las notas en bruto; recupera contexto del hilo del ticket con `search_tickets` solo para resolver referencias (qué ticket, qué <user>, qué <device>) — nunca para añadir partidas de trabajo.
2. Reescribe cada registro en pasado claro con el orden fijo de campos:
   1. **Incidencia** — qué se reportó.
   2. **Trabajo realizado** — lo que el ingeniero declaró explícitamente haber hecho.
   3. **Resultado** — el desenlace tal y como fue declarado.
   4. **Próximos pasos** — solo si el ingeniero indicó alguno.
3. Aplica la **regla de cero suposiciones**: incluye únicamente lo que el ingeniero dijo explícitamente que hizo. Nunca conviertas una recomendación, una intención o un "habría que" en una acción completada ("recomendó reiniciar" NO puede convertirse en "reinició"). Si las notas son ambiguas, pregunta o marca `[no claro — confirmar]`.
4. Aplica las sustituciones de palabras prohibidas según el estilo de la casa — juego típico: "arreglado" → "resuelto"; "el problema del tío este" → "incidencia reportada por <user>"; "la lió" → "configuró incorrectamente"; "debería ir bien" → "verificado y funcionando" **solo si la verificación fue declarada**, en caso contrario "pendiente de confirmación del usuario". Respeta la lista de prohibidas del propio desk cuando exista.
5. Produce dos versiones cuando las pidan:
   - **De cara al cliente** — lenguaje profesional llano, sin taquigrafía interna, sin culpas, sin quejas sobre proveedores, texto plano (seguro para el PSA).
   - **Interna** — todo el detalle técnico, hipótesis y avisos de seguimiento.
6. Devuelve los registros limpios con sus duraciones. Escribe con `log_time_entry` / `add_ticket_note` solo después de que el técnico confirme el texto y el tiempo imputado.

## Salvaguardas

- **Cero suposiciones es absoluto.** Nada de trabajo inferido, verificación inferida ni duraciones inferidas. Si no se indicó una duración, pregunta — no estimes en silencio.
- Conserva el sentido de lo que escribió el técnico; limpia solo la redacción. El detalle facturable debe seguir siendo fiel a lo que describen las notas.
- Nunca traslades comentarios solo internos (culpas, fricción con el cliente, sospechas de seguridad) a la versión de cara al cliente.
- Confirma antes de registrar: los registros afectan a la facturación. Ante la duda, entrega solo el texto y deja que el técnico lo envíe.

## Convenciones locales (español)

- Duraciones en el formato del desk: "1 h 30 min" o decimal ("1,5 h" — coma decimal, no punto) según lo que consuma el PSA; pregunta si no está claro.
- Versión de cara al cliente en registro de usted e impersonal donde encaje ("Se restableció la contraseña y se verificó el acceso"); la primera persona ("Restablecí...") solo si el desk la pide.
- Texto plano para el PSA: conserva tildes y ñ (son ortografía correcta), pero nada de markdown, emojis, comillas tipográficas ni rayas.
- Fechas de los registros en DD/MM/AAAA y horas en formato de 24 horas.

## Consolida

Skills de resumen de registro de tiempo, reformateo de notas de tiempo en bruto, cierre de llamada y estilo de notas con palabras prohibidas.
