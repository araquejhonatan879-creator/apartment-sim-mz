# APARTMENT SIM — VARIABLES GLOBALES
### Versión: 1.0 — RPG Maker MZ | Generado: 07/06/2026
### Engine: RPG Maker MZ 1.8.0 | JavaScript ES2020
### Fuentes obligatorias antes de implementar:
### - https://rpgmakermz-api.rmmzgame.com/ (API oficial MZ)
### - https://www.rpgmakerweb.com/blog/category/rpg-maker-mz (blog oficial)
### - ESTANDARES.md (convenciones del proyecto)

---

## REGLAS FUNDAMENTALES — VARIABLES EN RPG MAKER MZ
*(Leer ANTES de declarar o modificar cualquier variable)*

### Cómo funciona el sistema de saves en MZ

RPG Maker MZ guarda el estado del juego serializando `$gameVariables`,
`$gameSwitches` y los objetos de game que el engine controla.

**Las variables de JS custom NO se guardan automáticamente.**
Para que persistan en el save hay exactamente dos opciones:

| Método | Cuándo usar | Cómo |
|--------|-------------|------|
| `$gameVariables.setValue(id, valor)` | Variables numéricas simples que MZ necesita conocer | Se guardan solos con el save del engine |
| Objeto `AptSim` serializado en save | Toda la lógica custom JS compleja | Hook en `DataManager.makeSaveContents` y `extractSaveContents` |

**Decisión de arquitectura para este proyecto:**
Todas las variables de gameplay del proyecto viven en un objeto global `AptSim`
declarado en `js/custom/core/vars.js`. El Dev Core implementa los hooks de
guardado en ese mismo archivo. **Ningún otro dev toca los hooks de save.**

### Regla de objetos y arrays
En JavaScript, asignar un objeto a una variable guarda la *referencia*, no el valor.
Esto puede causar bugs sutiles en el sistema de saves.

**Patrón obligatorio para arrays:**
```javascript
// MAL — muta el array original
AptSim.mem_decisiones.push("nueva");

// BIEN — reasigna siempre
AptSim.mem_decisiones = [...AptSim.mem_decisiones, "nueva"];
```

**Patrón obligatorio para clonar estado antes de guardar:**
```javascript
// En el hook de save — siempre guardar copia profunda
contents.aptSim = JSON.parse(JSON.stringify(AptSim));
```

### Variables de RPG Maker MZ que NO debemos pisar
Las siguientes variables del engine están reservadas. **Nunca asignarlas:**
`$gameVariables`, `$gameSwitches`, `$gameActors`, `$gameMap`,
`$gamePlayer`, `$gameSystem`, `$gameScreen`, `$gameTroop`,
`$dataActors`, `$dataClasses`, `$dataSkills`, `$dataItems`.

---

## ESTRUCTURA DEL OBJETO GLOBAL

Todo el estado del juego vive en un único objeto:

```javascript
// js/custom/core/vars.js
var AptSim = {};
```

Al iniciar nueva partida, `fn_vars_inicializar()` lo puebla con los valores
de la tabla de abajo. Al cargar save, se restaura desde el hook de carga.

---

## 1. STATS POR PERSONAJE

**Archivo responsable:** `js/custom/core/stats.js`
**Archivos que pueden modificar:** solo a través de `fn_stats_modificar(personaje, stat, delta)`
**Regla:** Ningún evento del editor de MZ modifica estos valores directamente.
Siempre mediante llamada a la función de stats desde un Script Call.

| Variable | Tipo | Valor inicial | Rango | Descripción |
|----------|------|--------------|-------|-------------|
| `AptSim.cel_afecto` | `number` (int) | `15` | 0–100 | Afecto de Celine hacia el protagonista |
| `AptSim.cel_confianza` | `number` (int) | `10` | 0–100 | Confianza de Celine en el protagonista |
| `AptSim.cel_corrupcion` | `number` (int) | `5` | 0–100 | Apertura de Celine a escalar la relación |
| `AptSim.rox_afecto` | `number` (int) | `25` | 0–100 | Afecto de Roxy hacia el protagonista |
| `AptSim.rox_confianza` | `number` (int) | `15` | 0–100 | Confianza de Roxy en el protagonista |
| `AptSim.rox_corrupcion` | `number` (int) | `15` | 0–100 | Apertura de Roxy a escalar la relación |
| `AptSim.lun_afecto` | `number` (int) | `20` | 0–100 | Afecto de Luna hacia el protagonista |
| `AptSim.lun_confianza` | `number` (int) | `10` | 0–100 | Confianza de Luna en el protagonista |
| `AptSim.lun_corrupcion` | `number` (int) | `10` | 0–100 | Apertura de Luna a escalar la relación |

