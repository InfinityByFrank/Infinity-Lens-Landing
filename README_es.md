# 🌐 Infinity Lens — Landing Page

🌐 [English](README.md) · **Español**

Un sitio web de producto para una app de iPhone, en **seis idiomas**, sin rastreadores y sin servidor. Explica la app en segundos y publica las páginas legales que exige App Store (política de privacidad, términos y soporte).

🔗 Sitio en vivo: [lens.infinitydev.agency](https://lens.infinitydev.agency)
📱 La app que promociona: mira el caso de estudio [Infinity Lens](https://github.com/TheGeistGamer/infinity-lens).

<p align="center">
  <img src="assets/desktop-01-portada.png" width="560" alt="Portada de la landing (escritorio)" />
  <img src="assets/movil-01-portada.png" width="140" alt="Portada de la landing (móvil)" />
</p>

---

## 🎯 Sobre el proyecto

Mi app necesitaba una página antes de existir en la tienda: App Store exige una **política de privacidad pública** y un enlace de soporte, y además quería una portada que explicara la app rápido. Tres condiciones:

1. **Coherente con la promesa de la app:** sin rastreadores ni cookies de seguimiento.
2. **Multilingüe de verdad**, porque la app ya estaba en seis idiomas.
3. **Rápida y barata de mantener:** sin servidor ni base de datos.

## 📈 Resultados medidos

Lighthouse sobre el sitio publicado (escritorio, una corrida, 2026-10-13):

| Rendimiento | Accesibilidad | Buenas prácticas | SEO | Primer contenido visible |
|:-:|:-:|:-:|:-:|:-:|
| **100** | **96** | **96** | **100** | **0.6 s** |

<p align="center"><img src="assets/lighthouse-cifras.png" width="640" alt="Informe de Lighthouse" /></p>

## 🛠️ Tecnologías

| Tecnología | Para qué |
|---|---|
| [Astro 5](https://astro.build) | Generación de sitio estático, HTML por defecto |
| [React 19](https://react.dev) | Solo en los 3 componentes interactivos (header, pasos, FAQ) |
| [Tailwind CSS 4](https://tailwindcss.com) | Tokens de diseño y estilos |
| Plus Jakarta Sans | Tipografía (peso 800 y espaciado cerrado en títulos) |
| Cloudflare Workers (archivos estáticos) | Alojamiento, desplegado con `wrangler` |

## 🧠 Decisiones técnicas clave

### Astro: HTML por defecto, JavaScript solo donde hace falta
Es una página de contenido, no una aplicación. Astro genera HTML estático y manda JavaScript únicamente a los tres componentes que lo necesitan. Salida estática con URLs sin barra final y un `.html` por página, para que Cloudflare los sirva tal cual. Descarté un framework de aplicación completo: no hay estado ni datos que lo justifiquen. Unas 1 400 líneas entre componentes y textos.

### Una identidad en negro y grafito
El logo cuenta la app sin texto: un visor de cámara que encuadra un infinito, sobre una tira de ticket de borde rasgado.

- **Paleta neutra sin color de acento** (negro `#040405`, grafito `#3B3B44`, plata y niebla): decisión consciente para diferenciarse de las apps de finanzas azules y verdes.
- **Tokens en Tailwind 4** (`bg-brand-600`, `text-ink`, `bg-mist`): ningún color escrito a mano en los componentes.
- **Reglas escritas:** un solo acento (blanco o plata), texto blanco sobre grafito, nunca gris medio (pierde contraste).

<p align="center">
  <img src="assets/desktop-02-numeros-y-mision.png" width="460" alt="Sección de números y misión" />
  <img src="assets/desktop-04-funciones.png" width="460" alt="Nueve tarjetas de funciones" />
</p>

### Seis idiomas sin romper el SEO
Cada idioma es una página real, no un botón de traducir.

- **Todos los textos en un archivo** (`src/i18n/ui.ts`), un bloque por idioma; las páginas solo reciben `lang`. Agregar un idioma son cuatro pasos documentados.
- **Rutas por idioma:** español sin prefijo y `/en`, `/pt`, `/fr`, `/de`, `/it`. En portugués hasta los nombres de ruta cambian (`/pt/privacidade`).
- **Metadatos que salen de la lista de idiomas:** `canonical`, `hreflang` con `x-default`, `og:locale` y alternos, y un mapa del sitio con las versiones enlazadas.
- **JSON-LD** de tipo aplicación móvil por idioma e imagen para compartir generada desde una plantilla HTML con un navegador sin ventana.

<p align="center">
  <img src="assets/idioma-en-portada.png" width="300" alt="Versión en inglés" />
  <img src="assets/idioma-pt-portada.png" width="300" alt="Versión en portugués" />
  <img src="assets/idioma-fr-portada.png" width="300" alt="Versión en francés" />
</p>

### Textos legales que se mantienen al día
La app promete "sin internet y sin datos recopilados", así que la política pública debe decir exactamente lo mismo. Regla del proyecto: cuando una función de la app cambia lo que hace con los datos, se actualizan **a la vez** la pantalla de privacidad de la app, la política web, el manifiesto de privacidad de Apple y las etiquetas de App Store Connect. Política, términos y soporte están escritos completos en cada idioma.

<p align="center">
  <img src="assets/desktop-05-privacidad.png" width="460" alt="Sección de privacidad" />
  <img src="assets/pagina-privacidad.png" width="300" alt="Página de política de privacidad" />
</p>

### Responsive: de la mano al escritorio
La cuadrícula de nueve funciones pasa de una composición irregular a una columna; menú hamburguesa y selector de idioma en pantallas chicas; preguntas frecuentes como acordeón en React. Para las capturas móviles usé un navegador sin ventana dentro de un iframe de 390 px.

<p align="center">
  <img src="assets/movil-04-funciones.png" width="220" alt="Funciones en móvil" />
  <img src="assets/movil-06-preguntas.png" width="220" alt="Acordeón de preguntas en móvil" />
</p>

### Despliegue en Cloudflare
`npm run deploy` construye con Astro y sube `dist/` según `wrangler.jsonc`. Sin servidor que mantener ni base de datos que proteger. Primero publiqué en una dirección `workers.dev` y pasé al dominio propio al tenerlo, actualizando `site`, `robots.txt` y el sitemap a la vez.

## 🌱 Qué aprendí / qué mejoraría

- Una web de producto es parte del producto: la política legal importa tanto como la portada.
- Documentar la paleta y las reglas mantiene la coherencia para quien la mantenga.
- Los idiomas son arquitectura, no un parche al final.

Pendientes conocidos (lista honesta):
- Poner el enlace real de App Store y el precio (hoy dice "Muy pronto").
- Contraste del texto gris pequeño de las páginas legales (~2.6:1; con un gris más oscuro llega a 4.8:1).
- Revisión nativa de las traducciones PT/FR/DE/IT.

## 📄 Nota

Este repositorio contiene **solo documentación**; el código fuente es privado.

## 📬 Contacto

**Infinity dev by Frank** · [GitHub](https://github.com/InfinityByFrank) · [Infinity](https://infinitydev.agency) · [Portfolio](https://portfolio.infinitydev.agency/)
