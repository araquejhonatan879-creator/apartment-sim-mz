# APARTMENT SIM — GDD SISTEMAS
### Versión 1.0 — RPG Maker MZ | Generado: 07/06/2026

---

## INSTRUCCIÓN PARA DEVS

Este documento define TODOS los sistemas del juego con números exactos.
Ningún número puede ser inventado en código — si no está aquí, no se implementa
hasta que el Arquitecto lo apruebe y lo agregue.

**Dev Core:** Lee S01 al S05, S08, S09, S13, S19, S20 antes de escribir una línea.
**Dev Contenido:** Lee S06, S07, S10 al S18, S21 para implementar eventos y pantallas.
**Dev Arte:** Lee S11 para el sistema de outfits.
**Dev Debugger:** Lee todo antes de tocar cualquier archivo.

Si un sistema necesita un número que no aparece aquí, **para y consulta al Arquitecto**.

---

## S01 — STATS POR PERSONAJE

Tres barras independientes por personaje. Rango 0-100 cada una.

### Definiciones
**Afecto:** Qué tanto le importas. Sube con tiempo compartido, conversaciones,
regalos, quests completadas. Baja si la ignoras varios días seguidos o tomas
decisiones que la lastiman.

**Confianza:** Qué tanto cree en ti. Sube siendo consistente, cumpliendo promesas,
guardando secretos. Baja si mientes, fallas, o usas información en su contra.
Es la barra más difícil de recuperar.

**Corrupción:** Qué tan abierta está a escalar la relación físicamente.
Requiere mínimos de Afecto y Confianza para poder subir. No sube sola —
requiere acciones específicas del jugador.

### Requisitos mínimos para subir Corrupción
| Rango destino | Afecto mínimo | Confianza mínima |
|--------------|---------------|-----------------|
| 0 → 20 | 15 | 10 |
| 20 → 45 | 35 | 25 |
| 45 → 70 | 55 | 45 |
| 70 → 90 | 75 | 60 |
| 90 → 100 | 90 | 75 |

**Implementación en MZ:** Antes de aplicar cualquier delta de Corrupción,
la función JS debe verificar que Afecto y Confianza actuales cumplan el mínimo
del rango destino. Si no se cumplen, el delta de Corrupción se ignora silenciosamente
(no se muestra error al jugador).

### Velocidad de cambio base
| Acción | Afecto | Confianza | Corrupción |
|--------|--------|-----------|-----------|
| Conversación cotidiana exitosa | +2 | +0 | +0 |
| Conversación profunda | +5 | +3 | +0 |
| Regalo pequeño ($20-40) | +3 | +0 | +0 |
| Regalo significativo ($80+) | +8 | +2 | +0 |
| Salida completada | +10 a +15 | +5 a +8 | +0 |
| Quest completada | +15 a +20 | +10 a +15 | +0 |
| Evento narrativo completado | +20 | +10 | +5 a +10 |
| Ignorar un día | -1 | +0 | +0 |
| Ignorar 3 días seguidos | -5 | -2 | +0 |
| Mentira descubierta | -5 | -15 | +0 |
| Usar secreto en contra | -10 | -25 | +0 |

### Efecto del arquetipo dominante sobre Afecto ganado
- Arquetipo del jugador coincide con preferencia del personaje: ×1.2 a todos los deltas de Afecto
- Arquetipo opuesto: ×0.9 (reduce levemente, no bloquea)
- Se recalcula cada semana según elecciones recientes

### Stats iniciales
| Personaje | Afecto | Confianza | Corrupción |
|-----------|--------|-----------|-----------|
| Celine | 15 | 10 | 5 |
| Roxy | 25 | 15 | 15 |
| Luna | 20 | 10 | 10 |

---

## S02 — STATS DEL PROTAGONISTA

### Encanto (0-100)
Mejora opciones de diálogo disponibles y su efectividad.