**Nota sobre Corrupción:**
Antes de aplicar cualquier delta positivo de Corrupción, la función `fn_stats_modificar`
debe verificar los requisitos mínimos de Afecto y Confianza definidos en GDD_SISTEMAS.md S01.
Si no se cumplen, el delta de Corrupción se descarta silenciosamente — sin mensaje al jugador.

---

## 2. STATS DEL PROTAGONISTA

**Archivo responsable:** `js/custom/core/stats.js`
(Energía: también `js/custom/core/time.js` | Dinero: también `js/custom/core/economy.js`)

| Variable | Tipo | Valor inicial | Rango | Quién modifica | Descripción |
|----------|------|--------------|-------|----------------|-------------|
| `AptSim.prot_nombre` | `string` | `"Kai"` | — | `vars.js` (una vez al inicio) | Nombre del protagonista elegido por el jugador |
| `AptSim.prot_encanto` | `number` (int) | `10` | 0–100 | `stats.js` | Desbloquea opciones de diálogo adicionales |
| `AptSim.prot_energia_max` | `number` (int) | `6` | 6–10 | `stats.js` | Acciones máximas disponibles por día |
| `AptSim.prot_energia_actual` | `number` (int) | `6` | 0–`prot_energia_max` | `time.js` | Acciones restantes en el día actual |
| `AptSim.prot_dinero` | `number` (int) | `500` | 0–sin límite | `economy.js` | Dinero disponible del protagonista |

**Nota sobre prot_nombre:**
Se inicializa con `"Kai"`. Al comenzar nueva partida, el Dev Contenido implementa
una pantalla de input en el editor de MZ (Show Name Input Processing o plugin equivalente).
Una vez asignado, `AptSim.prot_nombre` no vuelve a cambiar. Solo se escribe una vez.

**Nota sobre prot_energia_actual:**
Se restaura a `prot_energia_max` al avanzar de madrugada a mañana (nuevo día).
`time.js` es el único archivo que llama a esa restauración.

---

## 3. PERSONALIDAD DEL JUGADOR

**Archivo responsable:** `js/custom/core/stats.js`
**Descripción:** Acumulativo — no tiene techo. Lo que importa es cuál valor es mayor.
Se recalcula el arquetipo dominante cada 7 días de juego.

| Variable | Tipo | Valor inicial | Quién modifica | Descripción |
|----------|------|--------------|----------------|-------------|
| `AptSim.prot_romantico` | `number` (int) | `0` | `stats.js` | Puntos acumulados del arquetipo romántico |
| `AptSim.prot_atrevido` | `number` (int) | `0` | `stats.js` | Puntos acumulados del arquetipo atrevido |
| `AptSim.prot_respetuoso` | `number` (int) | `0` | `stats.js` | Puntos acumulados del arquetipo respetuoso |
| `AptSim.prot_arquetipo_dominante` | `string` | `""` | `stats.js` | `"romantico"` / `"atrevido"` / `"respetuoso"` / `""` si empate |

**Valores válidos para `prot_arquetipo_dominante`:**
`"romantico"` / `"atrevido"` / `"respetuoso"` / `""` (cadena vacía = sin dominante claro)

---

## 4. SISTEMA DE TIEMPO

**Archivo responsable:** `js/custom/core/time.js`
**Quién puede modificar:** solo `time.js` mediante `fn_tiempo_avanzar_franja()`

| Variable | Tipo | Valor inicial | Quién modifica | Descripción |
|----------|------|--------------|----------------|-------------|
| `AptSim.time_dia` | `number` (int) | `1` | `time.js` | Día actual del juego (empieza en 1 = lunes) |
| `AptSim.time_franja` | `string` | `"manana"` | `time.js` | Franja activa del día |
| `AptSim.time_dia_semana` | `string` | `"lunes"` | `time.js` | Día de la semana en texto, sin tilde |
| `AptSim.time_es_finde` | `boolean` | `false` | `time.js` | `true` si `time_dia_semana` es `"sabado"` o `"domingo"` |
| `AptSim.time_total_franjas` | `number` (int) | `0` | `time.js` | Total de franjas transcurridas desde inicio |

