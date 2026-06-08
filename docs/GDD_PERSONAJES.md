# APARTMENT SIM — GDD PERSONAJES
### Versión 1.0 — RPG Maker MZ | Generado: 07/06/2026

---

## INSTRUCCIÓN PARA DEVS

Este documento es la referencia de personajes. Contiene todo lo que necesitas
saber sobre quién es cada personaje, cómo actúa, qué le pasa y cuándo.

**Dev Core:** Usa las secciones de stats iniciales, rutinas y umbrales de Corrupción.
**Dev Contenido:** Usa los 5 actos, las quests, los secretos y los diálogos por franja.
**Dev Arte:** Usa las descripciones físicas y las tablas de expresiones por personaje.

Si algo aquí contradice GDD_CORE.md, GDD_CORE.md gana. Si hay ambigüedad
narrativa que este documento no resuelve, consulta al Arquitecto — no improvises.

---

## 1. EL PROTAGONISTA

**Nombre:** Elegible al inicio (default: Kai)
**Edad:** 22
**Carrera:** Ingeniería de Sistemas (primer año)
**Visual:** Sin sprite propio en diálogos. Perspectiva primera persona.
En escenas grupales: silueta/sombra sin detalle — el jugador se proyecta.

### Backstory
Llegó a Vallanova para estudiar. Necesitaba cuarto urgente.
Su prima Celine tenía uno disponible en el apartamento que comparte con Roxy y Luna —
conocidas del mismo grupo desde el bachillerato.
Las conoce a todas desde antes. El tiempo y la distancia cambiaron las cosas.
Lo que antes era simple ya no lo es.

### Personalidad base
Neutral — se construye con las elecciones del jugador.
No tiene voz propia en diálogos: solo actúa.
Sus decisiones acumulan puntos en 3 arquetipos (ver GDD_SISTEMAS.md S03).

### Relaciones de entrada
| Personaje | Vínculo | Tensión inicial |
|-----------|---------|----------------|
| Celine | Prima — historia de toda la vida | Algo pasó entre ellos. Ninguno lo nombra. Ella lo trata con distancia calculada. |
| Roxy | Amiga del bachillerato / ex-algo sin nombre | Hubo algo romántico que no terminó bien. Ella actúa como si nada. Él sabe que no es verdad. |
| Luna | Conocida del mismo grupo — siempre observó | Nunca fueron cercanos pero ella siempre lo miró diferente. Ahora viven juntos. |

### Stats iniciales
- Encanto: 10 (rango 0-100)
- Energía máxima: 6 acciones por día (rango 6-10)
- Dinero: $500 (sin máximo)
- Personalidad: 0 / 0 / 0 (Romántico / Atrevido / Respetuoso)

### Cómo suben sus stats
- **Encanto:** Leer un libro (+2, máx 1 vez por libro) | conversación exitosa (+1) | quest con resultado óptimo (+3-5)
- **Energía máxima:** Ejercicio en mañana 5 días consecutivos = +1 nivel (máx 10)
- **Dinero:** Trabajar 1 acción = $50-150 aleatorio | quests completadas | eventos especiales

---

## 2. DÍA 1 — LLEGADA AL APARTAMENTO

Secuencia lineal. Sin opciones aún. Es el tutorial narrativo.

1. Celine abre la puerta. Lo mira un segundo. *"El cuarto es el del fondo. No hagas ruido después de las 11."* Se da vuelta y se va.
2. Roxy aparece de la cocina con una cuchara y una mancha en la camiseta. *"¡LLEGASTE! Pensé que era mañana. ¿Comiste? Estoy haciendo algo que podría ser pasta."* Abrazo inmediato sin pedir permiso.
3. Luna está en el sofá leyendo. Levanta la vista. Te mira exactamente tres segundos. Vuelve al libro. *"Hola."*
4. El jugador deja sus cosas en el cuarto. Desde ahí se escucha a Roxy cantando algo mal en la cocina.
5. Tutorial de HUD — el juego explica franjas, acciones y stats.
6. Fin del día 1. Comienza el juego libre.

**Stats después del día 1:**
| Personaje | Afecto | Confianza | Corrupción |
|-----------|--------|-----------|------------|
| Celine | 15 | 10 | 5 |
| Roxy | 25 | 15 | 15 |
| Luna | 20 | 10 | 10 |

*Nota: Los valores iniciales reflejan historia previa — no se conocen de cero.*

---

## 3. CELINE — LA FRÍA

**Edad:** 22 | **Carrera:** Derecho (tercer año)
**Relación:** Prima del protagonista
**Color UI:** `#818cf8`

### Apariencia física
- Pelo negro azabache, corte hime (flequillo recto, largo hasta los hombros)
- Ojos gris hielo — mirada directa, casi incómoda
- 172cm, complexión voluptuosa pero controlada en cómo se viste
- Siempre impecable en casa — ropa formal, nunca descuidada

### Personalidad
Tsundere inteligente — no es torpe ni cliché. Sarcástica, controlada, perfeccionista.
Dice exactamente lo que quiere decir y nada más. El silencio es su arma favorita.
Estudia derecho porque le gusta tener razón y que quede documentado.

En el fondo: tiene miedo de querer algo que no debería querer.
Lo lleva con dignidad absoluta.

