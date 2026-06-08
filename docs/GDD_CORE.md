# APARTMENT SIM — GDD CORE
### Versión 1.0 — RPG Maker MZ | Generado: 07/06/2026

---

## INSTRUCCIÓN PARA DEVS

Antes de leer cualquier otro documento, lee este.
Este es el documento raíz. Todo lo demás deriva de aquí.
Si algo en otro documento contradice este, este gana.
Si tienes dudas de diseño que este no resuelve, no improvises — consulta al Arquitecto.

---

## 1. VISIÓN DEL PROYECTO

**Nombre:** Apartment Sim
**Género:** Simulador de vida / Dating sim adulto con exploración libre
**Plataforma objetivo:** Windows (exportable a Mac/Linux)
**Audiencia:** Adultos 18+
**Idioma v1.0:** Español completo
**Idioma v1.1:** Inglés (distribución en itch.io y F95zone)
**Engine:** RPG Maker MZ versión 1.8.0
**Ciudad ficticia:** Vallanova — ciudad universitaria costera, tamaño mediano

### Concepto central
Simulador de vida en apartamento compartido con 3 compañeras en Vallanova.
Sin límite de tiempo. El jugador construye relaciones reales a través de tiempo,
gestos, decisiones y dinero. El contenido adulto se desbloquea de forma
progresiva y natural — nunca abrupta, nunca gratuita.

Las 3 historias se cruzan y se afectan entre sí. La dinámica central es única:
todos se conocen desde antes. No son extraños — son historia sin resolver.

### Referentes de diseño
| Juego | Qué tomamos |
|-------|-------------|
| Summer Memories | Sistema de días, rutinas, eventos por franja horaria |
| Teaching Feeling | Sandbox libre, interacciones en cualquier momento |
| Harem Hotel | Economía, quests por personaje, historias cruzadas |
| Milfy City | Stats del protagonista, mensajes, voyeur |
| Being a DIK | Personalidad del jugador que define al MC |
| Corrupted Kingdoms | Sistema de celos, stats trust/love/corruption |

---

## 2. STACK TÉCNICO

### Engine: RPG Maker MZ 1.8.0
- JavaScript como base — todos los sistemas custom van en JS
- Editor visual para mapas y eventos narrativos básicos
- Plugins para funcionalidades que el editor no cubre nativamente
- Documentación oficial: https://www.rpgmakerweb.com/blog/category/rpg-maker-mz

### Arte: Stable Diffusion Forge + LoRA
- SD Forge instalado en: `D:\Proyectos\stable-diffusion-forge`
- Cada personaje tiene su propio LoRA entrenado — garantiza consistencia visual
- Seed fija por personaje — misma cara en todas las expresiones y outfits
- Resolución de generación: 768x512 base, escalado a 1280x720 para el juego
- Edición final: GIMP o Krita
- Estilo objetivo: anime 2D semi-realista, líneas limpias, colores vibrantes
- Modelo base recomendado: Pony Diffusion v6 XL
- Detalle completo en GDD_ASSETS.md

### Resolución del juego
- **1280x720** (HD, 16:9)
- Configuración en `js/plugins/` al inicio del proyecto
- Todo el arte se genera y escala para esta resolución

### Software del equipo
- RPG Maker MZ 1.8.0
- Stable Diffusion Forge
- GIMP o Krita
- VS Code
- Git + GitHub

---

## 3. EL PROTAGONISTA

**Nombre:** Elegible al inicio (default: Kai)
**Edad:** 22
**Carrera:** Ingeniería de Sistemas (primer año)
**Visual:** Sin sprite propio en diálogos. Perspectiva primera persona.
En escenas grupales: silueta/sombra sin detalle.

### Backstory
Llegó a Vallanova para estudiar. Necesitaba cuarto urgente.
Su prima Celine tenía uno disponible en el apartamento que comparte con Roxy y Luna.
Las conoce a todas desde antes — el tiempo y la distancia cambiaron las cosas.
Lo que antes era simple ya no lo es.

### Relaciones de entrada
| Personaje | Vínculo | Tensión inicial |
|-----------|---------|----------------|
| Celine | Prima — historia de toda la vida | Algo pasó entre ellos. Ninguno lo nombra. Ella lo trata con distancia calculada. |
| Roxy | Amiga del bachillerato / ex-algo sin nombre | Hubo algo romántico que no terminó bien. Ella actúa como si nada. |
| Luna | Conocida del mismo grupo | Nunca fueron cercanos pero ella siempre lo observó. Ahora viven juntos. |

