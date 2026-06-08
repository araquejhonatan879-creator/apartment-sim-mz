# APARTMENT SIM — ESTÁNDARES DE DESARROLLO
### Versión 1.0 — RPG Maker MZ | Generado: 07/06/2026

---

## INSTRUCCIÓN PARA DEVS

Este documento se lee COMPLETO antes de escribir una sola línea de código o tocar
un solo archivo del proyecto. No es opcional. No tiene secciones opcionales.

**Si algo que necesitas hacer no está cubierto aquí, para y consulta al Arquitecto.**
No improvises convenciones. No inventes patrones. No "adaptes" reglas porque
te parecen innecesarias. Están aquí porque evitan bugs que tardan horas en encontrarse.

Orden de lectura obligatorio antes de cualquier sesión de trabajo:
1. Este documento (ESTANDARES.md) — completo
2. VARIABLES_GLOBALES.md — sección de tu archivo responsable
3. GDD_SISTEMAS.md — sistemas que vas a tocar
4. Tu ESTADO_ACTUAL_[ROL].md — para saber exactamente dónde estás

---

## 1. ARQUITECTURA DE ARCHIVOS

### Carpetas que existen y lo que va en cada una

```
js/
├── plugins/          ← PROHIBIDO. Solo se agregan plugins, nunca se editan.
└── custom/
    ├── core/         ← Sistemas base. Solo Dev Core.
    │   ├── vars.js       ← Objeto AptSim, fn_vars_inicializar(), hooks de save
    │   ├── stats.js      ← fn_stats_modificar(), verificación de requisitos
    │   ├── time.js       ← fn_tiempo_avanzar_franja(), restauración de energía
    │   ├── economy.js    ← fn_economia_trabajar(), fn_economia_comprar()
    │   ├── mood.js       ← fn_mood_recalcular()
    │   ├── secrets.js    ← fn_secreto_descubrir(), fn_secreto_conoce()
    │   ├── jealousy.js   ← fn_celos_actualizar(), fn_armonia_modificar()
    │   └── memory.js     ← fn_mem_registrar(), fn_mem_tiene()
    ├── characters/   ← Lógica por personaje. Dev Core define estructura, Dev Contenido usa.
    │   ├── celine.js     ← Verificadores de eventos y quests de Celine
    │   ├── roxy.js       ← Verificadores de eventos y quests de Roxy
    │   └── luna.js       ← Verificadores de eventos y quests de Luna
    └── systems/      ← Sistemas de pantalla y mini-juegos. Dev Contenido.
        ├── minigames.js  ← Strip 21 y Verdad o Reto
        ├── gallery.js    ← Galería y sistema voyeur
        └── phone.js      ← Mensajes de texto y teléfono
```

### Reglas absolutas de carpetas

- **`js/plugins/`** — NUNCA se toca. Si necesitas un plugin nuevo, lo agregas como
  archivo nuevo. Nunca editas un plugin existente.
- **`js/custom/core/`** — Solo Dev Core escribe aquí. Dev Contenido solo lee y llama
  funciones. Nunca crea archivos nuevos en esta carpeta.
- **`js/custom/characters/`** — Dev Core define las funciones de verificación.
  Dev Contenido las llama desde el editor de MZ via Script Call.
- **`js/custom/systems/`** — Dev Contenido. Dev Core no escribe aquí.
- **`_dev/`** — Solo archivos de estado. Nunca se empaqueta en build final.
- **`data/`** — Archivos JSON del engine de MZ. NUNCA se editan a mano.
  Solo el editor de MZ los modifica.

---

## 2. CONVENCIONES DE NOMENCLATURA

### Funciones JS

Formato: `fn_[modulo]_[accion](parametros)`