**Valores válidos para `time_franja`** (sin tildes — crítico para comparaciones):
`"manana"` → `"tarde"` → `"noche"` → `"madrugada"` → (avanza día) → `"manana"`

**Valores válidos para `time_dia_semana`** (sin tildes):
`"lunes"` `"martes"` `"miercoles"` `"jueves"` `"viernes"` `"sabado"` `"domingo"`

**Regla de encoding:** Los valores internos de estas variables NUNCA llevan tildes.
La UI muestra texto con tilde al jugador. Las comparaciones en código usan sin tilde.

---

## 5. ESTADO DE ÁNIMO DIARIO (MOOD)

**Archivo responsable:** `js/custom/core/mood.js`
**Quién puede modificar:** solo `mood.js` mediante `fn_mood_recalcular(personaje)`
**Cuándo se recalcula:** al avanzar cada franja

| Variable | Tipo | Valor inicial | Valores válidos | Descripción |
|----------|------|--------------|-----------------|-------------|
| `AptSim.mood_celine` | `string` | `"normal"` | ver abajo | Estado de ánimo de Celine |
| `AptSim.mood_roxy` | `string` | `"normal"` | ver abajo | Estado de ánimo de Roxy |
| `AptSim.mood_luna` | `string` | `"normal"` | ver abajo | Estado de ánimo de Luna |

**Valores válidos** (sin tildes):
`"normal"` / `"feliz"` / `"cansada"` / `"estresada"` / `"traviesa"`

---

## 6. ARMONÍA DEL APARTAMENTO

**Archivo responsable:** `js/custom/core/jealousy.js`

| Variable | Tipo | Valor inicial | Rango | Descripción |
|----------|------|--------------|-------|-------------|
| `AptSim.apt_armonia` | `number` (int) | `70` | 0–100 | Armonía global del apartamento |

---

## 7. CELOS

**Archivo responsable:** `js/custom/core/jealousy.js`
**Quién puede modificar:** solo `jealousy.js`
**Cuándo se resetea `_activo`:** al avanzar de día (madrugada → mañana)
**La intensidad persiste** hasta que la causa se resuelva (ver GDD_SISTEMAS.md S20)

| Variable | Tipo | Valor inicial | Rango | Descripción |
|----------|------|--------------|-------|-------------|
| `AptSim.celos_celine_activo` | `boolean` | `false` | — | `true` si Celine tiene celos activos |
| `AptSim.celos_roxy_activo` | `boolean` | `false` | — | `true` si Roxy tiene celos activos |
| `AptSim.celos_luna_activo` | `boolean` | `false` | — | `true` si Luna tiene celos activos |
| `AptSim.celos_celine_intensidad` | `number` (int) | `0` | 0–3 | 0=ninguno 1=leve 2=moderado 3=intenso |
| `AptSim.celos_roxy_intensidad` | `number` (int) | `0` | 0–3 | 0=ninguno 1=leve 2=moderado 3=intenso |
| `AptSim.celos_luna_intensidad` | `number` (int) | `0` | 0–3 | 0=ninguno 1=leve 2=moderado 3=intenso |

---

## 8. ACCESO A CUARTOS

**Archivo responsable:** `js/custom/core/stats.js` (verificación y escritura)
**Quién usa:** eventos del editor de MZ (lectura mediante Script Call)
**Regla de diseño:** Una vez `true`, NUNCA vuelve a `false`.

| Variable | Tipo | Valor inicial | Condición de desbloqueo | Descripción |
|----------|------|--------------|------------------------|-------------|
| `AptSim.acceso_cuarto_roxy` | `boolean` | `false` | `rox_afecto >= 20` | El jugador puede entrar al cuarto de Roxy |
| `AptSim.acceso_cuarto_celine` | `boolean` | `false` | `cel_afecto >= 35` | El jugador puede entrar al cuarto de Celine |
| `AptSim.acceso_cuarto_luna` | `boolean` | `false` | `lun_afecto >= 45` | El jugador puede entrar al cuarto de Luna |

---

## 9. SECRETOS DESCUBRIBLES

**Archivo responsable:** `js/custom/core/secrets.js`
**Quién modifica:** `secrets.js` mediante `fn_secreto_descubrir(id)`
**Regla:** Una vez `true`, NUNCA vuelve a `false`. No se exponen directamente — usar `fn_secreto_conoce(id)`.