### Stats iniciales
- Encanto: 10 (rango 0-100)
- Energía máxima: 6 acciones por día (rango 6-10)
- Dinero: $500 (sin máximo)
- Personalidad: 0/0/0 (Romántico/Atrevido/Respetuoso)

---

## 4. LAS COMPAÑERAS

Descripción resumida. Detalle completo en GDD_PERSONAJES.md.

### Celine — La Fría
- Prima del protagonista. Hay historia sin resolver.
- Tsundere inteligente — sarcástica, controlada, perfeccionista.
- Color UI: `#818cf8`
- Afecto inicial: 15 | Confianza: 10 | Corrupción: 5

### Roxy — La Caótica
- Ex-algo del protagonista. Actúa como si todo está bien.
- Energética, artista, caótica. Directa cuando le conviene.
- Color UI: `#fb923c`
- Afecto inicial: 25 | Confianza: 15 | Corrupción: 15

### Luna — La Observadora
- Siempre lo miró diferente. Dice poco, pesa mucho.
- Silenciosa, enigmática. Lo que dice importa exactamente porque dice poco.
- Color UI: `#c084fc`
- Afecto inicial: 20 | Confianza: 10 | Corrupción: 10

---

## 5. ESTRUCTURA DE TIEMPO

### Franjas horarias (4 por día)
| Franja | Horas | Nombre interno (sin tilde) |
|--------|-------|--------------------------|
| Mañana | 6am - 12pm | `manana` |
| Tarde | 12pm - 6pm | `tarde` |
| Noche | 6pm - 12am | `noche` |
| Madrugada | 12am - 6am | `madrugada` |

**Regla de encoding:** Las variables internas nunca llevan tildes.
La UI muestra el texto con tilde al jugador. Las comparaciones en código usan
siempre la versión sin tilde.

### Días
- El juego empieza en día 1 (lunes)
- Semana = 7 días. Día 1 siempre es lunes.
- Fines de semana (sábado/domingo): disponibilidad aumentada, eventos especiales +25%
- No hay límite de días — el juego no termina

### Energía del protagonista
- Máximo base: 6 acciones por día
- Se restaura al inicio de cada día (al pasar de madrugada a mañana)
- Cada franja avanzada consume 1 punto de energía
- Sube a 7, 8, 9, 10 haciendo ejercicio 5 días consecutivos por nivel

---

## 6. SISTEMA DE STATS — RESUMEN

Detalle completo con números en GDD_SISTEMAS.md.

### Por personaje (Celine, Roxy, Luna)
- **Afecto (0-100):** Qué tanto le importas. Sube con tiempo, gestos, regalos, quests.
- **Confianza (0-100):** Qué tanto cree en ti. La más difícil de recuperar si baja.
- **Corrupción (0-100):** Apertura a escalar la relación. Requiere mínimos de Afecto
  y Confianza para poder subir. No sube sola.

### Del protagonista
- **Encanto (0-100):** Más opciones de diálogo. A >75 el sistema voyeur sube a 85% éxito.
- **Energía máxima (6-10):** Acciones disponibles por día.
- **Dinero:** Sin máximo. Empieza en $500.
- **Personalidad:** Romántico / Atrevido / Respetuoso. Se acumula con elecciones.

### Regla de oro de stats
Ningún stat se modifica directamente desde eventos.
Siempre a través de las funciones de sistema definidas en VARIABLES_GLOBALES.md.
Modificar un stat directamente es un error — el Dev Debugger lo rastreará.

---

## 7. GAME OVER

**No existe game over.**
Si Confianza llega a 0 con un personaje: se vuelve hostil/fría de forma permanente
pero sigue en el juego. El jugador puede intentar recuperar la relación —
es muy difícil pero no imposible.

Si Armonía del apartamento llega a 0: modo conflicto global.
Las tres se evitan entre sí. Bloquea eventos grupales. No es game over — es
consecuencia que el jugador tiene que resolver.

---

## 8. MAPA DEL APARTAMENTO

### Locaciones y requisitos de acceso
| Locación | Afecto para acceso | Disponibilidad |
|----------|-------------------|----------------|
| Sala / Cocina | 0 | Siempre |
| Balcón | 0 | Siempre (Luna aquí de noche frecuentemente) |
| Baño | Sin requisito | Evento espontáneo — probabilidad por franja |
| Cuarto Roxy | Afecto Roxy ≥ 20 | Cuando ella está ahí |
| Cuarto Celine | Afecto Celine ≥ 35 | Cuando ella lo permite |
| Cuarto Luna | Afecto Luna ≥ 45 | En momentos específicos de su rutina |

**Regla:** Una vez desbloqueado, el acceso es PERMANENTE.
No se necesita volver a ganarlo.