### Dinámica con el protagonista
Son primos. Eso debería ser suficiente barrera. No lo es.
Hay algo sin resolver desde antes — un momento, una conversación, algo que los dos
recuerdan distinto. Ella lo invitó a vivir ahí porque Roxy insistió y no podía
decir que no sin dar explicaciones. Se arrepiente y no se arrepiente al mismo tiempo.

### Progresión emocional por nivel de Afecto
| Rango | Comportamiento visible |
|-------|----------------------|
| 0-30 | Distante, formal. Lo ignora si puede. Lo trata como un extraño que paga renta. |
| 31-60 | Busca excusas para estar cerca. Lo niega. Se molesta si la descubres. |
| 61-100 | Sigue siendo fría y sarcástica pero ya no puede ocultar que le importa. |

### Umbrales de Corrupción
| Nivel | Comportamiento |
|-------|---------------|
| 0-20 | Rechaza cualquier contacto físico. Si te acercas demasiado se va. |
| 21-45 | Acepta cercanía mínima. No se mueve si te sientas cerca. |
| 46-70 | Besos con vergüenza visible — lo odia y no se va. |
| 71-90 | Pierde el control en momentos específicos. Después actúa como si no pasó. |
| 91-100 | Sin barreras. Sigue siendo Celine — fría, directa — pero ya no finge. |

### Rutina semanal (lunes a viernes)
| Franja | Ubicación | Disponible para interactuar |
|--------|-----------|----------------------------|
| Mañana | Cocina (café en silencio) o su cuarto | Cocina: sí. Cuarto: no. |
| Tarde | Su cuarto o biblioteca de la universidad | No disponible |
| Noche | Sala (lee expedientes) o su cuarto | Sala: sí |
| Madrugada | Su cuarto — guardia baja | Solo si Afecto >40 |

### Rutina fin de semana (sábado y domingo)
| Franja | Ubicación | Disponible |
|--------|-----------|-----------|
| Mañana | Su cuarto o cocina | Sí — más relajada |
| Tarde | Casa o salida (compras, cafetería) | Variable |
| Noche | Sala o su cuarto | Sí |
| Madrugada | Su cuarto | Solo si Afecto >40 |

### Mensajes de texto
Estilo: formales, cortos, sin emojis. Esconden más de lo que dicen.
- *"¿Ya comiste?"* → Le importas pero no lo va a decir así
- *"Llega temprano hoy."* → Sin explicación. Sin contexto.
- *"."* → Solo un punto. Pensó en ti y no encontró qué decir.
- A Corrupción >50: mensajes más tarde en la noche. Mismo tono, hora diferente.

### Expresiones para sprites (Dev Arte)
| Expresión | Cuándo aparece |
|-----------|---------------|
| `neutral` | Estado base — cara controlada, sin emoción clara |
| `fria` | Afecto bajo — mirada cortante, postura cerrada |
| `seria` | Cuando habla de algo importante |
| `sorprendida` | Rarísimo — cuando algo la toma desprevenida |
| `molesta` | Cuando la descubren o la presionan |
| `traviesa` | Corrupción >60 — sonrisa apenas perceptible, ojos diferentes |
| `vulnerable` | Madrugada / Acto 4 y 5 — la máscara baja |

### Secretos descubribles
| # | Secreto | Condición de descubrimiento | Efecto al descubrir |
|---|---------|---------------------------|---------------------|
| 1 | Diario con entradas sobre el protagonista — cajón izquierdo escritorio | Estar en su cuarto + Afecto ≥35 | +10 Confianza. Desbloquea regalo secreto. |
| 2 | Roxy sabe algo que no debería — Celine se lo confió en un momento débil | Escuchar desde el pasillo + Tarde + Día ≥7 | +10 Confianza. Diálogos únicos con Roxy sobre Celine. |
| 3 | A veces llora sola de madrugada | Estar en pasillo + Madrugada + Día ≥14 | +10 Confianza. Si entras: evento especial. Si no entras: +5 extra. |

### Consecuencias de usar secretos
- **Usar a favor (opción positiva):** +15 Afecto +10 Confianza. Diálogo único de vulnerabilidad.
- **Usar en contra:** -20 Confianza permanente. Bloquea quest "El diario". Ella lo menciona en diálogos futuros.

### Quests
**Quest 1 — "Los apuntes"**
- Trigger: Confianza ≥30
- Evento: Necesita ayuda con un caso de derecho procesal. Lo pide de forma indirecta.
- Decisión: Ayudar bien (requiere Encanto >30 o elegir opciones correctas) vs ayudar mal
- Resultado óptimo: +15 Confianza. Desbloquea mensajes nocturnos de Celine.
- Consecuencia a largo plazo: Si fallas, -10 Confianza y relación se enfría una semana.

**Quest 2 — "La defensa"**
- Trigger: Afecto ≥50
- Evento: Alguien la confronta en público (universidad o cafetería). El jugador está presente.
- Decisión: Intervenir / No intervenir / Intervenir mal
- Resultado óptimo: +20 Afecto +10 Confianza. Celine no lo agradece en voz alta pero cambia.
- Consecuencia a largo plazo: Si no intervienes, -10 Afecto. Ella no lo menciona pero lo recuerda.