| Rango | Efecto |
|-------|--------|
| 0-25 | Diálogos básicos. Sin opciones especiales. |
| 26-50 | +1 opción de diálogo en conversaciones importantes |
| 51-75 | +2 opciones. Probabilidad de eventos espontáneos positivos +10% |
| 76-100 | +3 opciones. Sistema voyeur sube a 85% éxito. Algunas inician conversación primero. |

**Cómo sube Encanto:**
- Leer un libro: +2 (máximo 1 vez por libro, no acumulable)
- Conversación exitosa: +1
- Quest con resultado óptimo: +3 a +5
- Salida completada: +2

### Energía máxima (6-10)
Acciones disponibles por día. Se restaura al inicio de cada día.

**Cómo sube:**
- Ejercicio en franja mañana durante 5 días consecutivos = +1 nivel
- Máximo alcanzable: 10
- Si se interrumpe el ejercicio, el contador de días consecutivos se resetea a 0

### Dinero
- Sin máximo
- Inicial: $500
- Fuentes: trabajo ($50-150 aleatorio), quests, eventos especiales

---

## S03 — PERSONALIDAD DEL JUGADOR

Tres arquetipos acumulativos. El dominante es el que tiene mayor valor.
Se recalcula cada 7 días de juego comparando elecciones de la semana.

| Arquetipo | Qué elecciones lo suben | Personaje que lo prefiere |
|-----------|------------------------|--------------------------|
| Romántico | Gestos dulces, escuchar activamente, recordar detalles, paciencia | Luna |
| Atrevido | Directo, toma iniciativa, propone cosas, sin esperar permiso | Roxy |
| Respetuoso | Nunca presiona, espera señales claras, pide antes de actuar | Celine |

**Efecto del arquetipo dominante:**
- Coincide con preferencia del personaje: +20% a todos los puntos de Afecto ganados
- Es el opuesto: -10% (no bloquea, solo reduce)
- Sin arquetipo dominante claro (empate): sin modificador

**Implementación en MZ:** Variables `prot_romantico`, `prot_atrevido`, `prot_respetuoso`
se incrementan desde los eventos con la función correspondiente en JS.
El arquetipo dominante se guarda en `prot_arquetipo_dominante` como string.

---

## S04 — SISTEMA DE DÍAS Y TIEMPO

### Franjas horarias
| Franja | Variable interna | Horas |
|--------|-----------------|-------|
| Mañana | `"manana"` | 6am - 12pm |
| Tarde | `"tarde"` | 12pm - 6pm |
| Noche | `"noche"` | 6pm - 12am |
| Madrugada | `"madrugada"` | 12am - 6am |

Ciclo: `manana` → `tarde` → `noche` → `madrugada` → (nuevo día) → `manana`

**Regla de encoding:** Variables internas NUNCA llevan tildes. La UI muestra
texto con tilde al jugador. Las comparaciones en código usan siempre sin tilde.

### Días
- Día 1 = lunes
- El juego no tiene fin — no hay día máximo
- Fin de semana = sábado y domingo
- En fin de semana: disponibilidad aumentada, probabilidad de eventos especiales +25%

### Energía por franja
- Avanzar una franja consume 1 punto de energía del protagonista
- Al iniciar un nuevo día (madrugada → mañana): energía se restaura al máximo
- Si energía llega a 0: el jugador puede seguir navegando pero no puede iniciar
  interacciones que cuesten energía

### Rutinas base por personaje
Ver GDD_PERSONAJES.md para detalle completo.
Resumen para implementación JS:

| Personaje | Mañana semana | Tarde semana | Noche semana | Madrugada semana |
|-----------|--------------|-------------|-------------|-----------------|
| Celine | Cocina / cuarto | Fuera (universidad) | Sala / cuarto | Cuarto (baja guardia) |
| Roxy | Cocina / sala | Variable 50% | Sala / cuarto | Sala |
| Luna | Cocina (no interrumpir) | Sala / cuarto | Sala / balcón | Variable |