| Variable | Tipo | Valor inicial | Descripción |
|----------|------|--------------|-------------|
| `AptSim.sec_celine_1` | `boolean` | `false` | Diario de Celine descubierto (cajón escritorio) |
| `AptSim.sec_celine_2` | `boolean` | `false` | Conversación Celine-Roxy escuchada desde el pasillo |
| `AptSim.sec_celine_3` | `boolean` | `false` | Celine encontrada llorando de madrugada |
| `AptSim.sec_roxy_1` | `boolean` | `false` | Bocetos del protagonista en el sketchbook de Roxy |
| `AptSim.sec_roxy_2` | `boolean` | `false` | Fotos guardadas de ambos encontradas en el cajón |
| `AptSim.sec_roxy_3` | `boolean` | `false` | Conversación Roxy-Luna escuchada de noche |
| `AptSim.sec_luna_1` | `boolean` | `false` | Página del cuaderno con el nombre del protagonista |
| `AptSim.sec_luna_2` | `boolean` | `false` | Cartas que Luna nunca envió, en el fondo del cajón |
| `AptSim.sec_luna_3` | `boolean` | `false` | Luna encontrada llorando en cocina a las 3am |

### Flags de "secreto usado en contra" (traición)
Una vez `true`, el personaje lo recuerda en diálogos futuros permanentemente.

| Variable | Tipo | Valor inicial | Descripción |
|----------|------|--------------|-------------|
| `AptSim.sec_celine_traicion_usada` | `boolean` | `false` | El jugador usó un secreto de Celine en su contra |
| `AptSim.sec_roxy_traicion_usada` | `boolean` | `false` | El jugador usó un secreto de Roxy en su contra |
| `AptSim.sec_luna_traicion_usada` | `boolean` | `false` | El jugador usó un secreto de Luna en su contra |

---

## 10. PROGRESO — ACTOS NARRATIVOS

**Archivo responsable:** `js/custom/characters/[personaje].js` (escritura)
**Regla:** Una vez `true`, NUNCA vuelve a `false`. El evento del editor verifica este flag
antes de dispararse para no repetirse.

| Variable | Tipo | Valor inicial | Descripción |
|----------|------|--------------|-------------|
| `AptSim.ev_celine_acto1_done` | `boolean` | `false` | Acto 1 de Celine completado ("El apagón") |
| `AptSim.ev_celine_acto2_done` | `boolean` | `false` | Acto 2 de Celine completado ("Los apuntes") |
| `AptSim.ev_celine_acto3_done` | `boolean` | `false` | Acto 3 de Celine completado ("La fiesta de Roxy") |
| `AptSim.ev_celine_acto4_done` | `boolean` | `false` | Acto 4 de Celine completado ("La confesión a medias") |
| `AptSim.ev_celine_acto5_done` | `boolean` | `false` | Acto 5 de Celine completado ("Sin barreras") |
| `AptSim.ev_roxy_acto1_done` | `boolean` | `false` | Acto 1 de Roxy completado ("El sketchbook") |
| `AptSim.ev_roxy_acto2_done` | `boolean` | `false` | Acto 2 de Roxy completado ("La apuesta") |
| `AptSim.ev_roxy_acto3_done` | `boolean` | `false` | Acto 3 de Roxy completado ("La pelea") |
| `AptSim.ev_roxy_acto4_done` | `boolean` | `false` | Acto 4 de Roxy completado ("2am") |
| `AptSim.ev_roxy_acto5_done` | `boolean` | `false` | Acto 5 de Roxy completado ("Sin actuar") |
| `AptSim.ev_luna_acto1_done` | `boolean` | `false` | Acto 1 de Luna completado ("El libro") |
| `AptSim.ev_luna_acto2_done` | `boolean` | `false` | Acto 2 de Luna completado ("La madrugada") |
| `AptSim.ev_luna_acto3_done` | `boolean` | `false` | Acto 3 de Luna completado ("El cuaderno") |
| `AptSim.ev_luna_acto4_done` | `boolean` | `false` | Acto 4 de Luna completado ("Una frase") |
| `AptSim.ev_luna_acto5_done` | `boolean` | `false` | Acto 5 de Luna completado ("En voz alta") |

---

## 11. PROGRESO — QUESTS

**Archivo responsable:** `js/custom/characters/[personaje].js` (escritura)
**Regla:** Una vez `true`, NUNCA vuelve a `false`.