**Quest 3 — "El diario"**
- Trigger: Secreto 1 descubierto
- Evento: El jugador tiene el diario en sus manos. Nadie sabe.
- Decisiones: Leerlo completo / Cerrarlo sin leer / Preguntarle directamente
- Resultado leerlo: +15 Corrupción pero -15 Confianza. Desbloquea contenido pero daña la relación.
- Resultado cerrarlo: +20 Confianza. Ella lo descubre que lo encontraste y que no lo leíste — reacción única.
- Resultado preguntarle: +10 Afecto +10 Confianza. Conversación más honesta que ninguna hasta ahora.
- Consecuencia a largo plazo: Cada opción cambia el tono de los diálogos futuros de forma permanente.

---

### 5 ACTOS NARRATIVOS — CELINE

**Acto 1 (Afecto ≥20): "El apagón"**
Trigger: Afecto 20. Primera noche con Armonía >50 y jugador en sala de noche.

Desarrollo: Se corta la luz. Roxy y Luna están en sus cuartos. Celine estaba estudiando en la sala — no lo sabías. En la oscuridad no puede controlarse tanto. La conversación empieza incómoda y deriva sola.

Decisiones:
- Respetuoso: *"Si quieres te busco una linterna y te dejo estudiar."* → +8 Confianza +5 Afecto
- Romántico: *"No me molesta la oscuridad si tú tampoco."* → +10 Afecto +3 Confianza
- Atrevido: *"Es la primera vez que te veo sin tener el control de todo."* → +5 Afecto -2 Confianza (le molesta que tenga razón)

Resultado: Cuando vuelve la luz, Celine recoge sus cosas. Antes de irse: *"Buenas noches."* Dos palabras. Para ella es mucho.
Consecuencias: Desbloquea diálogo especial al día siguiente. Celine te busca en cocina sin razón aparente.

---

**Acto 2 (Afecto ≥40): "Los apuntes"**
Trigger: Afecto 40 + quest "Los apuntes" completada.

Desarrollo: La sesión de trabajo dura hasta tarde. En algún momento, mirando sus notas sin mirarte: *"Eres más útil de lo que pensaba."*

Decisiones:
- *"No tenía muchas expectativas tampoco."* → Ella ríe. Una vez. Breve. +10 Afecto
- *"¿Eso es un cumplido?"* → *"No lo sobreinterpretes."* +8 Afecto +5 Confianza
- Atrevido: *"¿Y en qué otras cosas podría ser útil?"* → Silencio. Te mira. No responde. +5 Afecto +8 Corrupción

Resultado: Al terminar, cuando te vas, ella dice tu nombre — el que elegiste al inicio. Solo eso.
Consecuencias: Desbloquea mensajes nocturnos de Celine.

---

**Acto 3 (Afecto ≥60): "La fiesta de Roxy"**
Trigger: Afecto 60. Ocurre en cumpleaños de Roxy (Día 28).

Desarrollo: La fiesta termina. Roxy dormida en el sofá. Luna en su cuarto. Celine está en la cocina buscando agua. Tú también. El apartamento en silencio con música baja. Ninguno tenía plan de que esto pasara.

Decisiones:
- Romántico: Te quedas sin moverte. Ella tampoco. *"¿Por qué sigues aquí?"* *"¿Por qué tú?"* → +12 Afecto +8 Corrupción
- Atrevido: Te acercas. Ella no retrocede. → +10 Afecto +15 Corrupción. Si Corrupción >40: posibilidad de primer beso.
- Respetuoso: *"Buenas noches, Celine."* Te vas. → +5 Confianza. Ella te mira irse.

Resultado si hubo beso: Celine se aparta. *"Eso no..."* Pausa. *"Buenas noches."* No sale hasta la mañana siguiente.
Consecuencias: Hubo beso → Celine evita 2 días, al tercero busca hablar. No hubo beso → avanza más despacio pero con más Confianza.

---

**Acto 4 (Afecto ≥80): "La confesión a medias"**
Trigger: Afecto 80. Madrugada, semana 6+.

Desarrollo: La encuentras despierta a las 2am en la sala. El libro abierto en la misma página hace una hora. *"No puedo dormir."* Para Celine eso es mucho. La conversación deriva. En algún momento: *"A veces pienso que las cosas serían más fáciles si no vivieras aquí."*

Decisiones:
- *"¿Más fáciles para quién?"* → Pausa larga. *"Para mí."* +15 Afecto. Se levanta y se va.
- *"Puedo buscar otro cuarto si quieres."* → *"No dije eso."* Silencio. +10 Afecto +10 Confianza
- Atrevido: *"¿O más difíciles?"* → Te mira. No responde. +12 Afecto +20 Corrupción

Consecuencias: Desbloquea diálogos de vulnerabilidad. Celine empieza a buscarte más — siempre con excusas.

---

**Acto 5 (Afecto ≥100): "Sin barreras"**
Trigger: Afecto 100 + Corrupción >70 + Confianza >65.

Desarrollo: Domingo por la mañana. Roxy salió. Luna en su cuarto. Celine te trae café sin pedirlo. Se sienta frente a ti. Te mira.
*"Voy a decir algo y no quiero que lo conviertas en un problema."*
Pausa.
*"Me importas. De una forma que no debería importarme y que no voy a seguir ignorando."*

Decisiones:
- Romántico: *"A mí también."* → Escena íntima con carga emocional alta. Sin prisa.
- Atrevido: Sin palabras — te acercas. Ella deja que te acerques. → Escena íntima más intensa.
- *"¿Qué pasa con que somos primos?"* → *"Lo sé."* Pausa. *"¿Y?"* → Cualquiera de las dos opciones anteriores disponibles.