---

## S05 — ECONOMÍA Y REGALOS

### Trabajo
- 1 acción = pago aleatorio con distribución normal centrada en $90
- Rango real: $50 a $150
- Límite: máximo 2 turnos de trabajo por franja horaria
- No se puede trabajar en madrugada
- Los turnos se resetean al cambiar de franja

### Catálogo de regalos
| ID | Regalo | Precio | Afecto base | Confianza base | Notas especiales |
|----|--------|--------|-------------|----------------|-----------------|
| `cafe` | Café favorito | $20 | +3 | +0 | Luna: +0 (toma té, no café) |
| `flores` | Flores | $40 | +5 | +0 | Luna: +8 (las prefiere) |
| `libro` | Libro específico | $80 | +8 | +3 | Sin condición de Afecto |
| `ropa` | Ropa | $120 | +5 | +0 | Solo aplica si Afecto >30. Si no: +0 |
| `joya` | Joya | $200 | +15 | +0 | Solo aplica si Afecto >50. Si no: +0 |
| `secreto` | Regalo secreto | $100 | +10 | +10 | Solo disponible si secreto correspondiente descubierto |

**Café favorito por personaje (hay que descubrirlo interactuando):**
- Celine: Americano negro sin azúcar
- Roxy: Matcha latte con mucho azúcar
- Luna: Té earl grey — si le compras café sube +0

**Regalo secreto por personaje:**
- Celine: requiere `sec_celine_1` descubierto → +10 Afecto +10 Confianza
- Roxy: requiere `sec_roxy_1` descubierto → +10 Afecto +5 Confianza
- Luna: requiere `sec_luna_1` descubierto → +10 Afecto +10 Confianza

### Catálogo de ítems (jugador, no regalos)
| ID | Ítem | Precio | Efecto |
|----|------|--------|--------|
| `camara` | Cámara compacta | $150 | Desbloquea sistema voyeur (S15) y fotos (S16) |

### Salidas fuera del apartamento
| Tipo | Afecto req. | Costo | Afecto ganado | Confianza ganada |
|------|-------------|-------|---------------|-----------------|
| Café | 25 | $30 | +8 | +0 |
| Cine | 40 | $50 | +10 | +5 |
| Compras | 50 | $80 | +8 | +0 + outfit desbloqueado |
| Cena | 60 | $100 | +12 | +8 |
| Playa/Parque | 70 | $60 | +10 | +0 y +10 Corrupción |
| Noche fuera | 85 | $150 | Evento especial único por personaje | — |

---

## S06 — EVENTOS NARRATIVOS

5 eventos únicos por personaje. Se desbloquean por nivel de Afecto.
Detalle completo de cada acto en GDD_PERSONAJES.md.

| Acto | Trigger Afecto | Personaje |
|------|---------------|-----------|
| Acto 1 | ≥20 | Celine / Roxy / Luna |
| Acto 2 | ≥40 | Celine / Roxy / Luna |
| Acto 3 | ≥60 | Celine / Roxy / Luna |
| Acto 4 | ≥80 | Celine / Roxy / Luna |
| Acto 5 | ≥100 + Corrupción y Confianza adicionales | Celine / Roxy / Luna |

**Condiciones adicionales Acto 5:**
- Celine: Afecto ≥100 + Corrupción >70 + Confianza >65
- Roxy: Afecto ≥100 + Corrupción >75 + Confianza >60
- Luna: Afecto ≥100 + Corrupción >65 + Confianza >75

**Estructura de cada evento:**
1. Trigger (condición verificada por el sistema al avanzar franja)
2. Escena con diálogos
3. Decisión del jugador (3 opciones vinculadas a arquetipos)
4. Consecuencia inmediata (deltas de stats)
5. Consecuencia a largo plazo (flags que afectan eventos futuros)

---

## S07 — INTERACCIONES LIBRES SANDBOX