| Módulo | Prefijo | Ejemplos |
|--------|---------|---------|
| vars | `fn_vars_` | `fn_vars_inicializar()` |
| stats | `fn_stats_` | `fn_stats_modificar(personaje, stat, delta)` |
| tiempo | `fn_tiempo_` | `fn_tiempo_avanzar_franja()`, `fn_tiempo_es_finde()` |
| economía | `fn_economia_` | `fn_economia_trabajar()`, `fn_economia_comprar(item)` |
| mood | `fn_mood_` | `fn_mood_recalcular(personaje)`, `fn_mood_obtener(personaje)` |
| secretos | `fn_secreto_` | `fn_secreto_descubrir(id)`, `fn_secreto_conoce(id)` |
| celos | `fn_celos_` | `fn_celos_actualizar()`, `fn_armonia_modificar(delta)` |
| memoria | `fn_mem_` | `fn_mem_registrar(id)`, `fn_mem_tiene(id)` |
| mini-juegos | `fn_mini_` | `fn_mini_generar_carta()`, `fn_mini_valor_mano(mano)` |
| galería | `fn_gal_` | `fn_gal_desbloquear(personaje, tipo, numero)` |
| verificación | `fn_check_` | `fn_check_acto_disponible(personaje, acto)` |
| debug | `fn_debug_` | `fn_debug_set_stats(personaje, a, c, cor)` |

**Reglas:**
- Todo en minúsculas. Sin camelCase. Sin mayúsculas en nombres de función.
- Los parámetros de personaje siempre como string: `"celine"` / `"roxy"` / `"luna"`
- Los parámetros de stat siempre como string: `"afecto"` / `"confianza"` / `"corrupcion"`
- Las funciones no retornan objetos complejos a eventos del editor — solo
  números, strings o booleans. El editor de MZ no procesa objetos JS nativamente.

### Variables del objeto AptSim

Formato: `AptSim.[prefijo_personaje]_[nombre_variable]`

| Tipo de variable | Prefijo | Ejemplo |
|-----------------|---------|---------|
| Stats Celine | `cel_` | `AptSim.cel_afecto` |
| Stats Roxy | `rox_` | `AptSim.rox_confianza` |
| Stats Luna | `lun_` | `AptSim.lun_corrupcion` |
| Stats protagonista | `prot_` | `AptSim.prot_encanto` |
| Tiempo | `time_` | `AptSim.time_franja` |
| Mood | `mood_` | `AptSim.mood_celine` |
| Secretos | `sec_` | `AptSim.sec_celine_1` |
| Eventos/actos | `ev_` | `AptSim.ev_celine_acto1_done` |
| Quests | `quest_` | `AptSim.quest_roxy_2_done` |
| Galería | `gal_` | `AptSim.gal_luna_foto_3` |
| Economía | `econ_` | `AptSim.econ_turnos_trabajados` |
| Armonía | `apt_` | `AptSim.apt_armonia` |
| Celos | `celos_` | `AptSim.celos_celine_activo` |
| Acceso | `acceso_` | `AptSim.acceso_cuarto_roxy` |
| Mini-juegos | `mini_` | `AptSim.mini_mano_jugador` |
| Memoria | `mem_` | `AptSim.mem_decisiones` |
| Debug | `DEBUG_` | `AptSim.DEBUG_MODE` |

**Regla crítica:** Ninguna variable del objeto `AptSim` se declara en código
sin estar primero en VARIABLES_GLOBALES.md. Si necesitas una variable nueva,
para y avisa al usuario antes de crearla.

### Archivos JS

Formato: `[nombre].js` — todo minúsculas, sin guiones, sin espacios.
Ejemplos correctos: `vars.js`, `stats.js`, `minigames.js`
Ejemplos incorrectos: `Stats.js`, `mini-games.js`, `miniGames.js`

### Assets — sprites

Formato: `[personaje]_[outfit]_[expresion].png`
- `[personaje]`: `celine` / `roxy` / `luna`
- `[outfit]`: `outfit1` / `outfit2` / `outfit3` / `outfit4`
- `[expresion]`: ver tabla por personaje en GDD_PERSONAJES.md

Ejemplos correctos: `celine_outfit1_neutral.png`, `roxy_outfit3_traviesa.png`
Ejemplos incorrectos: `Celine_Outfit1_neutral.png`, `celine-outfit1.png`, `celine_neutral.png`

**Ubicación:** `img/characters/[personaje]/`

### Assets — fondos