**Escena adulta:** Completa y explícita. Tono de Celine intacto — sarcástica, controlada, pero sin fingir que no quiere estar ahí.
Consecuencias: Relación entra en nuevo estado. Nuevas interacciones libres. Celos activos en Roxy y Luna.

---

## 4. ROXY — LA CAÓTICA

**Edad:** 21 | **Carrera:** Diseño Gráfico (segundo año)
**Relación:** Ex-algo del protagonista. Sin nombre oficial para lo que fueron.
**Color UI:** `#fb923c`

### Apariencia física
- Pelo naranja-rojo brillante, coletas altas asimétricas
- Ojos ámbar — expresivos, imposibles de leer y al mismo tiempo completamente obvios
- 165cm, complexión atlética, siempre en movimiento
- Ropa colorida, capas, pins en la mochila — nunca dos días igual

### Personalidad
Impulsiva, sin filtros, energética. Dice lo que piensa antes de pensar si debería.
Ríe fuerte. Ocupa espacio. Convierte cualquier situación en un juego.
Usa el humor como escudo — si todo es un chiste, nada puede doler de verdad.

En el fondo: todavía no procesó lo que pasó entre ellos. Prefiere no hacerlo.
Funciona mejor así. Hasta que no puede más.

### Dinámica con el protagonista
Fueron algo. Terminó mal por razones que ninguno explica bien.
Ella decidió que seguirían siendo amigos y actuó como si esa decisión lo resolvía todo.
Él no tuvo voz en eso. Ahora viven juntos y ella hace chistes sobre lo incómodo
que es mientras claramente no le parece incómodo en absoluto.

### Progresión emocional por nivel de Afecto
| Rango | Comportamiento visible |
|-------|----------------------|
| 0-30 | Cariñosa pero superficial. Te incluye en planes, memes, nada real. |
| 31-60 | Empieza a mostrarte al Roxy de verdad — el que dibuja a las 2am, el que llora con música, el que tiene miedo. |
| 61-100 | Ya no actúa. Es la primera vez en años que no actúa con alguien. |

### Umbrales de Corrupción
| Nivel | Comportamiento |
|-------|---------------|
| 0-20 | Coquetea pero para sola. Lo convierte en broma. |
| 21-45 | Besos impulsivos que "no cuentan". Ella lo decide. |
| 46-70 | Escenas íntimas con comentarios graciosos en medio — es su manera de no perder el control. |
| 71-90 | Completamente desinhibida. Sin chistes. Sin parar. |
| 91-100 | Sin límites. Lo vive como una aventura. Sonríe diferente. |

### Rutina semanal (lunes a viernes)
| Franja | Ubicación | Disponible |
|--------|-----------|-----------|
| Mañana | Cocina (caótica) o sala (música alta) | Sí |
| Tarde | Sala dibujando o afuera sin avisar | Variable — 50% está |
| Noche | Sala o su cuarto — puerta abierta siempre | Sí |
| Madrugada | Sala (rara vez duerme antes de las 3am) | Sí |

### Rutina fin de semana
| Franja | Ubicación | Disponible |
|--------|-----------|-----------|
| Mañana | Dormida hasta tarde | No antes de tarde |
| Tarde | Casa o afuera — más relajada | Sí |
| Noche | Casa o salida | Sí |
| Madrugada | Vuelve tarde, sala | Sí |

### Mensajes de texto
Estilo: memes, audios de voz, strings de emojis sin contexto, mensajes a las 3am.
- *"oye"* → sin continuación durante 20 minutos
- *"🦆🔥💀"* → no hay explicación disponible
- Audio de 43 segundos riéndose de algo → te tenías que haber quedado
- A Corrupción >50: mensajes más directos, menos emojis, más tarde en la noche.

### Expresiones para sprites (Dev Arte)
| Expresión | Cuándo aparece |
|-----------|---------------|
| `neutral` | Estado base — sonrisa fácil, postura abierta |
| `feliz` | Cuando algo la divierte o le emociona |
| `concentrada` | Dibujando — raramente disponible en este estado |
| `sorprendida` | Cuando algo la toma desprevenida de verdad |
| `seria` | Acto 3 en adelante — cuando baja la guardia |
| `traviesa` | Coqueteando — frecuente |
| `vulnerable` | Acto 3, 4 y 5 — cuando el humor no llega |

### Secretos descubribles
| # | Secreto | Condición de descubrimiento | Efecto al descubrir |
|---|---------|---------------------------|---------------------|
| 1 | Bocetos del protagonista en su sketchbook — muchos, de distintas épocas | Ver el sketchbook cuando lo deja en sala (sin requisito de Afecto) | +10 Confianza. Desbloquea diálogos únicos sobre el arte. |
| 2 | Fotos guardadas de los dos — dice que las iba a borrar | Cajón de su cuarto + Afecto ≥40 | +10 Confianza. Desbloquea regalo secreto. |
| 3 | Le habla a Luna de ti — Luna guarda todo | Escuchar conversación + Noche + Día ≥7 | +10 Confianza. Diálogos únicos cruzados Roxy-Luna. |