Disponibles una vez desbloqueadas. Se accede desde el mapa cuando el personaje
está en locación compatible y el jugador tiene Corrupción suficiente.

### Por nivel de Corrupción

**Nivel 1 (Corrupción ≥20):**
| Interacción | Locación | Req. adicional | Afecto | Confianza |
|-------------|----------|---------------|--------|-----------|
| Abrazo | Cualquier lugar | Afecto >25 | +2 | +0 |
| Masaje de hombros | Sala / cuarto | — | +3 | +0 |
| Ver película juntos | Sala, noche | — | +4 | +1 |

**Nivel 2 (Corrupción ≥40):**
| Interacción | Locación | Req. adicional | Afecto | Confianza | Corrupción |
|-------------|----------|---------------|--------|-----------|-----------|
| Beso | Solos | — | +5 | +0 | +5 |
| Dormir en el mismo cuarto | Noche | Afecto >60 o ella propone | +6 | +0 | +0 |
| Bailar | Sala, noche | — | +5 | +3 | +0 |

**Nivel 3 (Corrupción ≥60):**
| Interacción | Locación | Req. adicional | Efectos |
|-------------|----------|---------------|---------|
| Escena íntima nivel 1 | Cuarto, noche/madrugada | — | Guardada en galería |
| Ducha compartida | Baño | Probabilidad base 40% | — |
| Sesión de fotos | Cualquier | Cámara comprada | Fotos en colección |

**Nivel 4 (Corrupción ≥80):**
| Interacción | Req. adicional | Efectos |
|-------------|---------------|---------|
| Escena íntima nivel 2 | — | Variantes según personalidad |
| Proponer outfit | Confianza >70 | Acepta 60% base (sube con Afecto) |
| Iniciativa nocturna (ir a su cuarto) | — | Puede aceptar o rechazar según mood |

**Nivel 5 (Corrupción ≥95):**
| Interacción | Req. adicional | Efectos |
|-------------|---------------|---------|
| Escena íntima nivel 3 | — | Variante final por personaje |
| Interacciones grupales | Armonía >60 + Corrupción >70 en 2+ personajes | — |

### Iniciativa inversa
Si Corrupción >60 y Afecto >70 y mood == "traviesa": probabilidad 10% de que
el personaje proponga una interacción libre por su cuenta.
- Celine: indirecta — *"si quisieras... podría ser..."* seguido de silencio
- Roxy: directo, sin rodeos, con un chiste por encima
- Luna: aparece en tu cuarto sin decir nada y espera

---

## S08 — ESTADO DE ÁNIMO DIARIO (MOOD)

Se recalcula al avanzar cada franja. Depende de stats actuales, franja y fin de semana.

### Tabla de moods
| Mood | Probabilidad base | Efecto en Afecto ganado | Efecto especial |
|------|------------------|------------------------|-----------------|
| `normal` | 40% | ×1.0 | Ninguno |
| `feliz` | 20% | ×1.5 | Confianza ganada también ×1.3 |
| `cansada` | 15% | ×0.7 | Rechaza interacciones físicas de nivel 1-2 |
| `estresada` | 15% | ×1.3 positivas / ×0.3 negativas | Si la presionas: -10 Afecto extra |
| `traviesa` | 10% | ×1.2 | Puede proponer interacción libre si Corrupción >60 |

### Algoritmo de cálculo (prioridad descendente)
1. **traviesa:** Afecto ≥60 + Confianza ≥50 + (franja == noche/madrugada O es finde)
2. **feliz:** Afecto ≥50 + (es finde O franja == tarde/noche) O Afecto ≥65 (cualquier franja)
3. **estresada:** Confianza ≤15 + Afecto ≤30
4. **cansada:** franja == madrugada O (franja == manana + Afecto ≤25)
5. **normal:** todo lo demás

### Cómo saber el mood
- Observable visualmente por el sprite del personaje
- Los mensajes del día lo insinúan
- Con Encanto >50: el jugador puede "leer" el mood al llegar a la sala