Formato: `[locacion]_[franja].png`
- `[locacion]`: `sala`, `cocina`, `balcon`, `bano`, `cuarto_celine`, `cuarto_roxy`, `cuarto_luna`, `exterior`
- `[franja]`: `manana`, `tarde`, `noche`, `madrugada`

Ejemplos: `sala_noche.png`, `balcon_madrugada.png`, `cuarto_roxy_tarde.png`

**Ubicación:** `img/backgrounds/`

---

## 3. REGLAS DE CÓDIGO JS

### Encoding — regla sin excepciones

**Las variables internas NUNCA llevan tildes ni caracteres especiales.**
La UI muestra texto con tilde al jugador. El código usa siempre sin tilde.

```javascript
// CORRECTO
if (AptSim.time_franja === "manana") { ... }
if (AptSim.time_dia_semana === "miercoles") { ... }

// INCORRECTO — rompe comparaciones
if (AptSim.time_franja === "mañana") { ... }
if (AptSim.time_dia_semana === "miércoles") { ... }
```

Valores internos sin tilde obligatorios:
- Franjas: `"manana"` `"tarde"` `"noche"` `"madrugada"`
- Días: `"lunes"` `"martes"` `"miercoles"` `"jueves"` `"viernes"` `"sabado"` `"domingo"`
- Moods: `"normal"` `"feliz"` `"cansada"` `"estresada"` `"traviesa"`
- Personajes: `"celine"` `"roxy"` `"luna"`
- Stats: `"afecto"` `"confianza"` `"corrupcion"`
- Arquetipo: `"romantico"` `"atrevido"` `"respetuoso"`

### Comentarios — en español, siempre

Todo comentario en español. Sin excepción. Si copias código de documentación
oficial en inglés, el comentario que explica qué hace va en español.

```javascript
// CORRECTO
// Verificar que Afecto cumpla el mínimo antes de subir Corrupción
if (AptSim.cel_afecto < requisito_minimo) return;

// INCORRECTO
// Check that Affection meets the minimum before increasing Corruption
if (AptSim.cel_afecto < requisito_minimo) return;
```

Formato de comentario de cabecera en cada archivo:

```javascript
// ============================================================
// APARTMENT SIM — [NOMBRE DEL MÓDULO]
// Archivo: js/custom/[carpeta]/[archivo].js
// Responsable: Dev [Rol]
// Descripción: [Qué hace este archivo en una línea]
// Depende de: [archivos que este necesita]
// Modificado por: [archivos que pueden llamar funciones de este]
// ============================================================
```

### Arrays — patrón obligatorio

En JavaScript los arrays son referencias. Mutar un array puede causar bugs
en el sistema de saves. Siempre reasignar, nunca mutar.

```javascript
// CORRECTO — siempre reasignar
AptSim.mem_decisiones = [...AptSim.mem_decisiones, nueva_decision];

// INCORRECTO — muta la referencia original
AptSim.mem_decisiones.push(nueva_decision);
```

Lo mismo aplica para cualquier array temporal de mini-juegos:

```javascript
// CORRECTO
AptSim.mini_mano_jugador = [...AptSim.mini_mano_jugador, nueva_carta];

// INCORRECTO
AptSim.mini_mano_jugador.push(nueva_carta);
```

### Modificación de stats — solo a través de la función

Ningún evento del editor de MZ modifica stats directamente en `AptSim`.
Siempre a través de `fn_stats_modificar()`. Sin excepción.

```javascript
// CORRECTO — desde un Script Call en el editor de MZ
fn_stats_modificar("celine", "afecto", 5);

// INCORRECTO — modificación directa
AptSim.cel_afecto += 5;
AptSim.cel_afecto = AptSim.cel_afecto + 5;
```

La función `fn_stats_modificar` es la única que verifica rangos (0-100),
requisitos de Corrupción y efectos secundarios. Si se bypasea, el juego
puede quedar en estado inválido.

### Clamping — siempre al modificar números con rango

Cada función que modifica un valor numérico con rango definido debe hacer clamping.

```javascript
// Patrón de clamping
valor = Math.max(0, Math.min(100, valor + delta));

// fn_stats_modificar lo hace internamente — no lo repitas en los eventos
// Pero si escribes una función que modifica otro número con rango, clampea.
```