### Consecuencias de usar secretos
- **Usar a favor:** +15 Afecto +10 Confianza. Primer momento en que Roxy se queda sin chiste.
- **Usar en contra:** -20 Confianza permanente. Bloquea quest "La noche que llora". Roxy lo menciona con humor que no es humor.

### Quests
**Quest 1 — "El sketchbook"**
- Trigger: Afecto ≥25
- Evento: Te muestra sus diseños por primera vez — normalmente cierra el cuaderno. Esta vez no.
- Decisión: Reaccionar genuinamente / Reaccionar superficial / No mirar
- Resultado óptimo: +15 Afecto. Desbloquea diálogos únicos sobre arte y sobre ella.
- Consecuencia: Si reaccionas mal o no miras → -10 Afecto. No vuelve a mostrarte nada por una semana.

**Quest 2 — "La exposición"**
- Trigger: Afecto ≥55
- Evento: Tiene exposición de diseño en la universidad. Menciona la fecha una sola vez.
- Decisión: Ir / No ir (el jugador tiene que recordar la fecha — no hay recordatorio)
- Resultado óptimo: +20 Afecto. Escena única en la exposición.
- Consecuencia: Si no vas → -15 Afecto permanente. Roxy nunca lo menciona directamente. Eso es peor.

**Quest 3 — "La noche que llora"**
- Trigger: Confianza ≥60
- Evento: La encuentras llorando. No lo estaba esperando. No tiene excusa lista.
- Decisión: Quedarte en silencio / Preguntar qué pasó / Hacer un chiste para aligerar
- Resultado quedarte: +15 Confianza +10 Afecto. Primera vez que alguien la deja llorar sin arreglarlo.
- Resultado preguntar: +10 Confianza. Habla. Algo real.
- Resultado chiste: -10 Confianza. *"Sí, gracioso."* Se va.
- Consecuencia a largo plazo: Afecta la dinámica con Luna (Luna sabe lo que pasó, reacciona según elección).

---

### 5 ACTOS NARRATIVOS — ROXY

**Acto 1 (Afecto ≥20): "El sketchbook"**
Trigger: Afecto 20. Sala, tarde, ella dibujando.

Desarrollo: Normalmente cierra el cuaderno cuando alguien se acerca. Esta vez no. Lo deja abierto y te mira como esperando. *"¿Qué?"* Como si tú fueras el raro. Si te acercas: páginas llenas de diseños. Y en una página, sin señalarla: un boceto tuyo. Reciente.

Decisiones:
- Romántico: *"Este soy yo."* No como pregunta. → *"No sé de qué hablas."* +12 Afecto +5 Corrupción
- Respetuoso: Solo miras sin señalarlo. *"Son buenos."* → *"Lo sé."* +8 Afecto +8 Confianza
- Atrevido: *"¿Cuántos de estos hay?"* → Cierra el cuaderno. Sonríe. *"Los suficientes."* +10 Afecto +10 Corrupción

Consecuencias: Desbloquea secreto 1 de Roxy. Nuevos diálogos sobre arte.

---

**Acto 2 (Afecto ≥40): "La apuesta"**
Trigger: Afecto 40. Noche de fin de semana.

Desarrollo: *"Oye. ¿Jugamos algo?"* Sin contexto. Propone una apuesta. Las reglas son obviamente favorables para el jugador — porque quiere perder o quiere que él gane.

Primera aparición del Strip 21. Las apuestas son prendas.

Resultado: Dependiendo de quién gana, primer contacto físico significativo.
- Ella pierde: +10 Afecto +15 Corrupción
- Él pierde y acepta reto: +8 Afecto +10 Corrupción

Después: Roxy actúa como si nada. Le manda un meme a las 2am. Es su forma de procesar.

---

**Acto 3 (Afecto ≥60): "La pelea"**
Trigger: Afecto 60. Cuando Armonía baja de 55 O semana 5+.

Desarrollo: Discusión real sobre lo que pasó entre ellos. Roxy lleva semanas evitando el tema. Esta vez no puede. *"¿Por qué te fuiste?"* Sin setup. Sin contexto. Como si continuara una conversación que nunca terminó.

Decisiones:
- Atrevido: *"¿Por qué me dejaste ir?"* → Silencio. Después: +15 Afecto -5 Confianza
- Romántico: Escucharla primero sin interrumpir → +10 Afecto +15 Confianza
- Respetuoso: *"Podemos hablarlo cuando quieras. No me voy a ningún lado."* → +12 Confianza +8 Afecto

Resultado: Roxy llora. Una vez, rápido. Se limpia y hace un chiste inmediatamente. *"Olvida que dije eso."* Los dos saben que ya no puede olvidarse.
Consecuencias: Roxy empieza a bajar la guardia en pequeños momentos.

---

**Acto 4 (Afecto ≥80): "2am"**
Trigger: Afecto 80 + Corrupción >55. Madrugada, cualquier día.

Desarrollo: Aparece en la puerta de tu cuarto. No llama. La puerta estaba entreabierta. *"No podía dormir."* Pausa. *"¿Puedo?"* Sin más explicación. Entra, se sienta en el borde de la cama. No hay chiste esta vez.

Decisiones:
- *"Sí."* → Se queda. Duermen sin más. Primera vez que no actúa. +15 Afecto +10 Confianza
- Atrevido: La miras. Ella te mira. → Escena íntima nivel 2-3 según Corrupción actual
- Romántico: *"¿Qué pasó?"* → Habla. Por primera vez de verdad. +20 Confianza

