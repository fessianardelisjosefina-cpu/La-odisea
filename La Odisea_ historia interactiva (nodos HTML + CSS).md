# La Odisea: la venganza de Ítaca

Proyecto de historia interactiva no lineal. Cada nodo es un documento HTML con su propio CSS, y todos comparten `base.css`.

## Estructura de archivos

- `base.css`: sistema de diseño compartido (tipografías, paleta, paneles, cursor, animaciones, responsive).
- `n01.html` a `n11.html` y `f01.html`: nodos de la historia.
- `n01.css` a `n11.css` y `f01.css`: CSS de cada nodo. Importan `base.css` y definen el fondo de su escena.

### Mapa de nodos

| Nivel | Nodos | Escena |
|---|---|---|
| 1 · El viaje comienza | n01, f01 | Regreso a Ítaca |
| 2 · Los peligros del viaje | n02 a n07 | Palacio, pretendientes, sirvientes, duelo |
| 3 · El destino y el regreso | n08 a n11 | Prueba final, Telémaco, revelación, victoria |

Las rutas son: n01 → n02 o f01; n02 → n03 o n04; n03 → n04 o n05; n04 → n05 o n06; n05 → n06 o n07; n06 → n07; n07 → n08; n08 → n09; n09 → n10; n10 → n11; n11 → n01 o cierre.

---

## base.css

```css
@import url("https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Titillium+Web:ital,wght@0,200;0,300;0,400;0,600;0,700;0,900;1,400&family=Red+Hat+Display:wght@400;600&display=swap");

:root {
  --brown: #664135;
  --gold: #C6A15B;
  --black: #171311;
  --ivory: #F3EBDD;
  --navy: #173B4A;
  --mist: #82969A;

  --panel: rgba(102, 65, 53, 0.78);
  --shadow-out: 0 12px 30px rgba(23, 19, 17, 0.6);
  --shadow-in: inset 0 0 18px rgba(0, 0, 0, 0.35);
  --ease: 1.2s ease;

  /* Se sobrescribe en cada nodo: gradiente o imagen */
  --scene-bg: linear-gradient(180deg, #173B4A 0%, #82969A 55%, #171311 100%);

  /* Cursor personalizado: arco negro con flecha negra (SVG inline) */
  --cursor: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='32' height='32' viewBox='0 0 32 32'><path d='M10 3 Q-2 16 10 29' fill='none' stroke='black' stroke-width='2.5'/><line x1='10' y1='16' x2='30' y2='16' stroke='black' stroke-width='2'/><path d='M30 16 l-6 -4 v8 z' fill='black'/></svg>") 4 4, auto;
}

*, *::before, *::after { box-sizing: border-box; }

html, body {
  margin: 0;
  height: 100%;
  background: var(--black);
  color: var(--ivory);
}

body { cursor: var(--cursor); }
a, .choice { cursor: var(--cursor); }

/* Escena */
.scene {
  position: relative;
  min-height: 100vh;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 6vh 5vw;
}

.scene::before {
  content: "";
  position: absolute;
  inset: 0;
  background: rgba(23, 19, 17, 0.45);
  z-index: 1;
  pointer-events: none;
}

/* Capas de fondo */
.layer {
  position: absolute;
  inset: 0;
  z-index: 0;
  pointer-events: none;
}

.layer-bg {
  background-image: var(--scene-bg);
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  opacity: 0.6;
  transform: scale(1.05);
  animation: drift 30s ease-in-out infinite alternate;
}

.layer-fog {
  background: radial-gradient(ellipse at 50% 80%, rgba(130, 150, 154, 0.35), transparent 60%);
  animation: fog 14s ease-in-out infinite alternate;
}

.layer-video {
  width: 100%;
  height: 100%;
  border: 0;
  transform: scale(1.25);
  opacity: 0.45;
  filter: saturate(0.7);
}

/* Historia */
.story {
  position: relative;
  z-index: 2;
  width: min(760px, 100%);
  text-align: center;
  animation: rise 1.4s ease both;
}

h1 {
  font-family: "Bebas Neue", sans-serif;
  font-weight: 400;
  font-style: normal;
  font-size: clamp(3.2rem, 9vw, 6.5rem);
  letter-spacing: 0.08em;
  line-height: 0.95;
  margin: 0.2em 0;
  color: var(--ivory);
  text-shadow: 0 6px 18px rgba(0, 0, 0, 0.7);
}

h2 {
  font-family: "Titillium Web", sans-serif;
  font-weight: 600;
  font-style: normal;
  font-size: clamp(1.1rem, 2.6vw, 1.5rem);
  color: var(--gold);
  margin: 0.4em 0 1em;
  text-shadow: 0 2px 8px rgba(0, 0, 0, 0.8);
}

p {
  font-family: "Red Hat Display", sans-serif;
  font-optical-sizing: auto;
  font-weight: 400;
  font-style: normal;
  line-height: 1.7;
  font-size: clamp(1rem, 1.8vw, 1.15rem);
  margin: 0 0 1em;
}

.level {
  font-family: "Titillium Web", sans-serif;
  font-weight: 300;
  letter-spacing: 0.3em;
  text-transform: uppercase;
  color: var(--mist);
  font-size: 0.8rem;
}

/* Paneles narrativos */
.panel {
  background: var(--panel);
  border: 1px solid var(--gold);
  border-radius: 14px;
  padding: clamp(1.2rem, 4vw, 2.4rem);
  box-shadow: var(--shadow-out), var(--shadow-in);
  margin: 1.5rem auto;
  text-align: left;
  animation: float 6s ease-in-out infinite;
}

.panel p.quote {
  font-family: "Titillium Web", sans-serif;
  font-style: italic;
  font-weight: 300;
  color: var(--gold);
}

/* Decisiones */
.choices {
  display: grid;
  gap: 0.9rem;
  margin-top: 1.5rem;
}

.choice {
  display: block;
  padding: 0.9rem 1.2rem;
  background: rgba(23, 19, 17, 0.7);
  border: 1px solid var(--gold);
  border-radius: 999px;
  color: var(--ivory);
  text-decoration: none;
  font-family: "Titillium Web", sans-serif;
  font-weight: 600;
  transition: transform 0.3s, background 0.3s, box-shadow 0.3s;
}

.choice:hover,
.choice:focus-visible {
  background: var(--brown);
  transform: translateY(-2px);
  box-shadow: 0 0 18px rgba(198, 161, 91, 0.45);
  outline: none;
}

/* Animaciones */
@keyframes drift {
  from { transform: scale(1.05) translateX(-1%); }
  to   { transform: scale(1.1) translateX(1%); }
}

@keyframes fog {
  from { opacity: 0.4; }
  to   { opacity: 0.9; }
}

@keyframes rise {
  from { opacity: 0; transform: translateY(30px); }
  to   { opacity: 1; transform: none; }
}

@keyframes float {
  0%, 100% { transform: translateY(0); }
  50%      { transform: translateY(-4px); }
}

/* Responsive */
@media (max-width: 600px) {
  .scene { align-items: flex-start; padding: 4vh 4vw; }
  .choice { padding: 0.8rem 1rem; }
}

/* Dispositivos táctiles: cursor nativo */
@media (hover: none) {
  body, a, .choice { cursor: auto; }
}

/* Reducción de movimiento */
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation: none !important;
    transition: none !important;
  }
}
```

