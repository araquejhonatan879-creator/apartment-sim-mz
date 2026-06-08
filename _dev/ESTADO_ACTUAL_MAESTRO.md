# APARTMENT SIM — ESTADO ACTUAL MAESTRO
### Versión 1.0 — RPG Maker MZ
### Última actualización: 07/06/2026
### Actualizado por: Arquitecto

---

## INSTRUCCIÓN DE USO

Este documento es la primera lectura obligatoria de cualquier dev al inicio
de cada sesión. Refleja el estado real del proyecto en este momento.

**Quién lo actualiza:** Cualquier dev al cerrar su sesión — agrega su resumen
al final de la sección correspondiente. El Arquitecto lo consolida
periódicamente y resuelve contradicciones.

**Qué contiene:** Estado de cada fase, qué está listo para usarse,
qué está bloqueado y por qué, decisiones de diseño tomadas fuera de los GDDs.

---

## ESTADO GENERAL DEL PROYECTO

| Fase | Estado | Responsable | Bloqueado por |
|------|--------|-------------|---------------|
| Documentación | ✅ COMPLETA | Arquitecto | — |
| Dev Core — sistemas base | ⏳ PENDIENTE | Dev Core | Esperando primera sesión |
| Dev Arte — assets iniciales | ⏳ PENDIENTE | Dev Arte | Esperando primera sesión |
| Dev Contenido — implementación | ⏳ PENDIENTE | Dev Contenido | Dev Core + Dev Arte |
| Dev Debugger | 🔒 EN ESPERA | Dev Debugger | Solo se activa ante bug concreto |

---

## DOCUMENTACIÓN — COMPLETA ✅

Todos los documentos base están aprobados y listos.

| Documento | Estado | Ubicación |
|-----------|--------|-----------|
| GDD_CORE.md | ✅ Aprobado | `docs/GDD_CORE.md` |
| GDD_PERSONAJES.md | ✅ Aprobado | `docs/GDD_PERSONAJES.md` |
| GDD_SISTEMAS.md | ✅ Aprobado | `docs/GDD_SISTEMAS.md` |
| VARIABLES_GLOBALES.md | ✅ Aprobado | `docs/VARIABLES_GLOBALES.md` |
| ESTANDARES.md | ✅ Aprobado | `docs/ESTANDARES.md` |
| PROMPT_DEV_CORE.md | ✅ Aprobado | `_dev/PROMPT_DEV_CORE.md` |
| PROMPT_DEV_CONTENIDO.md | ✅ Aprobado | `_dev/PROMPT_DEV_CONTENIDO.md` |
| PROMPT_DEV_ARTE.md | ✅ Aprobado | `_dev/PROMPT_DEV_ARTE.md` |
| PROMPT_DEV_DEBUGGER.md | ✅ Aprobado | `_dev/PROMPT_DEV_DEBUGGER.md` |
| ESTADO_ACTUAL_MAESTRO.md | ✅ Activo | `_dev/ESTADO_ACTUAL_MAESTRO.md` |

---

## DEV CORE — ESTADO ACTUAL

**Sesiones completadas:** 0
**Último commit:** —

### Funciones disponibles para Dev Contenido
*Ninguna aún — Dev Core no ha comenzado.*

### Archivos implementados
| Archivo | Estado | Notas |
|---------|--------|-------|
| `js/custom/core/vars.js` | ⏳ PENDIENTE | — |
| `js/custom/core/stats.js` | ⏳ PENDIENTE | — |
| `js/custom/core/time.js` | ⏳ PENDIENTE | — |
| `js/custom/core/economy.js` | ⏳ PENDIENTE | — |
| `js/custom/core/mood.js` | ⏳ PENDIENTE | — |
| `js/custom/core/secrets.js` | ⏳ PENDIENTE | — |
| `js/custom/core/jealousy.js` | ⏳ PENDIENTE | — |
| `js/custom/core/memory.js` | ⏳ PENDIENTE | — |
| `js/custom/characters/celine.js` | ⏳ PENDIENTE | — |
| `js/custom/characters/roxy.js` | ⏳ PENDIENTE | — |
| `js/custom/characters/luna.js` | ⏳ PENDIENTE | — |

### Problemas conocidos
*Ninguno aún.*

### Próxima sesión
Comenzar con `vars.js` — objeto AptSim, `fn_vars_inicializar()` y hooks de save.
Ver PROMPT_DEV_CORE.md sección "Orden de implementación".

---

## DEV ARTE — ESTADO ACTUAL

**Sesiones completadas:** 0
**Último commit:** —

### Seeds documentadas
| Personaje | Seed | Estado |
|-----------|------|--------|
| Celine | — | ⏳ PENDIENTE — definir en primera sesión |
| Roxy | — | ⏳ PENDIENTE — definir en primera sesión |
| Luna | — | ⏳ PENDIENTE — definir en primera sesión |