Consecuencias: Después de esta noche, Roxy empieza a aparecer en espacios tuyos sin anunciarse. Como si ya fuera normal.

---

**Acto 5 (Afecto ≥100): "Sin actuar"**
Trigger: Afecto 100 + Corrupción >75 + Confianza >60.

Desarrollo: La encuentras en su cuarto. Tiene música y está dibujando. Cuando entras no hace el chiste de siempre. Te mira. Deja el lápiz.
*"¿Sabes cuánto tiempo llevo actuando como si no me importaras?"*

Decisiones:
- *"¿Cuánto?"* → *"Demasiado."* → Escena íntima con carga emocional real
- Atrevido: Sin responder, te acercas. → Escena íntima intensa. Ningún freno.
- Romántico: *"No tienes que seguir haciéndolo."* → Escena más lenta, más íntima.

**Escena adulta:** Completa y explícita. Tono de Roxy — energética pero esta vez sin broma al final. Solo silencio cómodo después. Inusual para ella.
Consecuencias: Relación entra en nuevo estado. Nuevas interacciones libres. Celos activos en Celine y Luna.

---

## 5. LUNA — LA MISTERIOSA

**Edad:** 22 | **Carrera:** Literatura (tercer año)
**Relación:** Conocida del grupo — siempre observó, nunca habló mucho
**Color UI:** `#c084fc`

### Apariencia física
- Pelo plateado con tintes lavanda, largo y liso — siempre algo lo sostiene: horquilla, lápiz, nada
- Ojos violeta claro — raramente hace contacto visual; cuando lo hace es incómodo de sostener
- 168cm, delgada, movimientos lentos y deliberados
- Ropa suave en capas, siempre tiene un libro o cuaderno en la mano

### Personalidad
Callada — no tímida, sino selectiva. Observa todo y dice poco.
Cuando habla, cada palabra fue elegida. No hace preguntas porque ya sabe las respuestas
o prefiere no saber. Escribe en un cuaderno negro que nunca deja ver.

En el fondo: siente cosas con una intensidad que asustaría a cualquiera.
Por eso no habla. Por eso escribe.

### Dinámica con el protagonista
Nunca fueron cercanos pero ella siempre lo miró diferente — desde el bachillerato.
Nadie sabe por qué. Ella tampoco lo explica. Ahora viven juntos y la dinámica
es la misma: él existe, ella observa, el aire entre los dos tiene algo que ninguno nombra.

### Progresión emocional por nivel de Afecto
| Rango | Comportamiento visible |
|-------|----------------------|
| 0-30 | Asiente, responde con monosílabos, desaparece a su cuarto. |
| 31-60 | Empieza a buscarte sin razón aparente. Aparece donde estás. |
| 61-100 | Te habla de verdad. Pocas palabras, mucho peso. Es el mayor regalo que puede dar. |

### Umbrales de Corrupción
| Nivel | Comportamiento |
|-------|---------------|
| 0-20 | Ignora insinuaciones — no con rechazo, simplemente no registra. |
| 21-45 | Permite cercanía en silencio. No se mueve. No dice nada. |
| 46-70 | Acciones inesperadas sin explicación — toca, no explica, desaparece. |
| 71-90 | Se entrega en silencio absoluto. Ojos abiertos. Sin palabras. |
| 91-100 | Dice en voz alta todo lo que nunca dijo. Una vez. Solo una vez. |

### Rutina semanal (lunes a viernes)
| Franja | Ubicación | Disponible |
|--------|-----------|-----------|
| Mañana | Cocina (té, silencio absoluto) | Sí, pero no interrumpir |
| Tarde | Su cuarto o sala (leyendo) | Sala: sí. Cuarto: solo Afecto >35 |
| Noche | Sala o balcón | Sí — especialmente balcón con luna |
| Madrugada | Cocina o ventana de su cuarto | Variable — no siempre duerme |

### Rutina fin de semana
| Franja | Ubicación | Disponible |
|--------|-----------|-----------|
| Mañana | Paseo matutino, vuelve antes del mediodía | No hasta que vuelve |
| Tarde | Casa — más abierta que entre semana | Sí |
| Noche | Sala o balcón | Sí — más disponible que entre semana |
| Madrugada | Cocina o ventana | Variable |

### Mensajes de texto
Estilo: frases sueltas, sin contexto, a horas impredecibles.
- *"."* → Solo eso.
- *"¿Estás despierto?"* → A las 2:47am. Sin continuación.
- *"El libro que mencionaste estaba bien."* → Tres días después de la conversación.
- A Corrupción >50: citas de libros que claramente hablan de ti.

### Expresiones para sprites (Dev Arte)
| Expresión | Cuándo aparece |
|-----------|---------------|
| `neutral` | Estado base — cara sin emoción clara, mirada ligeramente baja |
| `observando` | Cuando te mira fijamente — frecuente |
| `leyendo` | Concentrada en libro/cuaderno |
| `sorprendida` | Rarísimo — cuando algo la toma completamente desprevenida |
| `triste` | Acto 3, secreto 3 |
| `traviesa` | Corrupción >60 — sonrisa muy sutil, apenas perceptible |
| `vulnerable` | Acto 4 y 5 — cuando habla de verdad |