| Variable | Tipo | Valor inicial | Quest |
|----------|------|--------------|-------|
| `AptSim.quest_celine_1_done` | `boolean` | `false` | "Los apuntes" |
| `AptSim.quest_celine_2_done` | `boolean` | `false` | "La defensa" |
| `AptSim.quest_celine_3_done` | `boolean` | `false` | "El diario" |
| `AptSim.quest_roxy_1_done` | `boolean` | `false` | "El sketchbook" |
| `AptSim.quest_roxy_2_done` | `boolean` | `false` | "La exposición" |
| `AptSim.quest_roxy_3_done` | `boolean` | `false` | "La noche que llora" |
| `AptSim.quest_luna_1_done` | `boolean` | `false` | "El libro" |
| `AptSim.quest_luna_2_done` | `boolean` | `false` | "Tres días de silencio" |
| `AptSim.quest_luna_3_done` | `boolean` | `false` | "El mismo libro" |

### Flags de resultado de quest (para consecuencias a largo plazo)
Algunas quests guardan qué decisión tomó el jugador, no solo si la completó.
Estos flags desbloquean o bloquean diálogos futuros.

| Variable | Tipo | Valor inicial | Descripción |
|----------|------|--------------|-------------|
| `AptSim.quest_celine_3_result` | `string` | `""` | `"leyo"` / `"cerro"` / `"pregunto"` — decisión en "El diario" |
| `AptSim.quest_roxy_2_result` | `string` | `""` | `"fue"` / `"no_fue"` — decisión en "La exposición" |
| `AptSim.quest_luna_2_completada_limpia` | `boolean` | `false` | `true` si logró los 3 días sin hablar con Luna |

---

## 12. ECONOMÍA

**Archivo responsable:** `js/custom/core/economy.js`
**Quién puede modificar:** solo `economy.js`

| Variable | Tipo | Valor inicial | Descripción |
|----------|------|--------------|-------------|
| `AptSim.item_camara` | `boolean` | `false` | Cámara compacta comprada — habilita sistemas S15 y S16 |
| `AptSim.econ_turnos_trabajados` | `number` (int) | `0` | Turnos trabajados en la franja actual (máx 2) |
| `AptSim.econ_ultima_franja_trabajo` | `string` | `"ninguna"` | Franja en que se trabajó por última vez |

**Nota:** `econ_turnos_trabajados` se resetea a `0` al avanzar de franja.
`econ_ultima_franja_trabajo` se usa para detectar si el jugador ya trabajó hoy y cuándo.

---

## 13. GALERÍA

**Archivo responsable:** `js/custom/systems/gallery.js`
**Regla:** Estos flags son `true` de por vida. Nunca vuelven a `false`.
**Importante:** La galería debe persistir entre partidas. El Dev Core implementa
el mecanismo de persistencia al manejar los hooks de save/load.

### Actos narrativos desbloqueados en galería
| Variable | Tipo | Valor inicial | Descripción |
|----------|------|--------------|-------------|
| `AptSim.gal_celine_acto_1` | `boolean` | `false` | Escena Acto 1 Celine en galería |
| `AptSim.gal_celine_acto_2` | `boolean` | `false` | Escena Acto 2 Celine en galería |
| `AptSim.gal_celine_acto_3` | `boolean` | `false` | Escena Acto 3 Celine en galería |
| `AptSim.gal_celine_acto_4` | `boolean` | `false` | Escena Acto 4 Celine en galería |
| `AptSim.gal_celine_acto_5` | `boolean` | `false` | Escena Acto 5 Celine en galería |
| `AptSim.gal_roxy_acto_1` | `boolean` | `false` | Escena Acto 1 Roxy en galería |
| `AptSim.gal_roxy_acto_2` | `boolean` | `false` | Escena Acto 2 Roxy en galería |
| `AptSim.gal_roxy_acto_3` | `boolean` | `false` | Escena Acto 3 Roxy en galería |
| `AptSim.gal_roxy_acto_4` | `boolean` | `false` | Escena Acto 4 Roxy en galería |
| `AptSim.gal_roxy_acto_5` | `boolean` | `false` | Escena Acto 5 Roxy en galería |
| `AptSim.gal_luna_acto_1` | `boolean` | `false` | Escena Acto 1 Luna en galería |
| `AptSim.gal_luna_acto_2` | `boolean` | `false` | Escena Acto 2 Luna en galería |
| `AptSim.gal_luna_acto_3` | `boolean` | `false` | Escena Acto 3 Luna en galería |
| `AptSim.gal_luna_acto_4` | `boolean` | `false` | Escena Acto 4 Luna en galería |
| `AptSim.gal_luna_acto_5` | `boolean` | `false` | Escena Acto 5 Luna en galería |