---

## Nivel 1

### n01.html

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Nodo 01 · Regreso a Ítaca</title>
  <link rel="stylesheet" href="n01.css">
</head>
<body>
  <main class="scene">
    <div class="layer layer-bg"></div>
    <div class="layer layer-fog"></div>
    <section class="story">
      <p class="level">Nivel 1 · El viaje comienza</p>
      <h1>Regreso a Ítaca</h1>
      <h2>Odiseo pisa su tierra después de veinte años</h2>
      <div class="panel">
        <p>Tras la guerra de Troya, Odiseo despierta en la costa de Ítaca. Atenea aparece entre la niebla y le advierte que su palacio está lleno de enemigos. Si se presenta como rey, lo atacarán sin piedad.</p>
        <p class="quote">"Disfrázate de mendigo, Odiseo, y mira con tus propios ojos."</p>
      </div>
      <nav class="choices">
        <a class="choice" href="n02.html">Escuchar a Atenea y disfrazarte de mendigo</a>
        <a class="choice" href="f01.html">Revelarte ya ante los pretendientes</a>
      </nav>
    </section>
  </main>
</body>
</html>
```

### n01.css

```css
@import url("base.css");

:root {
  /* Reemplazar por la imagen adjunta del barco, por ejemplo: url("barco.jpg") */
  --scene-bg: linear-gradient(180deg, #173B4A 0%, #82969A 55%, #171311 100%);
}
```

### f01.html (final alternativo)

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Final alternativo · Emboscada</title>
  <link rel="stylesheet" href="f01.css">
</head>
<body>
  <main class="scene">
    <div class="layer layer-bg"></div>
    <div class="layer layer-fog"></div>
    <section class="story">
      <p class="level">Final alternativo</p>
      <h1>La emboscada</h1>
      <h2>Tu orgullo te delata</h2>
      <div class="panel">
        <p>Los pretendientes te rodean antes de que puedas tensar el arco. Sin un plan ni aliados, el regreso termina aquí. Atenea se aparta, y la historia se reinicia.</p>
      </div>
      <nav class="choices">
        <a class="choice" href="n01.html">Volver a empezar</a>
      </nav>
    </section>
  </main>
</body>
</html>
```

### f01.css

```css
@import url("base.css");

:root {
  --scene-bg: linear-gradient(180deg, #171311 0%, #664135 100%);
}
```

---

## Nivel 2

### n02.html

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Nodo 02 · Los pretendientes</title>
  <link rel="stylesheet" href="n02.css">
</head>
<body>
  <main class="scene">
    <div class="layer layer-bg"></div>
    <div class="layer layer-fog"></div>
    <section class="story">
      <p class="level">Nivel 2 · Los peligros del viaje</p>
      <h1>El palacio</h1>
      <h2>Odiseo observa a los pretendientes</h2>
      <div class="panel">
        <p>Entre los mendigos del palacio, Odiseo ve a los pretendientes beber sin medida, reír a costa de los bienes de Ítaca y acosar a Penélope. Todos esperan ocupar el trono de su esposo.</p>
      </div>
      <nav class="choices">
        <a class="choice" href="n03.html">Observar al pretendiente más arrogante</a>
        <a class="choice" href="n04.html">Buscar en silencio a Telémaco</a>
      </nav>
    </section>
  </main>
</body>
</html>
```

### n02.css

```css
@import url("base.css");

:root {
  --scene-bg: url("https://viapais.com.ar/images/2026/09/23/FXAZpbTaWe_0x1000__detail.jpg");
}
```

### n03.html

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Nodo 03 · El plan de Telémaco</title>
  <link rel="stylesheet" href="n03.css">
</head>
<body>
  <main class="scene">
    <div class="layer layer-bg"></div>
    <div class="layer layer-fog"></div>
    <section class="story">
      <p class="level">Nivel 2 · Los peligros del viaje</p>
      <h1>Un plan</h1>
      <h2>Telémaco reconoce a su padre</h2>
      <div class="panel">
        <p>Telémaco entiende que su padre está vivo. Juntos congenian un plan: primero reducir la fuerza de los pretendientes, y después recuperar el palacio sin levantar sospechas.</p>
      </div>
      <nav class="choices">
        <a class="choice" href="n05.html">Aceptar el plan de Telémaco</a>
        <a class="choice" href="n04.html">Pedir primero la ayuda de Penélope</a>
      </nav>
    </section>
  </main>
</body>
</html>
```

### n03.css

```css
@import url("base.css");

:root {
  --scene-bg: url("https://viapais.com.ar/images/2026/09/23/FXAZpbTaWe_0x1000__detail.jpg");
}
```

### n04.html

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Nodo 04 · Penélope y sus sirvientes</title>
  <link rel="stylesheet" href="n04.css">
</head>
<body>
  <main class="scene">
    <div class="layer layer-bg"></div>
    <div class="layer layer-fog"></div>
    <section class="story">
      <p class="level">Nivel 2 · Los peligros del viaje</p>
      <h1>Penélope</h1>
      <h2>La reina busca aliados fieles</h2>
      <div class="panel">
        <p>Cansada de la presión, Penélope reúne a sus sirvientes más leales. Les pide que juren lealtad a Telémaco, incluso si ella acepta casarse con otro hombre. Así protege el derecho de su hijo.</p>
        <p class="quote">"Juren que servirán a mi hijo, pase lo que pase conmigo."</p>
      </div>
      <nav class="choices">
        <a class="choice" href="n05.html">Pedir juramento de lealtad</a>
        <a class="choice" href="n06.html">Esperar en silencio su decisión</a>
      </nav>
    </section>
  </main>
</body>
</html>
```

### n04.css

```css
@import url("base.css");

:root {
  --scene-bg: url("https://viapais.com.ar/images/2026/09/23/FXAZpbTaWe_0x1000__detail.jpg");
}
```

### n05.html

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Nodo 05 · Los sirvientes reconocen a Odiseo</title>
  <link rel="stylesheet" href="n05.css">
</head>
<body>
  <main class="scene">
    <div class="layer layer-bg"></div>
    <div class="layer layer-fog"></div>
    <section class="story">
      <p class="level">Nivel 2 · Los peligros del viaje</p>
      <h1>Reconocimiento</h1>
      <h2>Los fieles ven al rey</h2>
      <div class="panel">
        <p>Un viejo sirviente reconoce a Odiseo bajo el disfraz. Las lágrimas llegan antes que las palabras. Sin perder tiempo, los sirvientes empiezan a llevar las armas del palacio a los lugares donde su señor las necesitará.</p>
      </div>
      <nav class="choices">
        <a class="choice" href="n06.html">Preparar las armas del palacio</a>
        <a class="choice" href="n07.html">Guardar el secreto hasta el duelo</a>
      </nav>
    </section>
  </main>
</body>
</html>
```

### n05.css

```css
@import url("base.css");

:root {
  --scene-bg: url("https://viapais.com.ar/images/2026/09/23/FXAZpbTaWe_0x1000__detail.jpg");
}
```

### n06.html

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Nodo 06 · El banquete de los pretendientes</title>
  <link rel="stylesheet" href="n06.css">
</head>
<body>
  <main class="scene">
    <div class="layer layer-bg"></div>
    <div class="layer layer-fog"></div>
    <section class="story">
      <p class="level">Nivel 2 · Los peligros del viaje</p>
      <h1>El banquete</h1>
      <h2>Los bienes de Ítaca se derrochan</h2>
      <div class="panel">
        <p>Los pretendientes ríen y beben sin fin, ignorando las leyes de Zeus sobre la hospitalidad. Cada uno cree que será el próximo rey, y ninguno sospecha que el mendigo lo observa todo.</p>
      </div>
      <nav class="choices">
        <a class="choice" href="n07.html">Observar su arrogancia en silencio</a>
        <a class="choice" href="n07.html">Acercarte a Penélope con una excusa</a>
      </nav>
    </section>
  </main>
</body>
</html>
```

### n06.css

```css
@import url("base.css");

:root {
  --scene-bg: url("https://viapais.com.ar/images/2026/09/23/FXAZpbTaWe_0x1000__detail.jpg");
}
```

### n07.html

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Nodo 07 · El duelo del arco</title>
  <link rel="stylesheet" href="n07.css">
</head>
<body>
  <main class="scene">
    <div class="layer layer-bg"></div>
    <div class="layer layer-fog"></div>
    <section class="story">
      <p class="level">Nivel 2 · Los peligros del viaje</p>
      <h1>El desafío</h1>
      <h2>Penélope pone sus condiciones</h2>
      <div class="panel">
        <p>Penélope anuncia un torneo: quien tense el arco de Odiseo y dispare a través de doce anillos de hierro será su esposo y rey de Ítaca. Los pretendientes aceptan con arrogancia.</p>
        <p class="quote">"Quien logre tensar el arco de mi esposo, gobernará Ítaca."</p>
      </div>
      <nav class="choices">
        <a class="choice" href="n08.html">Aceptar que comience la prueba</a>
        <a class="choice" href="n08.html">Exigir que el reto sea justo</a>
      </nav>
    </section>
  </main>
</body>
</html>
```

### n07.css

```css
@import url("base.css");

:root {
  --scene-bg: url("https://viapais.com.ar/images/2026/09/23/FXAZpbTaWe_0x1000__detail.jpg");
}
```

---

## Nivel 3

### n08.html

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Nodo 08 · La prueba del arco</title>
  <link rel="stylesheet" href="n08.css">
</head>
<body>
  <main class="scene">
    <iframe class="layer layer-video" src="https://www.youtube.com/embed/FuHjPTW2bVQ?autoplay=1&mute=1&loop=1&playlist=FuHjPTW2bVQ&controls=0" allow="autoplay; encrypted-media" title="Fondo ambiental" tabindex="-1"></iframe>
    <div class="layer layer-fog"></div>
    <section class="story">
      <p class="level">Nivel 3 · El destino y el regreso</p>
      <h1>La prueba</h1>
      <h2>Uno tras otro, los pretendientes fallan</h2>
      <div class="panel">
        <p>Ninguno de los pretendientes logra tensar el arco. Entonces un mendigo de aspecto humilde pide intentarlo. Las risas se apagan cuando el arco cede en sus manos.</p>
      </div>
      <nav class="choices">
        <a class="choice" href="n09.html">Permitir que el mendigo lo intente</a>
        <a class="choice" href="n09.html">Intervenir para proteger al mendigo</a>
      </nav>
    </section>
  </main>
</body>
</html>
```

### n08.css

```css
@import url("base.css");

:root {
  --scene-bg: none;
}

.layer-video { opacity: 0.45; }
```

### n09.html

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Nodo 09 · Telémaco lucha</title>
  <link rel="stylesheet" href="n09.css">
</head>
<body>
  <main class="scene">
    <iframe class="layer layer-video" src="https://www.youtube.com/embed/FuHjPTW2bVQ?autoplay=1&mute=1&loop=1&playlist=FuHjPTW2bVQ&controls=0" allow="autoplay; encrypted-media" title="Fondo ambiental" tabindex="-1"></iframe>
    <div class="layer layer-fog"></div>
    <section class="story">
      <p class="level">Nivel 3 · El destino y el regreso</p>
      <h1>La batalla</h1>
      <h2>Telémaco entra en combate</h2>
      <div class="panel">
        <p>Telémaco toma una lanza y se lanza contra los pretendientes. Cada golpe reduce su número y acerca la victoria.</p>
      </div>
      <nav class="choices">
        <a class="choice" href="n10.html">Tomar tu arco y unirte a la lucha</a>
        <a class="choice" href="n10.html">Esperar el momento preciso</a>
      </nav>
    </section>
  </main>
</body>
</html>
```

### n09.css

```css
@import url("base.css");

:root {
  --scene-bg: none;
}
```

### n10.html

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Nodo 10 · La revelación</title>
  <link rel="stylesheet" href="n10.css">
</head>
<body>
  <main class="scene">
    <iframe class="layer layer-video" src="https://www.youtube.com/embed/FuHjPTW2bVQ?autoplay=1&mute=1&loop=1&playlist=FuHjPTW2bVQ&controls=0" allow="autoplay; encrypted-media" title="Fondo ambiental" tabindex="-1"></iframe>
    <div class="layer layer-fog"></div>
    <section class="story">
      <p class="level">Nivel 3 · El destino y el regreso</p>
      <h1>La revelación</h1>
      <h2>El mendigo se quita el disfraz</h2>
      <div class="panel">
        <p>Odiseo deja caer su capa de mendigo. Su voz y su figura son inconfundibles. Los pretendientes palidecen, y Penélope lo reconoce al fin.</p>
        <p class="quote">"Soy Odiseo, y esta es mi casa."</p>
      </div>
      <nav class="choices">
        <a class="choice" href="n11.html">Revelar tu identidad a todos</a>
        <a class="choice" href="n11.html">Pedir a Penélope que te reconozca primero</a>
      </nav>
    </section>
  </main>
</body>
</html>
```

### n10.css

```css
@import url("base.css");

:root {
  --scene-bg: none;
}
```

### n11.html

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Nodo 11 · El regreso a casa</title>
  <link rel="stylesheet" href="n11.css">
</head>
<body>
  <main class="scene">
    <iframe class="layer layer-video" src="https://www.youtube.com/embed/FuHjPTW2bVQ?autoplay=1&mute=1&loop=1&playlist=FuHjPTW2bVQ&controls=0" allow="autoplay; encrypted-media" title="Fondo ambiental" tabindex="-1"></iframe>
    <div class="layer layer-fog"></div>
    <section class="story">
      <p class="level">Nivel 3 · El destino y el regreso</p>
      <h1>Ítaca</h1>
      <h2>Odiseo y Telémaco recuperan su hogar</h2>
      <div class="panel">
        <p>Los pretendientes caen y el palacio vuelve a ser de su legítimo rey. Penélope, Telémaco y Odiseo se reencuentran bajo el mismo techo. Ítaca vuelve a respirar.</p>
        <p class="quote">"Por fin, el hogar."</p>
      </div>
      <nav class="choices">
        <a class="choice" href="n01.html">Volver a empezar la historia</a>
      </nav>
    </section>
  </main>
</body>
</html>
```

### n11.css

```css
@import url("base.css");

:root {
  --scene-bg: none;
}
```

---

## Versión de desplazamiento vertical

Un solo documento que recorre las 10 etapas al bajar la página. Cada etapa aparece con `IntersectionObserver`, el fondo tiene parallax y algunas etapas ofrecen una decisión cuya consecuencia se revela en la misma página. Usa `odisea-scroll.html` y `odisea-scroll.css`.

### odisea-scroll.html

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>La Odisea · Desplazamiento</title>
  <link rel="stylesheet" href="odisea-scroll.css">
</head>
<body>

<section class="stage stage--sea" data-stage>
  <div class="bg"></div>
  <div class="content reveal">
    <p class="level">Nivel 1 · El viaje comienza</p>
    <h1>La partida de Troya</h1>
    <h2>El inicio del regreso</h2>
    <div class="panel"><p>Tras la guerra, Odiseo emprende el viaje de vuelta a Ítaca. El mar se abre ante él lleno de promesas y de peligros.</p></div>
  </div>
</section>

<section class="stage stage--sea" data-stage>
  <div class="bg"></div>
  <div class="content reveal">
    <h1>Los cíclopes</h1>
    <h2>La astucia frente a la fuerza</h2>
    <div class="panel"><p>Polifemo atrapa a los compañeros de Odiseo en su cueva. El héroe se presenta como «Nadie» y ciega al gigante.</p>
      <p class="quote">"Me llamo Nadie."</p></div>
  </div>
</section>

<section class="stage stage--sea" data-stage>
  <div class="bg"></div>
  <div class="content reveal">
    <h1>Los vientos de Eolo</h1>
    <h2>Esperanza y pérdida</h2>
    <div class="panel"><p>Eolo entrega a Odiseo los vientos atados en un odre. Cuando los marineros lo abren por curiosidad, la tempestad los devuelve al mar.</p></div>
  </div>
</section>

<section class="stage stage--image" data-stage>
  <div class="bg"></div>
  <div class="content reveal">
    <p class="level">Nivel 2 · Los peligros del viaje</p>
    <h1>Circe</h1>
    <h2>La hechicera de la isla</h2>
    <div class="panel"><p>Circe convierte a parte de la tripulación en cerdos. Odiseo acude con la protección del dios Hermes.</p>
      <div class="choices">
        <button class="choice" data-consequence="c4a">Beber la poción sin desconfiar</button>
        <button class="choice" data-consequence="c4b">Resistir el hechizo con la hierba moly</button>
      </div>
      <p class="consequence" id="c4a" hidden>Circe te convierte en su huésped durante un año. Tus hombres quedan atrapados con ella.</p>
      <p class="consequence" id="c4b" hidden>El hechizo no te afecta. Circe, sorprendida, jura no volver a dañarte.</p>
    </div>
  </div>
</section>

<section class="stage stage--image" data-stage>
  <div class="bg"></div>
  <div class="content reveal">
    <h1>El inframundo</h1>
    <h2>Los muertos hablan</h2>
    <div class="panel"><p>En el Hades, Odiseo escucha a Tiresias, que le revela cómo volver a Ítaca y le advierte sobre el peligro que corren sus compañeros.</p>
      <p class="quote">"Nadie vuelve sin pagar el precio del mar."</p></div>
  </div>
</section>

<section class="stage stage--image" data-stage>
  <div class="bg"></div>
  <div class="content reveal">
    <h1>Las sirenas</h1>
    <h2>El canto que seduce</h2>
    <div class="panel"><p>Odiseo manda atarse al mástil y tapar los oídos de sus hombres con cera. Quiere escuchar el canto sin dejarse arrastrar.</p>
      <div class="choices">
        <button class="choice" data-consequence="c6a">Ordenar a la tripulación que no te desate</button>
        <button class="choice" data-consequence="c6b">Pedir que te desaten si te ves perdido</button>
      </div>
      <p class="consequence" id="c6a" hidden>Escuchas el canto sin perder el rumbo y sobrevives con tu tripulación.</p>
      <p class="consequence" id="c6b" hidden>Suplicas por la libertad, pero la tripulación te obedece tarde. Pierdes un momento precioso.</p>
    </div>
  </div>
</section>

<section class="stage stage--image" data-stage>
  <div class="bg"></div>
  <div class="content reveal">
    <h1>Escila y Caribdis</h1>
    <h2>Entre dos amenazas</h2>
    <div class="panel"><p>Un estrecho separa a un monstruo de seis cabezas y a un remolino que traga barcos enteros. No hay manera de evitar pérdidas.</p>
      <div class="choices">
        <button class="choice" data-consequence="c7a">Pasar cerca de Escila y salvar el barco</button>
        <button class="choice" data-consequence="c7b">Arriesgarte al remolino de Caribdis</button>
      </div>
      <p class="consequence" id="c7a" hidden>Escila se lleva a seis hombres, pero el resto de la tripulación llega a salvo.</p>
      <p class="consequence" id="c7b" hidden>Caribdis se traga el barco entero. Solo sobrevives aferrado a una higuera.</p>
    </div>
  </div>
</section>

<section class="stage stage--video" data-stage>
  <iframe class="bg video" src="https://www.youtube.com/embed/FuHjPTW2bVQ?autoplay=1&mute=1&loop=1&playlist=FuHjPTW2bVQ&controls=0" allow="autoplay; encrypted-media" title="Fondo ambiental" tabindex="-1"></iframe>
  <div class="content reveal">
    <p class="level">Nivel 3 · El destino y el regreso</p>
    <h1>Calipso</h1>
    <h2>La nostalgia de Ítaca</h2>
    <div class="panel"><p>Durante siete años, la ninfa Calipso retiene a Odiseo. Cada amanecer él mira el mar hacia su hogar.</p>
      <p class="quote">"Ítaca es pequeña, pero es mi tierra."</p></div>
  </div>
</section>

<section class="stage stage--video" data-stage>
  <iframe class="bg video" src="https://www.youtube.com/embed/FuHjPTW2bVQ?autoplay=1&mute=1&loop=1&playlist=FuHjPTW2bVQ&controls=0" allow="autoplay; encrypted-media" title="Fondo ambiental" tabindex="-1"></iframe>
  <div class="content reveal">
    <h1>La isla del Sol</h1>
    <h2>Las consecuencias de desobedecer</h2>
    <div class="panel"><p>Hambrientos, sus compañeros sacrifican el ganado sagrado del dios Sol. Zeus responde con una tormenta que destruye la nave.</p></div>
  </div>
</section>

<section class="stage stage--video" data-stage>
  <iframe class="bg video" src="https://www.youtube.com/embed/FuHjPTW2bVQ?autoplay=1&mute=1&loop=1&playlist=FuHjPTW2bVQ&controls=0" allow="autoplay; encrypted-media" title="Fondo ambiental" tabindex="-1"></iframe>
  <div class="content reveal">
    <h1>El regreso a Ítaca</h1>
    <h2>El hogar reconocido</h2>
    <div class="panel"><p>Llevado por los feacios a la costa de Ítaca, Odiseo despierta sin reconocer su tierra. Atenea le revela dónde está.</p>
      <p class="quote">"Por fin, el hogar."</p></div>
  </div>
</section>

<script>
  // Aparición al entrar en pantalla
  const io = new IntersectionObserver((entries) => {
    entries.forEach((e) => {
      if (e.isIntersecting) { e.target.classList.add('in'); io.unobserve(e.target); }
    });
  }, { threshold: 0.2 });
  document.querySelectorAll('.reveal').forEach((el) => io.observe(el));

  // Parallax del fondo
  const stages = [...document.querySelectorAll('[data-stage]')];
  const reduce = matchMedia('(prefers-reduced-motion: reduce)').matches;
  let ticking = false;
  function parallax() {
    if (!reduce) {
      stages.forEach((s) => {
        const r = s.getBoundingClientRect();
        if (r.bottom < 0 || r.top > innerHeight) return;
        const offset = (r.top / innerHeight) * -40;
        s.querySelector('.bg').style.transform = `translate3d(0, ${offset}px, 0) scale(1.1)`;
      });
    }
    ticking = false;
  }
  addEventListener('scroll', () => {
    if (!ticking) { requestAnimationFrame(parallax); ticking = true; }
  }, { passive: true });
  parallax();

  // Decisiones: muestran la consecuencia dentro de la misma etapa
  document.querySelectorAll('[data-consequence]').forEach((btn) => {
    btn.addEventListener('click', () => {
      const target = document.getElementById(btn.dataset.consequence);
      const group = btn.closest('.choices');
      group.querySelectorAll('.choice').forEach((b) => b.classList.remove('selected'));
      btn.classList.add('selected');
      group.parentElement.querySelectorAll('.consequence').forEach((p) => p.hidden = (p !== target));
    });
  });
</script>
</body>
</html>
```

### odisea-scroll.css

```css
@import url("https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Titillium+Web:ital,wght@0,200;0,300;0,400;0,600;0,700;0,900;1,400&family=Red+Hat+Display:wght@400;600&display=swap");

:root {
  --brown: #664135;
  --gold: #C6A15B;
  --black: #171311;
  --ivory: #F3EBDD;
  --navy: #173B4A;
  --mist: #82969A;
  --panel: rgba(102, 65, 53, 0.78);
  --shadow-out: 0 12px 30px rgba(23, 19, 17, 0.6);
  --shadow-in: inset 0 0 18px rgba(0, 0, 0, 0.35);
  --cursor: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='32' height='32' viewBox='0 0 32 32'><path d='M10 3 Q-2 16 10 29' fill='none' stroke='black' stroke-width='2.5'/><line x1='10' y1='16' x2='30' y2='16' stroke='black' stroke-width='2'/><path d='M30 16 l-6 -4 v8 z' fill='black'/></svg>") 4 4, auto;
}

*, *::before, *::after { box-sizing: border-box; }

html, body { margin: 0; background: var(--black); color: var(--ivory); }
body { cursor: var(--cursor); }
button.choice { cursor: var(--cursor); }

h1 {
  font-family: "Bebas Neue", sans-serif;
  font-weight: 400;
  font-style: normal;
  font-size: clamp(3.2rem, 9vw, 6.5rem);
  letter-spacing: 0.08em;
  line-height: 0.95;
  margin: 0.2em 0;
  text-transform: uppercase;
  text-shadow: 0 6px 18px rgba(0, 0, 0, 0.7);
}

h2 {
  font-family: "Titillium Web", sans-serif;
  font-weight: 600;
  font-style: normal;
  font-size: clamp(1.1rem, 2.6vw, 1.5rem);
  color: var(--gold);
  margin: 0.4em 0 1em;
  text-shadow: 0 2px 8px rgba(0, 0, 0, 0.8);
}

p {
  font-family: "Red Hat Display", sans-serif;
  font-optical-sizing: auto;
  font-weight: 400;
  font-style: normal;
  line-height: 1.7;
  font-size: clamp(1rem, 1.8vw, 1.15rem);
  margin: 0 0 1em;
}

.level {
  font-family: "Titillium Web", sans-serif;
  font-weight: 300;
  letter-spacing: 0.3em;
  text-transform: uppercase;
  color: var(--mist);
  font-size: 0.8rem;
}

/* Etapas: cada una ocupa al menos una pantalla */
.stage {
  position: relative;
  min-height: 100vh;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 10vh 5vw;
}

.stage::before {
  content: "";
  position: absolute;
  inset: 0;
  background: linear-gradient(180deg, rgba(23, 19, 17, 0.5), rgba(23, 19, 17, 0.75));
  z-index: 1;
  pointer-events: none;
}

/* Fondos por nivel */
.bg {
  position: absolute;
  inset: -8% 0;
  width: 100%;
  height: 116%;
  z-index: 0;
  pointer-events: none;
  background-size: cover;
  background-position: center;
  opacity: 0.6;
  transition: transform 0.2s linear;
}

.stage--sea .bg {
  background-image: linear-gradient(180deg, #173B4A 0%, #82969A 55%, #171311 100%);
}

/* Imagen del nivel 2 */
.stage--image .bg {
  background-image: url("https://viapais.com.ar/images/2026/09/23/FXAZpbTaWe_0x1000__detail.jpg");
}

.video {
  border: 0;
  opacity: 0.45;
  filter: saturate(0.7);
}

/* Contenido */
.content {
  position: relative;
  z-index: 2;
  width: min(760px, 100%);
  text-align: center;
}

/* Aparición progresiva */
.reveal {
  opacity: 0;
  transform: translateY(40px);
  transition: opacity 1.2s ease, transform 1.2s ease;
}

.reveal.in {
  opacity: 1;
  transform: none;
}

/* Paneles */
.panel {
  background: var(--panel);
  border: 1px solid var(--gold);
  border-radius: 14px;
  padding: clamp(1.2rem, 4vw, 2.4rem);
  box-shadow: var(--shadow-out), var(--shadow-in);
  margin: 1.5rem auto;
  text-align: left;
}

.panel p.quote {
  font-family: "Titillium Web", sans-serif;
  font-style: italic;
  font-weight: 300;
  color: var(--gold);
}

/* Decisiones */
.choices {
  display: grid;
  gap: 0.8rem;
  margin-top: 1.2rem;
}

.choice {
  display: block;
  width: 100%;
  padding: 0.9rem 1.2rem;
  background: rgba(23, 19, 17, 0.7);
  border: 1px solid var(--gold);
  border-radius: 999px;
  color: var(--ivory);
  font-family: "Titillium Web", sans-serif;
  font-weight: 600;
  font-size: 1rem;
  text-align: center;
  transition: transform 0.3s, background 0.3s, box-shadow 0.3s;
}

.choice:hover,
.choice:focus-visible,
.choice.selected {
  background: var(--brown);
  transform: translateY(-2px);
  box-shadow: 0 0 18px rgba(198, 161, 91, 0.45);
  outline: none;
}

.consequence {
  margin-top: 1rem;
  padding-left: 0.8rem;
  border-left: 2px solid var(--gold);
  animation: rise 0.8s ease both;
}

@keyframes rise {
  from { opacity: 0; transform: translateY(12px); }
  to   { opacity: 1; transform: none; }
}

/* Móvil */
@media (max-width: 600px) {
  .stage { padding: 8vh 4vw; }
  .panel { margin: 1rem auto; }
}

/* Táctil: cursor nativo */
@media (hover: none) {
  body, button.choice { cursor: auto; }
}

/* Reducción de movimiento */
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation: none !important;
    transition: none !important;
  }
  .reveal { opacity: 1; transform: none; }
}
```

## Notas de implementación

- **Fondo del nivel 1:** no tengo la imagen adjunta del barco. Reemplaza el gradiente de `n01.css` por `url("tu-imagen.jpg")`.
- **Fondo del nivel 2:** usa la URL de imagen que indicaste.
- **Fondo del nivel 3:** usa el video de YouTube como `iframe`. YouTube puede mostrar controles o publicidad que el CSS no puede ocultar del todo.
- **Cursor:** el arco y la flecha son un SVG inline en `base.css`. En dispositivos táctiles se usa el cursor nativo.
- **Accesibilidad:** `prefers-reduced-motion` desactiva las animaciones.
- **Desplazamiento:** esta versión usa un nodo por pantalla con decisiones. Si quieres la versión de desplazamiento vertical con `IntersectionObserver` que describiste, puedo prepararla como siguiente paso.