### Variables locales dentro de funciones

Siempre declarar con `const` o `let`. Nunca `var` en código nuevo.
`var` solo aparece en la declaración global `var AptSim = {}` en `vars.js`.

```javascript
// CORRECTO
const resultado = fn_mini_valor_mano(AptSim.mini_mano_jugador);
let delta_afecto = 5;

// INCORRECTO
resultado = fn_mini_valor_mano(AptSim.mini_mano_jugador);
var delta_afecto = 5;
```

### Manejo de errores en funciones críticas

Las funciones de sistema deben fallar silenciosamente hacia el jugador
pero registrar en consola si DEBUG_MODE está activo.

```javascript
// Patrón de error silencioso con log de debug
function fn_stats_modificar(personaje, stat, delta) {
    if (!personaje || !stat || delta === undefined) {
        if (AptSim.DEBUG_MODE) console.warn("[AptSim] fn_stats_modificar: parámetros inválidos", personaje, stat, delta);
        return;
    }
    // ... lógica
}
```

---

## 4. INTEGRACIÓN CON EL EDITOR DE RPG MAKER MZ

### Script Call — cómo llamar funciones JS desde eventos

En el editor de MZ, dentro de un evento, se usa el comando **Script** (tercera página
de comandos de evento, categoría "Avanzado") para llamar funciones JS.

```
// En el editor de MZ — comando Script:
fn_stats_modificar("roxy", "afecto", 10);
```

Para leer un valor y guardarlo en una variable del engine de MZ:
```
// Guardar resultado de función JS en variable MZ #0001
$gameVariables.setValue(1, fn_mood_obtener("celine"));
```

**Regla:** Las funciones JS que se llaman desde el editor no pueden retornar
objetos — solo números, strings o booleans. El editor de MZ no procesa
objetos JS natively. Si necesitas múltiples valores, llama funciones separadas.

### Variables del engine de MZ vs objeto AptSim

El proyecto usa dos sistemas de variables en paralelo:

| Sistema | Qué guarda | Cuándo usar |
|---------|------------|-------------|
| `AptSim` (JS) | Todo el estado del juego | Siempre — es la fuente de verdad |
| `$gameVariables` (MZ engine) | Valores que el editor necesita leer directamente | Solo cuando un comando nativo de MZ necesita el valor |

**Regla:** La fuente de verdad es siempre `AptSim`. Si el editor necesita
un valor, se copia de `AptSim` a `$gameVariables` antes de usarlo — no al revés.

```javascript
// Copiar valor de AptSim a variable MZ para que un comando nativo lo use
$gameVariables.setValue(1, AptSim.cel_afecto);
// Después del bloque nativo que lo usa, actualizar AptSim si cambió:
AptSim.cel_afecto = $gameVariables.value(1);
```

### Show Choices — formato obligatorio

Cuando un evento del editor presenta opciones al jugador vinculadas a arquetipos,
el comentario encima del Show Choices debe indicar qué arquetipo corresponde a cada opción.

```
// Comentario en el evento (visible en el editor):
// Opción 1 = Romántico | Opción 2 = Atrevido | Opción 3 = Respetuoso
◆Show Choices: "Opción 1", "Opción 2", "Opción 3"
```

### Verificación de condiciones antes de disparar eventos

Todo evento narrativo (actos, quests) debe verificar sus condiciones con
una función JS antes de ejecutar el contenido. No usar condiciones nativas
del editor para verificar stats — son lentas y propensas a error.

```javascript
// En Conditional Branch del editor — llamar la función verificadora
fn_check_acto_disponible("celine", 2)
// Si retorna true: ejecutar el evento. Si false: no ejecutar.
```

---

## 5. SISTEMA DE SAVES — REGLAS DEL DEV CORE

Esta sección es solo para Dev Core. Los demás devs no tocan nada relacionado con saves.

### Qué se guarda y cómo

El objeto completo `AptSim` se serializa en el save de MZ mediante los hooks
en `vars.js`. El patrón obligatorio (ya definido en VARIABLES_GLOBALES.md):