### Escenas voyeur desbloqueadas
| Variable | Tipo | Valor inicial | Descripción |
|----------|------|--------------|-------------|
| `AptSim.gal_celine_voyeur_1` | `boolean` | `false` | Voyeur 1 Celine desbloqueado |
| `AptSim.gal_celine_voyeur_2` | `boolean` | `false` | Voyeur 2 Celine desbloqueado |
| `AptSim.gal_celine_voyeur_3` | `boolean` | `false` | Voyeur 3 Celine desbloqueado |
| `AptSim.gal_roxy_voyeur_1` | `boolean` | `false` | Voyeur 1 Roxy desbloqueado |
| `AptSim.gal_roxy_voyeur_2` | `boolean` | `false` | Voyeur 2 Roxy desbloqueado |
| `AptSim.gal_roxy_voyeur_3` | `boolean` | `false` | Voyeur 3 Roxy desbloqueado |
| `AptSim.gal_luna_voyeur_1` | `boolean` | `false` | Voyeur 1 Luna desbloqueado |
| `AptSim.gal_luna_voyeur_2` | `boolean` | `false` | Voyeur 2 Luna desbloqueado |
| `AptSim.gal_luna_voyeur_3` | `boolean` | `false` | Voyeur 3 Luna desbloqueado |

### Fotos coleccionables
10 fotos por personaje. Formato: `gal_[personaje]_foto_[1-10]`

| Variable | Tipo | Valor inicial |
|----------|------|--------------|
| `AptSim.gal_celine_foto_1` … `gal_celine_foto_10` | `boolean` | `false` |
| `AptSim.gal_roxy_foto_1` … `gal_roxy_foto_10` | `boolean` | `false` |
| `AptSim.gal_luna_foto_1` … `gal_luna_foto_10` | `boolean` | `false` |

*(30 variables en total — se declaran en el loop de inicialización en `vars.js`)*

---

## 14. MEMORIA DE DECISIONES

**Archivo responsable:** `js/custom/core/memory.js`
**Quién puede modificar:** `memory.js` mediante `fn_mem_registrar(id)`

| Variable | Tipo | Valor inicial | Límite | Descripción |
|----------|------|--------------|--------|-------------|
| `AptSim.mem_decisiones` | `Array<string>` | `[]` | 20 elementos | IDs de decisiones importantes tomadas por el jugador |

**Patrón obligatorio de escritura:**
```javascript
// NUNCA push directo
// SIEMPRE reasignar
AptSim.mem_decisiones = [...AptSim.mem_decisiones, id_decision];
if (AptSim.mem_decisiones.length > 20) {
    AptSim.mem_decisiones = AptSim.mem_decisiones.slice(-20);
}
```

**Función de lectura:**
```javascript
// fn_mem_tiene(id) — retorna bool
// Usar desde eventos del editor con Script Call
fn_mem_tiene("quest_celine_ayude_bien");
```

**IDs de decisiones registradas** (lista que crece con el contenido — Dev Contenido las define):
- Formato: `[contexto]_[decision]` — ejemplo: `"celine_acto3_beso"`, `"roxy_exposicion_no_fue"`

---

## 15. MINI-JUEGOS (VARIABLES TEMPORALES)

**Archivo responsable:** `js/custom/systems/minigames.js`
**Tipo:** Temporales — solo existen durante una partida de mini-juego activa.
No se guardan en save. Se resetean al iniciar cada mini-juego.

| Variable | Tipo | Valor inicial | Descripción |
|----------|------|--------------|-------------|
| `AptSim.mini_mano_jugador` | `Array<number>` | `[]` | Cartas actuales del jugador (Strip 21) |
| `AptSim.mini_mano_rival` | `Array<number>` | `[]` | Cartas actuales del personaje (Strip 21) |
| `AptSim.mini_total_jugador` | `number` (int) | `0` | Suma de la mano del jugador |
| `AptSim.mini_total_rival` | `number` (int) | `0` | Suma de la mano del rival |
| `AptSim.mini_personaje_activo` | `string` | `""` | Personaje con quien se juega (`"celine"` / `"roxy"` / `"luna"`) |
| `AptSim.mini_turno_actual` | `number` (int) | `0` | Turno actual en Verdad o Reto (impar=jugador elige, par=ella elige) |

