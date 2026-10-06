<div align="center">

<img src=".github/logo.svg" alt="Logo de Unblock" width="120" height="120">

# Unblock

**Una skill de Claude Code que sigue intentando cuando falla una consulta web.**<br>
¿Una herramienta se bloquea, recibe rate limit o vuelve vacía? Pasa a la siguiente de las 13 herramientas gratuitas hasta que una funcione.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/obrenoalvim/unblock?style=flat&logo=github&color=8b9cff)](https://github.com/obrenoalvim/unblock/stargazers)
[![Claude Code plugin](https://img.shields.io/badge/Claude_Code-plugin-5B5BD6)](#inicio-rápido)
[![13 herramientas gratis](https://img.shields.io/badge/herramientas-13_gratis-8b9cff)](#la-cadena)

[English](README.md) · [Português](README.pt.md) · **Español**

[Qué hace](#qué-hace) · [Inicio rápido](#inicio-rápido) · [Cómo funciona](#cómo-funciona) · [La cadena](#la-cadena) · [Instalación](#instalación) · [Preguntas frecuentes](#preguntas-frecuentes)

</div>

---

Unblock es una skill de Claude Code para la investigación web que choca con un muro. Cuando una búsqueda, un scraping o un crawl se bloquea, recibe rate limit, muestra un CAPTCHA o vuelve vacío, pasa a la siguiente herramienta en un orden fijo, entre 13 herramientas gratuitas, hasta que una funcione.

## Qué hace

Úsala cada vez que falle una búsqueda, un scraping o un crawl, o desde el principio cuando un bloqueo parezca probable: paywalls, bot walls, sitios cargados de JS, redes sociales.

Todas las herramientas de la cadena son gratuitas. Sin API key, sin plan de pago, sin registro.

## Inicio rápido

```
/plugin marketplace add obrenoalvim/unblock
/plugin install unblock@unblock
```

Luego pídela: "Usa la skill web para encontrar X." También se activa sola cuando una búsqueda, un scraping o un crawl se bloquea.

---

## Cómo funciona

1. **Primero una verificación barata.** En una URL directa, una petición de estado HTTP y tamaño del body distingue un bloqueo (403, 429, CAPTCHA) de una página genuinamente vacía. Un 200 vacío detiene la cadena, así que nunca ejecuta 13 herramientas contra la nada.
2. **Un marcador por dominio.** La skill recuerda qué herramienta funcionó en cada dominio en `~/.claude/unblock-scoreboard.json` y prueba esa primero la próxima vez.
3. **Un orden fijo.** La cadena va de la herramienta más general y robusta hasta el último recurso. Un bloqueo o un resultado vacío la manda a la siguiente, y nunca repite la misma herramienta dos veces.
4. **Un informe honesto.** Separa las herramientas intentadas (instaladas y ejecutadas) de las omitidas (no instaladas), para que una máquina con 3 de las 13 no se lea como "13 herramientas fallaron".

## La cadena

Primero la más general y robusta:

| # | Herramienta | Para qué sirve |
|---|---|---|
| 1 | **[agent-reach](https://buildwithclaude.com/skill/agent-reach)** | Enrutador multiplataforma (小红书, X, B站, Reddit, GitHub, YouTube, LinkedIn, RSS, web en general) |
| 2 | **[last30days](https://github.com/mvanhorn/last30days-skill)** | Discusión reciente en Reddit, X, YouTube, TikTok, HN, GitHub |
| 3 | **[claude-in-chrome](https://code.claude.com/docs/en/chrome)** | Automatización de navegador real, para sitios que exigen login, interacción o renderizado real |
| 4 | **crawler** | Jina Reader y después el navegador sigiloso de Scrapling. Convierte una URL en markdown limpio |
| 5 | **web-scraping** | Revisa el sitio primero y elige entre interceptación de tráfico, sitemap, API o DOM |
| 6 | **web-scraping-automation** | Playwright/requests más llamadas REST y GraphQL genéricas |
| 7 | **playwright-web-scraper** | Extracción estructurada multipágina, con rate limit, respeta robots.txt |
| 8 | **web-scraper** | Extractor multiestrategia para tablas, listas, precios, contactos |
| 9 | **site-crawler** | Crawl multipágina directo |
| 10 | **scraping-skills** | Paquete de técnicas académicas y éticas |
| 11 | **scrapy-web-scraping** | Framework Scrapy para crawls grandes multipágina |
| 12 | **news-extractor** | Extracción ajustada para noticias y artículos en 12 sitios |
| 13 | **[Scrapling](https://github.com/D4Vinci/Scrapling)** | Librería de Python, último recurso: escribe y ejecuta un script corto de stealth-fetch |

Los ítems 4 a 12 son nombres de skills genéricos de la comunidad. Varias implementaciones independientes usan el mismo nombre en marketplaces y registros distintos (buildwithclaude.com, skills.rest, forks sueltos en GitHub), así que no hay una fuente canónica única que enlazar con confianza. Busca el nombre en tu marketplace de skills o registro de plugins para instalar una.

**Necesitas las 13 instaladas para tener cobertura completa.** Una herramienta no instalada se omite, no cuenta como fallo.

Algunas herramientas necesitan configuración además de instalarse. **agent-reach** funciona sin configuración en 6 canales, pero necesita una cookie o un token en otros (Twitter/X, 小红书). Ejecuta `agent-reach doctor --json` para comprobarlo. Lee la documentación de cada herramienta antes de asumir que un fallo es un bloqueo real.

**Fuera a propósito:** todo lo que necesite una API key, un plan de pago o una cuenta (firecrawl-scrape, herramientas x402, skills de proxy de pago). Esta cadena se mantiene gratuita.

### Ejemplo de informe

```
Worked: `crawler`. Tried 4 installed tool(s) (9 of 13 not installed, skipped: `web-scraping`, `web-scraping-automation`, `playwright-web-scraper`, `web-scraper`, `site-crawler`, `scraping-skills`, `scrapy-web-scraping`, `news-extractor`, `Scrapling`).
```

## Cuándo no usarla

Salta la cadena para una pregunta factual simple que una búsqueda web común ya responde.

---

## Instalación

**Como plugin (disponible en todas las sesiones):**
```
/plugin marketplace add obrenoalvim/unblock
/plugin install unblock@unblock
```

Luego pídela: "Usa la skill web para encontrar X."

**Sin instalar:**
> "Lee https://github.com/obrenoalvim/unblock y sigue la skill Unblock."

**Copia el archivo:**
Copia [`skills/web/SKILL.md`](skills/web/SKILL.md) a tu directorio de skills e invócalo desde tu propio sistema de skills.

## Funciona con

Cualquier sesión de Claude Code. Cada herramienta de la cadena (agent-reach, crawler, Scrapling y el resto) necesita su propia instalación para que su paso se ejecute. Una herramienta no instalada se omite y la cadena sigue.

No existe un comando único que instale las 13 a la vez. Viven en fuentes y marketplaces distintos, así que el `plugin.json` de `unblock` no puede declararlas como dependencias obligatorias con seguridad (un marketplace ausente rompería la instalación para todos). Instala cada una de la forma normal en tu entorno y comprueba la cobertura preguntándole a Claude: "¿cuáles de las 13 herramientas de unblock están instaladas?"

---

## Preguntas frecuentes

**¿Necesito API keys o un plan de pago?**
No. Todas las herramientas de la cadena son gratuitas, y lo que exige una clave, un plan de pago o una cuenta queda fuera a propósito.

**¿Necesito las 13 herramientas?**
No. Las que faltan se omiten y aparecen como omitidas en el informe. La cobertura completa solo llega con las 13.

**¿Va a martillar un sitio reintentando?**
No. Nunca repite la misma herramienta dos veces, y `playwright-web-scraper` tiene rate limit y respeta robots.txt.

## Más skills para Claude Code del mismo autor

- [**zero-drift**](https://github.com/obrenoalvim/zero-drift): mantiene ancladas las sesiones largas con respuestas con nombre y un `TASK.md` vivo.
- [**keep-improving**](https://github.com/obrenoalvim/keep-improving): un bucle autónomo de mejora con un panel de revisión de diez roles.
- [**findable**](https://github.com/obrenoalvim/findable): investigación de SEO y GEO que aplica las correcciones seguras.
- [**no-watermark**](https://github.com/obrenoalvim/no-watermark): detecta y elimina marcas de agua Unicode invisibles en el texto.

## Contribuir

¿Conoces una herramienta gratuita que merezca entrar en la cadena, o un orden mejor? Abre un issue o un PR. Consulta el [CONTRIBUTING.md](CONTRIBUTING.md) y el [changelog](CHANGELOG.md).

## Licencia

[MIT](LICENSE)

---

<div align="center">

Si Unblock te sacó de un muro, una ⭐ ayuda a que otras personas lo encuentren también.

<sub>**Temas:** claude-code · claude-skill · claude-code-plugin · web-scraping · web-scraper · web-crawler · web-research · data-extraction · playwright · scrapling · mcp</sub>

</div>