### Implementación en MZ
El mapa del apartamento es una pantalla de selección de locación —
no un mapa tile caminable. El jugador elige a dónde ir y el juego
lo transporta al evento de esa locación + franja.
Detalle de implementación en el prompt del Dev Contenido.

---

## 9. SISTEMAS DEL JUEGO

Lista de todos los sistemas. Números exactos en GDD_SISTEMAS.md.

| ID | Sistema | Dev responsable |
|----|---------|----------------|
| S01 | Stats por personaje | Dev Core |
| S02 | Stats del protagonista | Dev Core |
| S03 | Personalidad del jugador | Dev Core |
| S04 | Días y franjas horarias | Dev Core |
| S05 | Economía y regalos | Dev Core |
| S06 | Eventos narrativos (5 actos por personaje) | Dev Contenido |
| S07 | Interacciones libres sandbox | Dev Contenido |
| S08 | Estado de ánimo diario (mood) | Dev Core |
| S09 | Secretos descubribles (3 por personaje) | Dev Core + Dev Contenido |
| S10 | Eventos espontáneos | Dev Contenido |
| S11 | Sistema de ropa / outfits | Dev Arte + Dev Contenido |
| S12 | Salidas fuera del apartamento | Dev Contenido |
| S13 | Memoria de decisiones | Dev Core |
| S14 | Galería de recuerdos | Dev Contenido |
| S15 | Sistema voyeur | Dev Contenido |
| S16 | Fotos coleccionables | Dev Contenido |
| S17 | Mensajes de texto | Dev Contenido |
| S18 | Quests por personaje (3 por personaje) | Dev Contenido |
| S19 | Armonía del apartamento | Dev Core |
| S20 | Celos entre personajes | Dev Core |
| S21 | Mini-juegos (Strip 21 + Verdad o Reto) | Dev Contenido |

---

## 10. MINI-JUEGOS

Dos mini-juegos en v1.0. Expandibles si funcionan bien.

### Strip 21 (Blackjack 1v1)
- Se activa si Corrupción >30 y mood "traviesa", o el jugador lo propone
- Sin dealer — jugador vs personaje
- Perder prenda: +5 Corrupción | Reto completado: +5 Afecto +3 Corrupción
- Implementación en MZ: sistema de eventos con variables de juego
- La lógica de cartas va en JS. La UI va en el editor de eventos.

### Verdad o Reto
- Se activa si Corrupción >20 y Afecto >30
- Banco de preguntas y retos indexado por nivel de Corrupción (bajo/medio/alto/máximo)
- Rechazar verdad: -3 Confianza | Rechazar reto: -5 Confianza
- Respuesta honesta difícil: +8 Confianza | Reto completado: +5 Afecto +3 Corrupción

**Nota del Arquitecto sobre mini-juegos en MZ:**
En el proyecto anterior estos mini-juegos usaban el sistema `menu:` de Ren'Py.
En MZ se implementan con el sistema de mensajes + opciones del editor de eventos,
apoyado por funciones JS para la lógica de cartas (Strip 21) y selección
aleatoria del banco (Verdad o Reto). El Dev Contenido debe revisar
la documentación de MZ sobre "Show Choices" y "Control Variables" antes
de implementar. No improvisar la estructura.

---

## 11. GALERÍA

Accesible desde el teléfono del protagonista en cualquier momento.

### Contenido
- Escenas narrativas desbloqueadas (automático al completarlas)
- Interacciones libres marcadas como favoritas (el jugador elige)
- Fotos coleccionables (sistema S16)
- Imágenes voyeur desbloqueadas (sistema S15)

### Filtros
- Por personaje (Celine / Roxy / Luna)
- Por tipo (escena narrativa / interacción libre / foto / voyeur)
- Por semana del juego

---

## 12. PANTALLA DE TÍTULO

- Fondo: el apartamento de noche con lluvia suave
- Las 3 siluetas visibles desde la ventana — sin detalle, solo presencia
- Música: tema ambiente nocturno
- Menú: Nuevo juego / Continuar / Galería / Configuración / Salir

---

## 13. SISTEMA DE SAVES

- **Slots manuales:** 10
- **Autosave:** Automático al avanzar franja (sobreescribe el slot de autosave)
- **Acceso:** Desde el menú en cualquier momento fuera de escenas

---

## 14. SCOPE

### v1.0 — Lo que se entrega
- 3 personajes completos con todas sus historias
- 21 sistemas implementados
- Sandbox sin fin definido
- Mini-juegos: Strip 21 y Verdad o Reto
- Arte generado con SD + LoRA
- Solo en español