---

## S09 — SECRETOS DESCUBRIBLES

Cada personaje tiene 3 secretos. Se descubren explorando en el lugar correcto
a la hora correcta con los stats correctos.

### Tabla completa de secretos
| ID | Personaje | Secreto | Condición | Efecto descubrir |
|----|-----------|---------|-----------|-----------------|
| `sec_celine_1` | Celine | Diario con entradas sobre el protagonista | Cuarto Celine + Afecto >35 | +10 Confianza |
| `sec_celine_2` | Celine | Roxy sabe algo que no debería | Pasillo + Tarde + Día >7 | +10 Confianza |
| `sec_celine_3` | Celine | Llora sola de madrugada | Pasillo + Madrugada + Día >14 | +10 Confianza |
| `sec_roxy_1` | Roxy | Bocetos del protagonista en sketchbook | Sala (sketchbook visible, sin req. Afecto) | +10 Confianza |
| `sec_roxy_2` | Roxy | Fotos guardadas de los dos | Cuarto Roxy + Afecto >40 | +10 Confianza |
| `sec_roxy_3` | Roxy | Le habla a Luna del protagonista | Escuchar conversación + Noche + Día >7 | +10 Confianza |
| `sec_luna_1` | Luna | Página del cuaderno con nombre del protagonista | Cuaderno olvidado — evento aleatorio | +10 Confianza |
| `sec_luna_2` | Luna | Cartas que nunca envió | Cuarto Luna + Madrugada + Afecto >45 | +10 Confianza |
| `sec_luna_3` | Luna | Llorando en cocina a las 3am | Cocina + Madrugada + Día >14 | +10 Confianza |

**Regla:** Una vez descubierto un secreto, el flag correspondiente es `true` para siempre.

### Usos del secreto
**Usar a favor (opción positiva):**
- +15 Afecto +10 Confianza adicionales
- Diálogo único de vulnerabilidad

**Usar en contra:**
- -20 Confianza permanente
- -10 Afecto permanente
- Bloquea 1 quest relacionada con ese personaje
- El personaje lo recuerda — menciones negativas en diálogos futuros

---

## S10 — EVENTOS ESPONTÁNEOS

Probabilidad base: 15% por franja de que ocurra uno.
No se repite el mismo evento en la misma semana de juego.

| Evento | Franja | Condición | Efecto posible |
|--------|--------|-----------|---------------|
| Puerta del baño sin seguro | Mañana / Noche | Aleatorio | Escena voyeur sin cámara (+5 Corrupción, no se guarda) |
| Apagón | Noche | Semana 1+ | Conversación forzada en oscuridad (+5 Afecto +3 Confianza) |
| Lluvia intensa | Tarde / Noche | Cualquier semana | Todas en casa, ambiente íntimo (+3 Afecto a todas) |
| Se rompe algo | Mañana / Tarde | Cualquier semana | Si lo arreglas: +5 Confianza con la afectada |
| Te encuentra durmiendo en el sofá | Mañana | Madrugada previa activa | Reacción según personaje y Afecto actual |
| Pesadilla (ella) | Madrugada | Semana 2+ | Si vas: +8 Afecto +5 Confianza |
| Cocinar juntos sin planear | Mañana | Fin de semana | Mini-juego simple / +6 Afecto |
| Encuentras su ropa en secadora | Mañana | Cualquier semana | Decisión moral / consecuencias variables |

---

## S11 — SISTEMA DE ROPA / OUTFITS

4 outfits por personaje. Se desbloquean por nivel de Corrupción.

| Outfit | Corrupción req. | Descripción | Disponibilidad |
|--------|-----------------|-------------|---------------|
| `outfit1` (base) | 0 | Ropa de diario. Siempre. | Siempre |
| `outfit2` | 25 | Más casual / ropa de casa relajada | Siempre en casa |
| `outfit3` | 50 | Más revelador | Solo noche / madrugada |
| `outfit4` | 80 | Máxima exposición | Solo en contexto de interacción libre |