### Sprites entregados
| Personaje | Outfit | Expresiones listas | Total |
|-----------|--------|--------------------|-------|
| Celine | outfit1 | ninguna | 0/7 |
| Celine | outfit2 | ninguna | 0/7 |
| Celine | outfit3 | ninguna | 0/7 |
| Celine | outfit4 | ninguna | 0/7 |
| Roxy | outfit1 | ninguna | 0/7 |
| Roxy | outfit2 | ninguna | 0/7 |
| Roxy | outfit3 | ninguna | 0/7 |
| Roxy | outfit4 | ninguna | 0/7 |
| Luna | outfit1 | ninguna | 0/7 |
| Luna | outfit2 | ninguna | 0/7 |
| Luna | outfit3 | ninguna | 0/7 |
| Luna | outfit4 | ninguna | 0/7 |

### Fondos entregados
| Fondo | Estado |
|-------|--------|
| sala_manana | ⏳ PENDIENTE |
| sala_tarde | ⏳ PENDIENTE |
| sala_noche | ⏳ PENDIENTE |
| sala_madrugada | ⏳ PENDIENTE |
| cocina_manana | ⏳ PENDIENTE |
| cocina_tarde | ⏳ PENDIENTE |
| cocina_noche | ⏳ PENDIENTE |
| cocina_madrugada | ⏳ PENDIENTE |
| balcon_tarde | ⏳ PENDIENTE |
| balcon_noche | ⏳ PENDIENTE |
| balcon_madrugada | ⏳ PENDIENTE |
| bano_manana | ⏳ PENDIENTE |
| bano_noche | ⏳ PENDIENTE |
| cuarto_celine_tarde | ⏳ PENDIENTE |
| cuarto_celine_noche | ⏳ PENDIENTE |
| cuarto_celine_madrugada | ⏳ PENDIENTE |
| cuarto_roxy_tarde | ⏳ PENDIENTE |
| cuarto_roxy_noche | ⏳ PENDIENTE |
| cuarto_roxy_madrugada | ⏳ PENDIENTE |
| cuarto_luna_tarde | ⏳ PENDIENTE |
| cuarto_luna_noche | ⏳ PENDIENTE |
| cuarto_luna_madrugada | ⏳ PENDIENTE |

### Galería entregada
*Ninguna aún. Se genera cuando Dev Contenido confirma que el acto está implementado.*

### Bloqueadores
*Ninguno aún.*

### Próxima sesión
Definir seed de Celine y generar `celine_outfit1_*` completo (7 expresiones).
Ver PROMPT_DEV_ARTE.md sección "Flujo de trabajo por personaje".

---

## DEV CONTENIDO — ESTADO ACTUAL

**Sesiones completadas:** 0
**Último commit:** —

**BLOQUEADO** — Esperando:
- Dev Core: `vars.js`, `stats.js`, `time.js` mínimo
- Dev Arte: `outfit1` completo de los 3 personajes + fondos de sala y cocina

### Eventos implementados
| Evento | Estado | Notas |
|--------|--------|-------|
| Pantalla de título | ⏳ PENDIENTE | — |
| Día 1 — secuencia lineal | ⏳ PENDIENTE | — |
| Mapa apartamento — navegación | ⏳ PENDIENTE | — |
| HUD básico | ⏳ PENDIENTE | — |
| Celine Acto 1 "El apagón" | ⏳ PENDIENTE | — |
| Celine Acto 2 "Los apuntes" | ⏳ PENDIENTE | — |
| Celine Acto 3 "La fiesta de Roxy" | ⏳ PENDIENTE | — |
| Celine Acto 4 "La confesión a medias" | ⏳ PENDIENTE | — |
| Celine Acto 5 "Sin barreras" | ⏳ PENDIENTE | — |
| Roxy Acto 1 "El sketchbook" | ⏳ PENDIENTE | — |
| Roxy Acto 2 "La apuesta" | ⏳ PENDIENTE | — |
| Roxy Acto 3 "La pelea" | ⏳ PENDIENTE | — |
| Roxy Acto 4 "2am" | ⏳ PENDIENTE | — |
| Roxy Acto 5 "Sin actuar" | ⏳ PENDIENTE | — |
| Luna Acto 1 "El libro" | ⏳ PENDIENTE | — |
| Luna Acto 2 "La madrugada" | ⏳ PENDIENTE | — |
| Luna Acto 3 "El cuaderno" | ⏳ PENDIENTE | — |
| Luna Acto 4 "Una frase" | ⏳ PENDIENTE | — |
| Luna Acto 5 "En voz alta" | ⏳ PENDIENTE | — |
| Celine Quest 1 "Los apuntes" | ⏳ PENDIENTE | — |
| Celine Quest 2 "La defensa" | ⏳ PENDIENTE | — |
| Celine Quest 3 "El diario" | ⏳ PENDIENTE | — |
| Roxy Quest 1 "El sketchbook" | ⏳ PENDIENTE | — |
| Roxy Quest 2 "La exposición" | ⏳ PENDIENTE | — |
| Roxy Quest 3 "La noche que llora" | ⏳ PENDIENTE | — |
| Luna Quest 1 "El libro" | ⏳ PENDIENTE | — |
| Luna Quest 2 "Tres días de silencio" | ⏳ PENDIENTE | — |
| Luna Quest 3 "El mismo libro" | ⏳ PENDIENTE | — |
| Interacciones libres nivel 1 | ⏳ PENDIENTE | — |
| Interacciones libres nivel 2 | ⏳ PENDIENTE | — |
| Interacciones libres nivel 3 | ⏳ PENDIENTE | — |
| Interacciones libres nivel 4 | ⏳ PENDIENTE | — |
| Interacciones libres nivel 5 | ⏳ PENDIENTE | — |
| Strip 21 | ⏳ PENDIENTE | — |
| Verdad o Reto | ⏳ PENDIENTE | — |
| Sistema mensajes de texto | ⏳ PENDIENTE | — |
| Galería | ⏳ PENDIENTE | — |
| Sistema voyeur | ⏳ PENDIENTE | — |
| Eventos espontáneos | ⏳ PENDIENTE | — |
| Eventos de temporada | ⏳ PENDIENTE | — |