### Secretos descubribles
| # | Secreto | Condición de descubrimiento | Efecto al descubrir |
|---|---------|---------------------------|---------------------|
| 1 | Página del cuaderno con el nombre del protagonista escrito muchas veces | Ver el cuaderno cuando lo deja olvidado — evento aleatorio | +10 Confianza. Desbloquea regalo secreto y diálogos únicos. |
| 2 | Cartas que nunca envió — en el fondo del cajón de su cuarto | Cuarto de Luna + Madrugada + Afecto ≥45 | +10 Confianza. Desbloquea diálogos únicos sobre lo que siente. |
| 3 | La encuentras llorando en la cocina a las 3am | Cocina + Madrugada + Día ≥14 | +10 Confianza. Si te quedas sin preguntar: +20 Confianza adicional. |

### Consecuencias de usar secretos
- **Usar a favor:** +15 Afecto +10 Confianza. Luna dice algo en voz alta que normalmente solo escribe.
- **Usar en contra:** -20 Confianza permanente. Luna desaparece 5 días. Cuando vuelve, nunca menciona lo que pasó. Eso es lo que más duele.

### Quests
**Quest 1 — "El libro"**
- Trigger: Afecto ≥20
- Evento: Te deja un libro en la mesa sin decir nada. Si lo lees antes de 2 días de juego y le dices algo real: reacción única.
- Decisión: Leerlo y responder genuinamente / Responder superficialmente / No leerlo
- Resultado óptimo: +20 Confianza. Luna hace una pregunta — la primera que te hace a ti.
- Consecuencia: No leerlo → -5 Afecto silencioso. Nunca lo menciona.

**Quest 2 — "Tres días de silencio"**
- Trigger: Confianza ≥40
- Evento: Quest pasiva — no preguntarle nada durante 3 días completos de juego.
- Regla: Si el jugador inicia conversación con Luna en esos 3 días, la quest se reinicia.
- Resultado: Si lo logra → +25 Confianza. Luna habla primero por primera vez.
- Consecuencia: Es la única quest que depende de lo que el jugador NO hace.

**Quest 3 — "El mismo libro"**
- Trigger: Afecto ≥70
- Evento: Leer el mismo libro al mismo tiempo — ella te da una copia, sin explicación.
- Desarrollo: Evento narrativo largo con intercambios lentos durante varios días.
- Resultado: Desbloquea Acto 4 anticipado si Confianza >65. Nuevo nivel de diálogos.
- Consecuencia: Si el jugador no sigue el ritmo (no lee), la quest falla silenciosamente.

---

### 5 ACTOS NARRATIVOS — LUNA

**Acto 1 (Afecto ≥20): "El libro"**
Trigger: Afecto 20. Sala, tarde.

Desarrollo: Sin aviso, te deja un libro en la mesa mientras pasas. Lo mira. Te mira. Sigue caminando. El libro no tiene nota. Si lo agarras y lees antes de 2 días: siguiente diálogo desbloqueado. Si no lo lees: el libro desaparece sin comentario.

Si lo lees y le dices algo real: Luna levanta la vista. Te escucha. Asiente una vez. *"Bien."* Vuelve a leer. Para Luna eso es una conversación completa.

Decisiones:
- Romántico: Decirle lo que realmente te provocó el libro. → +15 Confianza +10 Afecto
- *"Estuvo bien."* → *"¿Solo bien?"* Si respondes honestamente: +10 Confianza
- No leerlo: -5 Afecto silencioso.

---

**Acto 2 (Afecto ≥40): "La madrugada"**
Trigger: Afecto 40. Jugador activo en madrugada (cocina o balcón).

Desarrollo: Son las 3am. Luna en la cocina con té frío que ya no está tomando. *"¿No puedes dormir?"* No es pregunta — es observación. Se queda en silencio. Tú también. A veces el silencio no es incómodo.

Decisiones:
- Sentarte sin decir nada. → 10 minutos en silencio. Ella: *"Gracias."* +15 Confianza
- Romántico: *"¿Qué estás pensando?"* → Pausa larga. Te cuenta algo pequeño pero real. +12 Afecto +10 Confianza
- Intentar hacer conversación normal. → Se levanta y se va amablemente. +3 Afecto (no funciona con Luna)

Consecuencias: Desbloquea mensajes de madrugada de Luna.

---

**Acto 3 (Afecto ≥60): "El cuaderno"**
Trigger: Afecto 60. Jugador entra al balcón de noche y Luna está ahí.

Desarrollo: Está escribiendo en su cuaderno negro. Al verte, no lo cierra — eso es nuevo. En algún momento del intercambio, sin querer, ves una página. Tu nombre. Escrito varias veces. Ella sabe que lo viste. No dice nada. Sigue escribiendo.

Decisiones:
- Respetuoso: No lo mencionas. Conversación normal. → +20 Confianza. Luna baja la guardia notablemente.
- Romántico: *"No tenía que ver eso."* → *"No."* Pausa. *"Pero ya lo viste."* +15 Confianza +10 Afecto
- Atrevido: *"¿Cuántas veces está?"* → Cierra el cuaderno. Sonrisa apenas. +15 Corrupción -5 Confianza (demasiado directo para ella)

Consecuencias: Desbloquea secreto 1 de Luna. Nuevos diálogos con referencia indirecta al cuaderno.

---