```javascript
// Al guardar — copia profunda obligatoria
contents.aptSim = JSON.parse(JSON.stringify(AptSim));

// Al cargar — restauración con fallback
if (contents.aptSim) {
    AptSim = contents.aptSim;
} else {
    fn_vars_inicializar();
}
```

**Por qué copia profunda:** Sin `JSON.parse(JSON.stringify(...))`, los arrays
y objetos anidados guardan referencias, no valores. Al modificar `AptSim`
después de guardar, el save se corrompería silenciosamente.

### Qué NO se guarda en el save

Las variables temporales de mini-juegos (`mini_mano_jugador`, etc.) no
necesitan guardarse — se resetean al inicio de cada partida de mini-juego.
Se serializan igual porque están en `AptSim`, pero no tienen estado
crítico entre sesiones.

---

## 6. NOMENCLATURA DE EVENTOS EN EL EDITOR DE MZ

### Nombres de mapas

Formato: `[Locacion]_[Descriptor]`
Ejemplos: `Apartamento_Principal`, `Exterior_Cafeteria`, `Balcon_Noche`

### Nombres de eventos dentro del editor

Formato: `[Personaje/Sistema]_[TipoEvento]_[Identificador]`
Ejemplos:
- `Celine_Acto1_Apagon`
- `Roxy_Quest2_Exposicion`
- `Sistema_Tutorial_Dia1`
- `Espontaneo_Apagon_Global`
- `Voyeur_Celine_1`

**Regla:** Nunca dejar un evento sin nombre en el editor. Los eventos sin nombre
son imposibles de rastrear cuando hay un bug.

### Comentarios en eventos del editor

El primer comando de todo evento narrativo debe ser un Comentario con:
```
[EVENTO] Nombre del evento
[TRIGGER] Condición que lo disparó
[PERSONAJE] A quién afecta
[ARCHIVO] js/custom/... que llama este evento
```

---

## 7. REGLAS DE ARTE — PARA DEV ARTE

### Sprites

- Resolución de generación: 768x512 en SD Forge, escalado a tamaño final en el juego
- Formato final: PNG con transparencia
- Fondo: siempre transparente
- Seed fija por personaje — la misma seed para todas las expresiones y outfits
  de ese personaje garantiza consistencia facial
- LoRA activo por personaje durante toda la generación de ese personaje

### Fondos

- Resolución: 1280x720 (resolución final del juego, sin escalado)
- Formato: PNG o JPG (JPG para fondos sin transparencia)
- Una versión por franja horaria: mañana (luz natural clara), tarde (luz cálida),
  noche (oscuro con luces artificiales), madrugada (oscuridad casi total)

### Naming y entrega

Antes de entregar cualquier asset al proyecto:
1. Verificar que el nombre sigue el formato de la sección 2
2. Verificar que la resolución es correcta
3. Verificar que el personaje es visualmente consistente con los otros sprites
   de esa misma personaje (mismo LoRA, misma seed base)
4. Actualizar ESTADO_ACTUAL_ARTE.md con la lista de lo entregado

### Lo que NO se reutiliza del proyecto anterior

Los assets del proyecto Ren'Py no se usan. Todo el arte parte de cero
con SD Forge + LoRA entrenado para este proyecto. Si ves assets del proyecto
anterior en alguna carpeta, no los uses — consulta al Arquitecto.

---

## 8. FLUJO DE TRABAJO POR SESIÓN

### Al inicio de cada sesión

1. Leer ESTADO_ACTUAL_MAESTRO.md
2. Leer tu propio ESTADO_ACTUAL_[ROL].md
3. Leer los GDDs relevantes para lo que vas a implementar
4. Verificar que no hay conflictos con lo que otro dev está trabajando
5. Si vas a tocar un archivo que no es tuyo: avisar antes de empezar

### Durante la sesión

- Máximo 2-3 archivos modificados por sesión
- Cada función que escribas, prueba en consola antes de conectar al editor
- Si encuentras un bug en código de otro dev: **no lo arregles sin avisar**
  Documenta el bug en tu ESTADO_ACTUAL y avisa al usuario
- Si necesitas una variable que no existe en VARIABLES_GLOBALES.md: para y avisa

### Al cerrar la sesión