### Problemas conocidos
*Ninguno aún.*

---

## DEV DEBUGGER — HISTORIAL DE INTERVENCIONES

*Ninguna intervención hasta el momento.*

---

## DECISIONES DE DISEÑO TOMADAS FUERA DE LOS GDDs

*Ninguna aún. Esta sección se usa para registrar decisiones que el Arquitecto
tomó durante el desarrollo y que no están en los documentos base.*

Formato cuando se agregue:
```
### [fecha] — [título de la decisión]
**Contexto:** [por qué surgió la necesidad]
**Decisión:** [qué se decidió]
**Impacto:** [qué archivos o sistemas afecta]
**Aprobado por:** Arquitecto
```

---

## VARIABLES AGREGADAS DURANTE EL DESARROLLO

*Ninguna aún. Si durante el desarrollo se aprueba una variable nueva
que no estaba en VARIABLES_GLOBALES.md v1.0, se documenta aquí
hasta que se actualice el documento base.*

Formato cuando se agregue:
```
| Variable | Tipo | Valor inicial | Motivo | Sesión aprobada |
```

---

## HISTORIAL DE SESIONES

| Sesión | Fecha | Dev | Resumen |
|--------|-------|-----|---------|
| 001 | 07/06/2026 | Arquitecto | Documentación completa — todos los GDDs y prompts aprobados |

---

## PRÓXIMAS SESIONES — ORDEN RECOMENDADO

```
SESIÓN 002 → Dev Core
  Objetivo: vars.js + stats.js funcionales y testeados
  Entrega: fn_vars_inicializar() + fn_stats_modificar() + hooks de save
  Duración estimada: 1 sesión

SESIÓN 003 → Dev Core
  Objetivo: time.js + economy.js + mood.js
  Entrega: fn_tiempo_avanzar_franja() + fn_economia_trabajar() + fn_mood_recalcular()
  Duración estimada: 1 sesión

SESIÓN 004 → Dev Core (paralelo con Arte)
  Objetivo: secrets.js + jealousy.js + memory.js + characters/*.js
  Entrega: sistemas completos — Dev Core termina su fase
  Duración estimada: 1-2 sesiones

SESIÓN 002-A → Dev Arte (paralelo con Core desde inicio)
  Objetivo: seed de Celine + celine_outfit1_* completo (7 sprites)
  Entrega: 7 archivos PNG en img/characters/celine/
  Duración estimada: 1 sesión

SESIÓN 003-A → Dev Arte
  Objetivo: roxy_outfit1_* + luna_outfit1_* completos
  Entrega: 14 archivos PNG
  Duración estimada: 1-2 sesiones

SESIÓN 004-A → Dev Arte
  Objetivo: fondos mínimos para Dev Contenido (sala + cocina, 8 fondos)
  Entrega: 8 PNG en img/backgrounds/
  Duración estimada: 1 sesión

SESIÓN 005 → Dev Contenido
  PREREQUISITO: Dev Core completo + outfit1 de 3 personajes + fondos sala/cocina
  Objetivo: título + día 1 + navegación mapa + HUD básico
  Duración estimada: 1-2 sesiones

SESIÓN 006+ → Dev Contenido iterando
  Actos, quests, interacciones libres, mini-juegos
  En orden de PROMPT_DEV_CONTENIDO.md sección "Orden de implementación"