---

## 16. FLAGS DE PROGRESIÓN ESPECIAL

Variables que no encajan en otras categorías pero son necesarias para el flujo del juego.

**Archivo responsable:** `js/custom/core/vars.js`

| Variable | Tipo | Valor inicial | Quién modifica | Descripción |
|----------|------|--------------|----------------|-------------|
| `AptSim.tutorial_completo` | `boolean` | `false` | evento del editor (día 1) | `true` después del tutorial del día 1 |
| `AptSim.ejercicio_dias_consecutivos` | `number` (int) | `0` | `stats.js` | Contador de días de ejercicio consecutivos en mañana |
| `AptSim.ejercicio_ultima_franja` | `string` | `""` | `stats.js` | Última franja en que se hizo ejercicio (para detectar consecutivos) |
| `AptSim.voyeur_bloqueado_celine` | `boolean` | `false` | `systems/gallery.js` | Bloqueado del voyeur de Celine esta semana |
| `AptSim.voyeur_bloqueado_roxy` | `boolean` | `false` | `systems/gallery.js` | Bloqueado del voyeur de Roxy esta semana |
| `AptSim.voyeur_bloqueado_luna` | `boolean` | `false` | `systems/gallery.js` | Bloqueado del voyeur de Luna esta semana |
| `AptSim.luna_dias_sin_hablar` | `number` (int) | `0` | `time.js` | Contador para la quest "Tres días de silencio" de Luna |

---

## 17. FLAGS DE DEBUG (NO SE EMPAQUETAN EN BUILD)

**Archivo responsable:** `js/custom/core/vars.js`
**⚠ Eliminar o poner en `false` antes de cualquier build de distribución.**

| Variable | Tipo | Valor inicial | Descripción |
|----------|------|--------------|-------------|
| `AptSim.DEBUG_MODE` | `boolean` | `false` | Activa console.log en todos los sistemas |
| `AptSim.DEBUG_stats_libres` | `boolean` | `false` | Ignora requisitos mínimos de Corrupción |
| `AptSim.DEBUG_skip_tutorial` | `boolean` | `false` | Salta el día 1 lineal al iniciar partida |

---

## RESUMEN — DECLARACIÓN POR ARCHIVO JS

### `js/custom/core/vars.js` — objeto principal + inicializador + hooks de save