1. Actualizar ESTADO_ACTUAL_[ROL].md con exactamente lo que se hizo
2. Documentar cualquier decisión de implementación que tomaste
3. Documentar cualquier problema encontrado y su estado (resuelto / pendiente)
4. Hacer commit a GitHub con el formato:
   `[Dev Rol] descripción breve — ESTADO_ACTUAL actualizado`
5. Verificar que `AptSim.DEBUG_MODE = false` antes del commit

### Formato de commit

```
[Dev Core] implementa fn_stats_modificar con verificación de Corrupción — ESTADO_ACTUAL actualizado
[Dev Contenido] acto 1 Celine completo con tres decisiones — ESTADO_ACTUAL actualizado
[Dev Arte] sprites celine outfit1 completos (7 expresiones) — ESTADO_ACTUAL actualizado
[Dev Debugger] corrige bug en fn_tiempo_avanzar_franja — ESTADO_ACTUAL actualizado
```

---

## 9. HANDOFF ENTRE DEVS — CUÁNDO PASAR EL TESTIGO

### Dev Core → Dev Contenido

Dev Core entrega cuando:
- `vars.js` completo y `fn_vars_inicializar()` funciona sin errores en consola
- `stats.js` implementado con `fn_stats_modificar()` verificada con tests en consola
- `time.js` implementado con avance de franja funcional
- `economy.js` con trabajo y compra básicos funcionales
- `mood.js` con `fn_mood_recalcular()` funcional
- `secrets.js` con `fn_secreto_descubrir()` y `fn_secreto_conoce()` funcionales
- `jealousy.js` con celos y armonía funcionales
- `memory.js` con registro y consulta funcionales
- Hooks de save verificados: guardar y cargar sin pérdida de datos
- ESTADO_ACTUAL_CORE.md documenta qué funciones existen y cómo llamarlas

Dev Core NO entrega si:
- Alguna función falla silenciosamente sin log en DEBUG_MODE
- Los saves corrompan datos al cargar
- Hay variables declaradas en código que no están en VARIABLES_GLOBALES.md

### Dev Arte → Dev Contenido

Dev Arte entrega cuando:
- Celine outfit1 completo: las 7 expresiones definidas en GDD_PERSONAJES.md
- Roxy outfit1 completo: las 7 expresiones definidas en GDD_PERSONAJES.md
- Luna outfit1 completo: las 7 expresiones definidas en GDD_PERSONAJES.md
- Fondos del apartamento completos: sala, cocina, balcón en las 4 franjas (12 fondos mínimo)
- Todos los archivos con naming correcto y en su carpeta correcta
- ESTADO_ACTUAL_ARTE.md con lista de assets entregados y pendientes

Dev Arte NO entrega assets sueltos sin las 7 expresiones de ese outfit.
Entregar 3 expresiones de Celine y después 4 más crea inconsistencias en el editor.

### Dev Contenido → iteración siguiente

Dev Contenido cierra una iteración cuando:
- El día 1 completo es navegable sin errores
- Al menos un acto narrativo (el Acto 1 de cualquier personaje) está implementado y funciona
- La pantalla de mapa del apartamento permite navegar entre locaciones
- El HUD muestra stats, tiempo y dinero correctamente
- ESTADO_ACTUAL_CONTENIDO.md documenta exactamente qué eventos están implementados

### Cuándo llamar al Dev Debugger

Solo si se cumplen las dos condiciones siguientes:
1. Hay un bug reproducible (sabes exactamente qué pasos lo causan)
2. El dev responsable del archivo ya intentó resolverlo y no pudo

El Dev Debugger necesita recibir:
- El ESTADO_ACTUAL de todos los devs
- Descripción exacta del bug y pasos para reproducirlo
- El archivo y función donde ocurre el error (si se sabe)
- El error exacto de consola (si hay)

**El Dev Debugger no trabaja con "algo no funciona bien" — trabaja con bugs concretos.**

---

## 10. COSAS PROHIBIDAS — LISTA DEFINITIVA

Las siguientes acciones están prohibidas en cualquier circunstancia.
Si ves que alguna de estas está a punto de suceder, para y consulta al Arquitecto.