**Acto 4 (Afecto ≥80): "Una frase"**
Trigger: Afecto 80 + Confianza >65. Sala, noche.

Desarrollo: Está leyendo. Cuando te sientas cerca, después de un rato, sin levantar la vista:
*"Hay una frase en este libro que no puedo sacarme de la cabeza."*
Pausa.
*"'Algunas personas entran tan despacio que no te das cuenta hasta que ya no puedes imaginar el cuarto sin ellas.'"*
Sigue leyendo. No dice nada más.

Decisiones:
- No decir nada. Quedarte ahí. → Al rato levanta la vista. Te mira. Asiente apenas. +15 Afecto +15 Confianza
- Romántico: *"¿Hablas de alguien en particular?"* → *"Tú qué crees."* +20 Afecto +10 Corrupción
- *"Buen libro."* → *"Sí."* Fin de conversación. +5 Afecto (perdiste el momento)

---

**Acto 5 (Afecto ≥100): "En voz alta"**
Trigger: Afecto 100 + Corrupción >65 + Confianza >75.

Desarrollo: Te busca. Por primera vez activamente, deliberadamente. Toca la puerta de tu cuarto. *"¿Puedo entrar?"* Cuando entras se queda parada. Te mira.
*"Voy a decir algo en voz alta porque si no lo digo lo voy a seguir escribiendo en el cuaderno para siempre."*
Pausa larga.
*"Te quiero. De esa forma. Desde hace mucho."*

Decisiones:
- Romántico: *"Yo también."* Sin más. → Escena íntima lenta, silenciosa, cargada.
- *"¿Desde cuándo?"* → *"Bachillerato."* Pausa. *"¿Eso es un problema?"* → Escena íntima
- Atrevido: Acercarte sin decir nada. Ella no retrocede. → Escena íntima más intensa.

**Escena adulta:** Completa y explícita. Tono de Luna — en silencio casi total, solo respiración y contacto. Una frase al final, una sola: *"Ya."*
Consecuencias: Relación entra en nuevo estado. Nuevas interacciones libres. Celos activos en Celine y Roxy.

---

## 6. DINÁMICAS ENTRE PERSONAJES

### Celine ↔ Roxy
Tensión directa y verbal. Se conocen demasiado bien para ser completamente amigas.
Roxy dice lo que Celine piensa. Celine dice lo que Roxy no quiere escuchar. Se respetan. No lo admiten.

Si el jugador sube Afecto con Roxy mientras Celine está alta:
Celine se vuelve más fría durante 2-3 días. No lo menciona.

### Celine ↔ Luna
Alianza silenciosa. Se protegen mutuamente sin decirlo.
Si alguien lastima a una, la otra lo sabe antes de que se diga.
Comparten algo del protagonista que ninguna verbaliza.

### Roxy ↔ Luna
Complicidad secreta. Roxy habla, Luna escucha.
Roxy le confía cosas que no le dice a nadie más. Luna guarda todo.
El jugador puede descubrir este vínculo explorando — tiene consecuencias en las historias de ambas.

### Las tres juntas
Armonía base: 70 al inicio (llevan tiempo conviviendo bien).
El jugador puede subirla o bajarla con sus decisiones.

| Nivel Armonía | Efecto |
|---------------|--------|
| >70 | Eventos grupales frecuentes. Ambiente cálido. Interactúan naturalmente. |
| 40-70 | Normal. Sin efectos especiales. |
| <40 | Algunas se evitan. Menos eventos disponibles. Diálogos más tensos. |
| <20 | Algunas se niegan a estar en el mismo cuarto. Bloquea eventos grupales. |

---

## 7. EVENTOS DE TEMPORADA

| Evento | Día del juego | Condición | Descripción |
|--------|--------------|-----------|-------------|
| Cumpleaños Celine | Día 15 | Afecto >50 | Momento único — le preparas algo. Reacción según Afecto. |
| Cumpleaños Roxy | Día 28 | Siempre | Fiesta en el apartamento. Evento grupal. |
| Cumpleaños Luna | Día 42 | Afecto >40 | Momento íntimo y silencioso si estás solo con ella. |
| San Valentín | Día 35 | — | Evento con quien tengas más Afecto. Las otras notan. |
| Noche de películas | Cada viernes | Armonía >50 | Evento grupal sala. Dinámicas según Afecto y Celos. |
| Primera lluvia | Día 7 | — | Evento espontáneo global. Todas en casa. |

---

## 8. EVENTOS GRUPALES

### "La noche de los tres" (cumpleaños Roxy expandido)
Condición: Afecto >50 con las tres + Armonía >65 + Corrupción >40 con al menos una.
Descripción: La fiesta termina. Solo están ustedes cuatro. El ambiente cambia. Las dinámicas de celos y afinidad determinan cómo avanza la noche.

### "La lluvia larga"
Condición: Evento lluvia activo + Armonía >60 + Afecto >40 con las tres.
Descripción: Quedan atrapados en casa todo el día. Conversación grupal real. Cada una revela algo sin querer.

### "La película que nadie planeó"
Condición: Viernes noche + Armonía >70 + al menos una en estado Feliz o Traviesa.
Descripción: Terminan viendo algo juntos. Dónde se sienta cada una importa.

---

*Documento generado por el Arquitecto — Apartment Sim v1.0 RPG Maker MZ*
*Siguiente documento: GDD_SISTEMAS.md*