**Sugerir outfit:**
Con Corrupción >75 y Confianza >70: el jugador puede sugerirle qué ponerse.
Acepta con probabilidad base 60% (sube con Afecto: +1% por punto de Afecto sobre 70).

**Implementación en MZ:** El nombre del sprite incluye el outfit.
Formato: `[personaje]_[outfit]_[expresion]`
Ejemplo: `celine_outfit2_feliz`, `roxy_outfit3_traviesa`

---

## S12 — SALIDAS FUERA DEL APARTAMENTO

Ver tabla en S05 para costos y requisitos.

**Salidas grupales:**
- Invitar a dos personajes con Afecto >50 en ambas activa evento grupal especial
- La tercera que no fue se entera: -5 Armonía + celos activos en ella
- Requiere que ambas invitadas tengan Armonía >40 entre sí para aceptar

---

## S13 — MEMORIA DE DECISIONES

El juego almacena hasta 20 decisiones importantes.
Se mencionan orgánicamente en diálogos días o semanas después.

### Tipos de decisiones recordadas
- Promesas hechas (y si se cumplieron)
- Momentos de crueldad o indiferencia
- Momentos de apoyo inesperado
- Secretos descubiertos y cómo se usaron
- Elecciones en actos narrativos

### Cómo aparece en juego
- Diálogos cotidianos: *"Eso que hiciste el martes..."*
- Momentos de tensión: referencia directa
- Eventos narrativos: cambia opciones disponibles

### Implementación en MZ
Variable `mem_decisiones` = array de strings con IDs de decisiones.
Máximo 20 entradas — si se llena, la más antigua se elimina.
Verificación con función `fn_mem_tiene(id)` que retorna bool.
**Regla de listas en MZ:** Siempre reasignar el array completo, no mutar.
`mem_decisiones = [...mem_decisiones, nuevo_id]`

---

## S14 — GALERÍA DE RECUERDOS

Accesible desde el teléfono del protagonista en cualquier momento.

### Contenido
- Escenas narrativas desbloqueadas (automático al completarlas)
- Interacciones libres marcadas como favoritas (el jugador elige)
- Fotos coleccionables del sistema S16
- Imágenes voyeur del sistema S15

### Filtros disponibles
- Por personaje: Celine / Roxy / Luna
- Por tipo: escena narrativa / interacción libre / foto / voyeur
- Por semana del juego

### Flags de galería
Formato: `gal_[personaje]_[tipo]_[numero]` = bool
Ejemplos: `gal_celine_acto_1`, `gal_roxy_voyeur_2`, `gal_luna_foto_3`
Todos inician en `false`. Una vez `true`, nunca vuelven a `false`.

---

## S15 — SISTEMA VOYEUR

**Requisito:** Ítem `camara` comprado ($150).
**Disponibilidad:** Solo noche y madrugada.
**Probabilidad de éxito base:** 70%
**Con Encanto >75:** sube a 85%

### Si te pillan
- -25 Confianza con ese personaje
- Bloqueado del sistema voyeur con ella esa semana
- Diálogo de confrontación (tono varía según Afecto actual)

### Si tienes éxito
- Escena desbloqueada y guardada en galería
- +15 Corrupción con ese personaje

### Escenas por personaje (3 por personaje)
| Escena | Corrupción previa req. | Descripción general |
|--------|----------------------|---------------------|
| Voyeur 1 | 20 | Situación cotidiana íntima (cambiarse, ducharse) |
| Voyeur 2 | 50 | Situación más comprometedora |
| Voyeur 3 | 75 | Escena explícita |

---

## S16 — FOTOS COLECCIONABLES

En momentos específicos aparece un ícono de cámara en pantalla.
El jugador tiene 3-5 segundos para hacer clic. Si lo hace: foto guardada en galería.