| Prohibido | Por qué | Consecuencia |
|-----------|---------|-------------|
| Editar archivos en `js/plugins/` | Rompe compatibilidad con el engine y los plugins | Bug irreproducible que tarda horas en encontrar |
| Editar archivos en `data/` a mano | El engine de MZ espera un formato JSON exacto | Corrupción del proyecto que puede no recuperarse |
| Declarar variable en código sin estar en VARIABLES_GLOBALES.md | El Dev Debugger no puede rastrear variables que no están documentadas | Estado del juego incoherente |
| Modificar `AptSim.[stat]` directamente desde un evento | Bypasea validaciones de rango y requisitos | Personajes con stats fuera de rango, bugs de contenido |
| Usar `Array.push()` en arrays de `AptSim` | Bug en el sistema de saves por referencias | Saves corruptos que el jugador no puede recuperar |
| Usar `var` para variables locales en funciones nuevas | Scope problemático, colisiones con el engine | Bugs difíciles de rastrear |
| Commit sin actualizar ESTADO_ACTUAL | El próximo dev no sabe en qué estado está el proyecto | Trabajo duplicado, conflictos |
| Trabajar en el archivo de otro dev sin avisar | Conflictos de merge, trabajo perdido | Pérdida de código |
| Inventar variables que no están en VARIABLES_GLOBALES.md | Sistema de saves no las guarda, no se rastrean | Pérdida de progreso del jugador |
| Empaquetar la carpeta `_dev/` en build | Expone notas internas de desarrollo | No es un bug pero es un error grave |
| Dejar `AptSim.DEBUG_MODE = true` en commit | Logs en consola del jugador final | Apariencia no profesional |
| Usar tildes en valores internos de variables | Las comparaciones de strings fallan silenciosamente | Lógica de rutinas y moods rota |

---

## 11. DOCUMENTACIÓN DE FUENTES OFICIALES

Antes de implementar cualquier funcionalidad que use el engine de MZ,
consultar la documentación oficial de la versión 1.8.0:

- **API oficial MZ:** https://rpgmakermz-api.rmmzgame.com/
- **Blog oficial:** https://www.rpgmakerweb.com/blog/category/rpg-maker-mz
- **Foro oficial:** https://forums.rpgmakerweb.com/

**Regla:** Si la documentación oficial dice algo diferente a lo que recuerdas
de versiones anteriores o de otros proyectos, la documentación oficial gana.
RPG Maker MZ difiere de MV en varios comportamientos críticos.

**Funcionalidades que requieren consulta previa a la documentación antes de implementar:**
- Show Name Input Processing (pantalla de nombre del protagonista)
- Picture system (para sprites de personajes si no se usa el sistema de mensajes)
- Plugin Command (si se conecta algún plugin de terceros)
- Any Save/Load hook que no esté ya en `vars.js`

---

## 12. GLOSARIO — TÉRMINOS QUE USAMOS DE FORMA ESPECÍFICA

| Término | Significado en este proyecto |
|---------|------------------------------|
| **Acto** | Uno de los 5 eventos narrativos principales de un personaje |
| **Quest** | Uno de los 3 mini-arcos de historia de un personaje |
| **Franja** | Uno de los 4 bloques de tiempo del día |
| **Mood** | Estado de ánimo calculado del personaje, recalculado por franja |
| **Corrupción** | Apertura a escalar la relación — no connotación negativa de personaje |
| **Armonía** | Salud del ambiente del apartamento (variable global) |
| **Celos** | Sistema de reacción cuando hay diferencia de atención entre personajes |
| **Secreto** | Información sobre un personaje que se descubre explorando |
| **Flag** | Variable booleana que registra si algo ocurrió (nunca vuelve a false) |
| **Delta** | El valor que se suma o resta a una variable (puede ser negativo) |
| **Handoff** | Momento en que un dev termina su fase y el siguiente puede empezar |
| **Script Call** | Comando del editor de MZ que ejecuta código JavaScript |
| **Clamping** | Limitar un valor a su rango válido (0-100) con Math.min/Math.max |

---

*Documento generado por el Arquitecto — Apartment Sim v1.0 RPG Maker MZ*
*Próximo documento: Prompt Dev Core*