### Fuera de scope v1.0 — no se implementa, no se planea, no se discute
- Voces actuadas
- Animaciones de escenas
- Más de 3 personajes principales
- Multijugador
- Versión en inglés (v1.1)

### Escalabilidad pensada desde el inicio
- Nuevo personaje = nuevo archivo JS + sprites + archivo de eventos
- Stats genéricos para cualquier personaje (no hardcodeados)
- Mini-juegos modulares — nuevos juegos = nuevo archivo
- Tienda expandible sin tocar código existente

---

## 15. PALETA UI

| Elemento | Color hex |
|----------|-----------|
| Fondo principal | `#0d0d0f` |
| Paneles / cajas | `#13131a` |
| Texto principal | `#e2e8f0` |
| Acento Celine | `#818cf8` |
| Acento Roxy | `#fb923c` |
| Acento Luna | `#c084fc` |
| Positivo / dinero | `#4ade80` |
| Negativo / alerta | `#f87171` |

---

## 16. AUDIO

- 4 temas musicales base (uno por franja horaria)
- Temas especiales por personaje (se activan en eventos de esa personaje)
- Efectos: notificaciones, puertas, lluvia, ambiente nocturno
- Fuente: packs royalty-free — itch.io / freesound.org
- Formato: OGG para música, WAV para efectos cortos

---

## 17. ESTRUCTURA DE ARCHIVOS DEL PROYECTO

```
ApartmentSim/
├── js/
│   ├── plugins/              ← plugins oficiales y de terceros (NO tocar)
│   └── custom/               ← TODO el JS nuestro va aquí
│       ├── core/             ← stats, tiempo, economía, mood, celos, secretos, memoria
│       ├── characters/       ← lógica específica por personaje
│       └── systems/          ← mini-juegos, galería, voyeur, fotos
├── img/
│   ├── characters/           ← sprites por personaje, expresión y outfit
│   │   ├── celine/
│   │   ├── roxy/
│   │   └── luna/
│   ├── backgrounds/          ← fondos por locación y franja horaria
│   ├── ui/                   ← HUD, iconos, marcos, botones
│   └── gallery/              ← imágenes de galería desbloqueables
├── audio/
│   ├── bgm/                  ← música de fondo
│   └── se/                   ← efectos de sonido
├── data/                     ← archivos JSON de RPG Maker MZ (NO editar a mano)
├── docs/                     ← GDD_CORE, GDD_PERSONAJES, GDD_SISTEMAS, etc.
└── _dev/                     ← ESTADO_ACTUAL de cada dev + notas internas
    ├── ESTADO_ACTUAL_MAESTRO.md
    ├── ESTADO_ACTUAL_CORE.md
    ├── ESTADO_ACTUAL_CONTENIDO.md
    └── ESTADO_ACTUAL_ARTE.md
```

**Regla absoluta:** La carpeta `_dev/` NUNCA se empaqueta en el build final.
La carpeta `js/plugins/` NUNCA se toca — solo se agregan plugins, nunca se editan.
Cada dev trabaja SOLO en sus carpetas. Ver ESTANDARES.md para detalle.

---

## 18. REPOSITORIO GITHUB

Estructura del repo:
```
docs/          ← todos los GDDs y documentos de diseño
_dev/          ← estados actuales de cada dev
js/custom/     ← todo el código JS del proyecto
img/           ← assets finales aprobados
```

**Regla de commits:**
- Cada sesión termina con un commit
- Mensaje de commit: `[ROL] descripción breve — ESTADO_ACTUAL actualizado`
- Ejemplo: `[Dev Core] sistema de stats completo — ESTADO_ACTUAL_CORE actualizado`
- NUNCA hacer commit sin actualizar el ESTADO_ACTUAL del dev correspondiente

---

## 19. REGLAS QUE NUNCA SE ROMPEN

1. Ningún dev escribe código antes de que VARIABLES_GLOBALES.md y ESTANDARES.md estén completos
2. Ninguna variable se declara en código sin estar en VARIABLES_GLOBALES.md primero
3. Ningún stat se modifica directamente — siempre a través de las funciones de sistema
4. Cada sesión termina con ESTADO_ACTUAL del dev actualizado — sin excepción
5. Cada sesión termina con commit a GitHub — sin excepción
6. Máximo 2-3 archivos entregados por sesión
7. Si un dev necesita una variable de otro módulo, avisa al usuario antes de crearla
8. Todo comentado en español
9. El Dev Debugger es el último recurso — no el primero
10. El contenido adulto es intencional y parte del diseño — no se filtra, no se omite

---

*Documento generado por el Arquitecto — Apartment Sim v1.0 RPG Maker MZ*
*Próximo documento: GDD_PERSONAJES.md*