**Con Corrupción >60:** puedes pedirle que pose.
Acepta con probabilidad base 50%.

**Cantidad:** 8 a 10 fotos por personaje, distribuidas a lo largo del juego.

**Implementación en MZ:** Temporizador con variable de control.
Si el jugador activa el evento de foto en el tiempo: flag `gal_[personaje]_foto_[n]` = true.

---

## S17 — MENSAJES DE TEXTO

Accesible desde el teléfono en cualquier momento.
Cada personaje envía mensajes espontáneos según Afecto y estado del día.

### Respuestas
| Acción | Efecto |
|--------|--------|
| Responder bien | +2 Afecto |
| Ignorar | -1 Afecto por día ignorado |
| Responder mal | -3 Afecto |

### Frecuencia de mensajes espontáneos
| Afecto | Mensajes por día |
|--------|-----------------|
| 0-30 | 0 a 1 |
| 31-60 | 1 a 2 |
| 61-100 | 2 a 4 |

---

## S18 — QUESTS POR PERSONAJE

3 quests por personaje. 9 en total. Detalle completo en GDD_PERSONAJES.md.

### Estructura de toda quest
1. **Trigger:** Condición de Afecto / Confianza / flag
2. **Evento / escena:** Lo que pasa
3. **Decisión del jugador:** Mínimo 2 opciones, máximo 3
4. **Consecuencia inmediata:** Deltas de stats
5. **Consecuencia a largo plazo:** Flag persistente que afecta eventos futuros

### Lista de quests
| Personaje | Quest | Trigger |
|-----------|-------|---------|
| Celine | "Los apuntes" | Confianza ≥30 |
| Celine | "La defensa" | Afecto ≥50 |
| Celine | "El diario" | `sec_celine_1` descubierto |
| Roxy | "El sketchbook" | Afecto ≥25 |
| Roxy | "La exposición" | Afecto ≥55 |
| Roxy | "La noche que llora" | Confianza ≥60 |
| Luna | "El libro" | Afecto ≥20 |
| Luna | "Tres días de silencio" | Confianza ≥40 |
| Luna | "El mismo libro" | Afecto ≥70 |

---

## S19 — ARMONÍA DEL APARTAMENTO

Variable global `apt_armonia` (0-100). Inicial: 70.

| Nivel | Efecto |
|-------|--------|
| >70 | Eventos grupales frecuentes. Ambiente cálido. Las tres interactúan naturalmente. |
| 40-70 | Normal. Sin efectos especiales. |
| <40 | Algunas se evitan entre sí. Menos eventos disponibles. Diálogos más tensos. |
| <20 | Algunas se niegan a estar en el mismo cuarto. Bloquea eventos grupales. |
| 0 | Modo conflicto global — todas hostiles entre sí. |

### Qué sube Armonía
| Acción | Puntos |
|--------|--------|
| Evento grupal positivo | +5 a +10 |
| Resolver conflicto entre ellas | +10 a +15 |
| Cumpleaños exitoso | +10 |

### Qué baja Armonía
| Acción | Puntos |
|--------|--------|
| Celos activos en alguien | -3 por día |
| Conflicto sin resolver | -5 |
| Elección que favorece a una y perjudica a otra | -5 a -10 |

---

## S20 — CELOS Y DINÁMICAS

El sistema de celos se activa cuando hay diferencia grande de atención entre personajes.

### Umbral de activación
- Diferencia de Afecto >25 entre dos personajes
- O evento narrativo con otra presenciado por una tercera

### Intensidades
| Intensidad | Diferencia de Afecto respecto al promedio | Impacto en Armonía por día |
|-----------|------------------------------------------|---------------------------|
| 0 (ninguno) | <10 | 0 |
| 1 (leve) | 10-19 | -2 |
| 2 (moderado) | 20-34 | -5 |
| 3 (intenso) | ≥35 | -10 |

