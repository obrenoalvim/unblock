<div align="center">

<img src=".github/logo.svg" alt="Logo do Unblock" width="120" height="120">

# Unblock

**Uma skill do Claude Code que não desiste quando uma busca na web falha.**<br>
Uma ferramenta bloqueia, dá rate limit ou volta vazia? Ela passa para a próxima das 13 ferramentas grátis até uma passar.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/obrenoalvim/unblock?style=flat&logo=github&color=8b9cff)](https://github.com/obrenoalvim/unblock/stargazers)
[![Claude Code plugin](https://img.shields.io/badge/Claude_Code-plugin-5B5BD6)](#início-rápido)
[![13 ferramentas grátis](https://img.shields.io/badge/ferramentas-13_gr%C3%A1tis-8b9cff)](#a-corrente)

[English](README.md) · **Português** · [Español](README.es.md)

[O que faz](#o-que-faz) · [Início rápido](#início-rápido) · [Como funciona](#como-funciona) · [A corrente](#a-corrente) · [Instalação](#instalação) · [Perguntas frequentes](#perguntas-frequentes)

</div>

---

O Unblock é uma skill do Claude Code para pesquisa na web que bate numa parede. Quando uma busca, scrape ou crawl é bloqueado, toma rate limit, pede CAPTCHA ou volta vazio, ela passa para a próxima ferramenta, em ordem fixa, por 13 ferramentas grátis, até uma passar.

## O que faz

Use sempre que uma busca, scrape ou crawl falhar, ou de cara quando bloqueio parece provável: paywall, bot wall, site pesado em JS, rede social.

Toda ferramenta da corrente é grátis. Sem API key, sem tier pago, sem cadastro.

## Início rápido

```
/plugin marketplace add obrenoalvim/unblock
/plugin install unblock@unblock
```

Depois peça: "Usa a skill web pra achar X." Ela também dispara sozinha quando busca, scrape ou crawl trava.

---

## Como funciona

1. **Uma checagem barata primeiro.** Numa URL direta, uma requisição de status HTTP e tamanho do body distingue bloqueio (403, 429, CAPTCHA) de página genuinamente vazia. Um 200 vazio para a corrente, então ela nunca roda 13 ferramentas contra nada.
2. **Um placar por domínio.** A skill guarda qual ferramenta funcionou em cada domínio em `~/.claude/unblock-scoreboard.json` e tenta essa primeiro da próxima vez.
3. **Uma ordem fixa.** A corrente vai da ferramenta mais geral e robusta até o último recurso. Bloqueio ou resultado vazio manda para a próxima, e nunca repete a mesma ferramenta duas vezes.
4. **Um relatório honesto.** Ele separa as ferramentas tentadas (instaladas e rodadas) das puladas (não instaladas), pra uma máquina com 3 das 13 não parecer "13 ferramentas falharam".

## A corrente

Mais geral e robusta primeiro:

| # | Ferramenta | Pra quê |
|---|---|---|
| 1 | **[agent-reach](https://buildwithclaude.com/skill/agent-reach)** | Roteador multi-plataforma (小红书, X, B站, Reddit, GitHub, YouTube, LinkedIn, RSS, web geral) |
| 2 | **[last30days](https://github.com/mvanhorn/last30days-skill)** | Discussão recente no Reddit, X, YouTube, TikTok, HN, GitHub |
| 3 | **[claude-in-chrome](https://code.claude.com/docs/en/chrome)** | Automação de browser real, pra site que exige login, interação ou renderização real |
| 4 | **crawler** | Jina Reader, depois Scrapling stealth browser. Transforma URL em markdown limpo |
| 5 | **web-scraping** | Checa o site antes, escolhe interceptação de tráfego, sitemap, API ou DOM |
| 6 | **web-scraping-automation** | Playwright/requests mais chamada REST e GraphQL genérica |
| 7 | **playwright-web-scraper** | Extração estruturada multi-página, rate-limited, respeita robots.txt |
| 8 | **web-scraper** | Extrator multi-estratégia pra tabela, lista, preço, contato |
| 9 | **site-crawler** | Crawl multi-página direto |
| 10 | **scraping-skills** | Pacote de técnicas acadêmicas e éticas |
| 11 | **scrapy-web-scraping** | Framework Scrapy pra crawl grande multi-página |
| 12 | **news-extractor** | Extração ajustada pra notícia e artigo em 12 sites |
| 13 | **[Scrapling](https://github.com/D4Vinci/Scrapling)** | Lib Python, último recurso: escreve e roda um script curto de stealth-fetch |

Os itens 4 a 12 são nomes de skill genéricos da comunidade. Várias implementações independentes usam o mesmo nome em marketplaces e registros diferentes (buildwithclaude.com, skills.rest, forks soltos no GitHub), então não tem uma fonte canônica única pra linkar com confiança. Procura o nome no seu marketplace de skills ou registro de plugin pra instalar uma.

**Precisa das 13 instaladas pra cobertura completa.** Ferramenta não instalada é pulada, não conta como falha.

Algumas ferramentas pedem configuração além de instalar. O **agent-reach** funciona sem configuração em 6 canais, mas precisa de cookie ou token em outros (Twitter/X, 小红书). Rode `agent-reach doctor --json` pra checar. Leia a documentação de cada ferramenta antes de assumir que uma falha é bloqueio de verdade.

**Fora de propósito:** qualquer coisa que precise de API key, tier pago ou conta (firecrawl-scrape, ferramentas x402, skills de proxy pago). Essa corrente fica grátis.

### Exemplo de relatório

```
Worked: `crawler`. Tried 4 installed tool(s) (9 of 13 not installed, skipped: `web-scraping`, `web-scraping-automation`, `playwright-web-scraper`, `web-scraper`, `site-crawler`, `scraping-skills`, `scrapy-web-scraping`, `news-extractor`, `Scrapling`).
```

## Quando não usar

Pule a corrente pra pergunta factual simples que uma busca web comum já responde.

---

## Instalação

**Como plugin (disponível em toda sessão):**
```
/plugin marketplace add obrenoalvim/unblock
/plugin install unblock@unblock
```

Depois peça: "Usa a skill web pra achar X."

**Sem instalar:**
> "Leia https://github.com/obrenoalvim/unblock e siga a skill Unblock."

**Copiando o arquivo:**
Copie [`skills/web/SKILL.md`](skills/web/SKILL.md) pro diretório de skills do seu sistema e invoque pelo seu próprio sistema.

## Funciona com

Qualquer sessão Claude Code. Cada ferramenta da corrente (agent-reach, crawler, Scrapling e o resto) precisa da própria instalação pro passo dela rodar. Ferramenta não instalada é pulada, e a corrente segue.

Não existe um comando único que instala as 13 de uma vez. Elas vêm de fontes e marketplaces diferentes, então o `plugin.json` do `unblock` não pode declarar elas como dependência obrigatória com segurança (marketplace faltando quebraria a instalação pra todo mundo). Instale cada uma do jeito normal do seu setup e confira a cobertura perguntando ao Claude: "quais das 13 ferramentas do unblock estão instaladas?"

---

## Perguntas frequentes

**Preciso de API key ou plano pago?**
Não. Toda ferramenta da corrente é grátis, e o que exige chave, tier pago ou conta fica de fora de propósito.

**Preciso das 13 ferramentas?**
Não. As que faltam são puladas e aparecem como puladas no relatório. A cobertura completa só vem com as 13.

**Ela vai martelar um site tentando de novo?**
Não. Nunca repete a mesma ferramenta duas vezes, e o `playwright-web-scraper` tem rate limit e respeita o robots.txt.

## Mais skills para Claude Code do mesmo autor

- [**zero-drift**](https://github.com/obrenoalvim/zero-drift): mantém sessões longas ancoradas com respostas com nome e um `TASK.md` vivo.
- [**keep-improving**](https://github.com/obrenoalvim/keep-improving): loop autônomo de melhoria com um painel de revisão de dez papéis.
- [**findable**](https://github.com/obrenoalvim/findable): pesquisa de SEO e GEO que aplica as correções seguras.
- [**no-watermark**](https://github.com/obrenoalvim/no-watermark): detecta e remove marcas d'água Unicode invisíveis de textos.

## Contribuindo

Conhece uma ferramenta grátis que merece entrar na corrente, ou uma ordem melhor? Abra uma issue ou um PR. Veja o [CONTRIBUTING.md](CONTRIBUTING.md) e o [changelog](CHANGELOG.md).

## Licença

[MIT](LICENSE)

---

<div align="center">

Se o Unblock te fez passar por uma parede, uma ⭐ ajuda outras pessoas a encontrá-lo também.

<sub>**Tópicos:** claude-code · claude-skill · claude-code-plugin · web-scraping · web-scraper · web-crawler · web-research · data-extraction · playwright · scrapling · mcp</sub>

</div>