```javascript
var AptSim = {};

function fn_vars_inicializar() {
    // Stats de personajes
    AptSim.cel_afecto = 15;    AptSim.cel_confianza = 10;    AptSim.cel_corrupcion = 5;
    AptSim.rox_afecto = 25;    AptSim.rox_confianza = 15;    AptSim.rox_corrupcion = 15;
    AptSim.lun_afecto = 20;    AptSim.lun_confianza = 10;    AptSim.lun_corrupcion = 10;

    // Protagonista
    AptSim.prot_nombre = "Kai";
    AptSim.prot_encanto = 10;
    AptSim.prot_energia_max = 6;
    AptSim.prot_energia_actual = 6;
    AptSim.prot_dinero = 500;

    // Personalidad
    AptSim.prot_romantico = 0;
    AptSim.prot_atrevido = 0;
    AptSim.prot_respetuoso = 0;
    AptSim.prot_arquetipo_dominante = "";

    // Tiempo
    AptSim.time_dia = 1;
    AptSim.time_franja = "manana";
    AptSim.time_dia_semana = "lunes";
    AptSim.time_es_finde = false;
    AptSim.time_total_franjas = 0;

    // Mood
    AptSim.mood_celine = "normal";
    AptSim.mood_roxy = "normal";
    AptSim.mood_luna = "normal";

    // Armonía y celos
    AptSim.apt_armonia = 70;
    AptSim.celos_celine_activo = false;
    AptSim.celos_roxy_activo = false;
    AptSim.celos_luna_activo = false;
    AptSim.celos_celine_intensidad = 0;
    AptSim.celos_roxy_intensidad = 0;
    AptSim.celos_luna_intensidad = 0;

    // Acceso a cuartos
    AptSim.acceso_cuarto_roxy = false;
    AptSim.acceso_cuarto_celine = false;
    AptSim.acceso_cuarto_luna = false;

    // Secretos (9 + 3 flags de traición)
    ["celine","roxy","luna"].forEach(p => {
        for (let i = 1; i <= 3; i++) AptSim["sec_" + p + "_" + i] = false;
        AptSim["sec_" + p + "_traicion_usada"] = false;
    });

    // Actos narrativos (15 flags)
    ["celine","roxy","luna"].forEach(p => {
        for (let i = 1; i <= 5; i++) AptSim["ev_" + p + "_acto" + i + "_done"] = false;
    });

    // Quests (9 flags + 3 de resultado)
    ["celine","roxy","luna"].forEach(p => {
        for (let i = 1; i <= 3; i++) AptSim["quest_" + p + "_" + i + "_done"] = false;
    });
    AptSim.quest_celine_3_result = "";
    AptSim.quest_roxy_2_result = "";
    AptSim.quest_luna_2_completada_limpia = false;

    // Economía
    AptSim.item_camara = false;
    AptSim.econ_turnos_trabajados = 0;
    AptSim.econ_ultima_franja_trabajo = "ninguna";

    // Galería — actos (15)
    ["celine","roxy","luna"].forEach(p => {
        for (let i = 1; i <= 5; i++) AptSim["gal_" + p + "_acto_" + i] = false;
    });
    // Galería — voyeur (9)
    ["celine","roxy","luna"].forEach(p => {
        for (let i = 1; i <= 3; i++) AptSim["gal_" + p + "_voyeur_" + i] = false;
    });
    // Galería — fotos (30)
    ["celine","roxy","luna"].forEach(p => {
        for (let i = 1; i <= 10; i++) AptSim["gal_" + p + "_foto_" + i] = false;
    });

    // Memoria de decisiones
    AptSim.mem_decisiones = [];

    // Mini-juegos (temporales)
    AptSim.mini_mano_jugador = [];
    AptSim.mini_mano_rival = [];
    AptSim.mini_total_jugador = 0;
    AptSim.mini_total_rival = 0;
    AptSim.mini_personaje_activo = "";
    AptSim.mini_turno_actual = 0;

    // Progresión especial
    AptSim.tutorial_completo = false;
    AptSim.ejercicio_dias_consecutivos = 0;
    AptSim.ejercicio_ultima_franja = "";
    AptSim.voyeur_bloqueado_celine = false;
    AptSim.voyeur_bloqueado_roxy = false;
    AptSim.voyeur_bloqueado_luna = false;
    AptSim.luna_dias_sin_hablar = 0;

    // Debug (SIEMPRE false en build)
    AptSim.DEBUG_MODE = false;
    AptSim.DEBUG_stats_libres = false;
    AptSim.DEBUG_skip_tutorial = false;
}
```

### Hooks de save — también en `vars.js`

```javascript
// Guardar AptSim en el save de MZ
const _DataManager_makeSave = DataManager.makeSaveContents;
DataManager.makeSaveContents = function() {
    const contents = _DataManager_makeSave.call(this);
    contents.aptSim = JSON.parse(JSON.stringify(AptSim)); // copia profunda
    return contents;
};

// Restaurar AptSim al cargar save
const _DataManager_extractSave = DataManager.extractSaveContents;
DataManager.extractSaveContents = function(contents) {
    _DataManager_extractSave.call(this, contents);
    if (contents.aptSim) {
        AptSim = contents.aptSim;
    } else {
        fn_vars_inicializar(); // save antiguo sin datos — inicializar de cero
    }
};
```

---

## CONTEO TOTAL DE VARIABLES

| Categoría | Cantidad |
|-----------|----------|
| Stats de personajes | 9 |
| Stats del protagonista | 5 |
| Personalidad | 4 |
| Sistema de tiempo | 5 |
| Mood | 3 |
| Armonía | 1 |
| Celos | 6 |
| Acceso a cuartos | 3 |
| Secretos | 12 (9 + 3 traición) |
| Actos narrativos | 15 |
| Quests | 12 (9 + 3 resultado) |
| Economía | 3 |
| Galería | 54 (15 actos + 9 voyeur + 30 fotos) |
| Memoria | 1 (array) |
| Mini-juegos | 6 |
| Progresión especial | 7 |
| Debug | 3 |
| **TOTAL** | **149** |

---

*Documento generado por el Arquitecto — Apartment Sim v1.0 RPG Maker MZ*
*Siguiente documento: ESTANDARES.md*