### Cómo expresa los celos cada personaje
| Personaje | Celos hacia | Expresión |
|-----------|-------------|-----------|
| Celine | Roxy | Más fría y distante. Más sarcástica. |
| Celine | Luna | Silencio activo. Deja de buscar al jugador. |
| Roxy | Celine | Comentarios ácidos con humor. Lo dice como chiste pero no es chiste. |
| Roxy | Luna | Pregunta más, escucha menos. Busca más atención. |
| Luna | Cualquiera | Desaparece. Afecto baja 1 punto por día hasta que el jugador la busque. |

### Resolver celos
Dedicar 3 días de acciones principalmente a la personaje celosa baja el nivel de celos a 0.
Los celos se resetean al inicio de cada día — la intensidad puede volver a subir
si la causa no se resuelve.

---

## S21 — MINI-JUEGOS

### Strip 21 — Blackjack 1v1

**Activación:**
- El jugador lo propone: disponible si Corrupción >30
- El personaje lo propone: Corrupción >30 + mood == `"traviesa"`

**Reglas:**
- Objetivo: llegar a 21 sin pasarse
- Cartas 1-10 + figuras J/Q/K (valen 10). As: vale 11, reduce a 1 si se pasa de 21.
- Sin dealer — es 1v1
- IA del personaje: se planta si tiene 17 o más (regla clásica)
- Empate: ronda no cuenta, se reparte de nuevo

**Si el jugador pierde:**
| Opción | Efecto |
|--------|--------|
| Perder una prenda | +5 Corrupción (en el personaje) |
| Hacer un reto | +5 Afecto +3 Corrupción |

**Si el jugador gana:**
| Opción que elige para ella | Efecto |
|---------------------------|--------|
| Perder una prenda | +5 Corrupción |
| Hacer un reto | +5 Afecto +3 Corrupción |

**Si rechaza el reto:** -5 Confianza

**Implementación en MZ:**
- Lógica de cartas: función JS `fn_mini_generar_carta()` y `fn_mini_valor_mano()`
- Flujo del juego: sistema de mensajes y opciones del editor de MZ
- Variables temporales de partida: `mini_mano_jugador`, `mini_mano_rival`, `mini_total_jugador`, `mini_total_rival`
- Dev Contenido debe revisar la documentación de MZ sobre Control Variables y Show Choices
  antes de implementar. No improvisar la estructura.

---

### Verdad o Reto

**Activación:**
- El jugador lo propone: disponible si Corrupción >20 y Afecto >30
- El personaje lo propone: mismas condiciones + mood == `"traviesa"`

**Niveles de Corrupción para el banco de preguntas:**
| Nivel | Rango | Tipo de contenido |
|-------|-------|------------------|
| `bajo` | 0-30 | Preguntas inocentes / retos tontos |
| `medio` | 31-60 | Preguntas personales / retos con contacto leve |
| `alto` | 61-85 | Preguntas íntimas / retos explícitos |
| `maximo` | 86-100 | Sin filtros |

**Efectos:**
| Acción | Efecto |
|--------|--------|
| Responder verdad honesta | +8 Confianza |
| Rechazar verdad | -3 Confianza |
| Completar reto | +5 Afecto +3 Corrupción |
| Rechazar reto | -5 Confianza |

**Banco de preguntas:** Mínimo 7 por nivel por tipo (verdad / reto).
El banco completo está en el código del Dev Contenido — no se documenta aquí
para evitar spoilers al desarrollar, pero debe estar en el archivo JS del sistema.

**Implementación en MZ:**
- Selección aleatoria del banco: función JS `fn_mini_vot_obtener_pregunta(personaje, nivel)`
- Flujo del turno: sistema de mensajes y opciones del editor de MZ
- Turnos: impar = jugador elige para ella / par = ella elige para el jugador

---

*Documento generado por el Arquitecto — Apartment Sim v1.0 RPG Maker MZ*
*Siguiente documento: VARIABLES_GLOBALES.md*
