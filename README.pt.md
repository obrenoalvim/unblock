# Unblock

[🇺🇸 Read in English](README.md)

Skill do Claude Code que não desiste quando uma busca na web falha. Uma ferramenta bloqueia, dá rate limit, pede CAPTCHA ou volta vazia? Ela passa pra próxima, em ordem, por 13 ferramentas grátis, até uma passar.

---

## O que faz

Use sempre que uma busca, scrape ou crawl falhar, ou de cara quando bloqueio parece provável: paywall, bot wall, site pesado em JS, rede social.

**Ordem, mais geral e robusta primeiro:**

1. **agent-reach**: roteador multi-plataforma (小红书, X, B站, Reddit, GitHub, YouTube, LinkedIn, RSS, web geral)
2. **last30days**: puxa discussão recente no Reddit, X, YouTube, TikTok, HN, GitHub
3. **claude-in-chrome**: automação de browser real, pra site que exige login, interação ou renderização real
4. **crawler**: Jina Reader, depois Scrapling stealth browser. Transforma URL em markdown limpo
5. **web-scraping**: checa o site antes, escolhe interceptação de tráfego, sitemap, API ou DOM
6. **web-scraping-automation**: Playwright/requests mais chamada REST e GraphQL genérica
7. **playwright-web-scraper**: extração estruturada multi-página, rate-limited, respeita robots.txt
8. **web-scraper**: extrator multi-estratégia pra tabela, lista, preço, contato
9. **site-crawler**: crawl multi-página direto
10. **scraping-skills**: pacote de técnicas acadêmicas e éticas
11. **scrapy-web-scraping**: framework Scrapy pra crawl grande multi-página
12. **news-extractor**: extração ajustada pra notícia e artigo em 12 sites
13. **Scrapling** (lib Python, último recurso): escreve e roda um script curto de stealth-fetch

Toda ferramenta da lista é grátis. Sem API key, sem tier pago, sem cadastro. Bloqueio ou resultado vazio manda pra próxima ferramenta; nunca repete a mesma duas vezes.

**Precisa das 13 instaladas pra cobertura completa.** Ferramenta não instalada é pulada, não conta como falha — o relatório final sempre separa "tentadas" (instaladas, rodadas de fato) de "puladas" (não instaladas), pra uma máquina com 3 das 13 não parecer "13 ferramentas falharam".

Algumas ferramentas pedem configuração além de instalar. **agent-reach** é zero-config pra 6 canais mas precisa de cookie/token pra outros (Twitter/X, 小红书) — roda `agent-reach doctor --json` pra checar. Lê a documentação de cada ferramenta antes de assumir que uma falha é bloqueio de verdade.

Antes de gastar a corrente toda, uma checagem barata de status HTTP + tamanho do body numa URL direta distingue "bloqueado" de "página genuinamente vazia" — não faz sentido rodar 13 ferramentas contra uma página sem conteúdo nenhum. A corrente também guarda qual ferramenta ganhou pra cada domínio (um placar local pequeno) e tenta essa primeiro da próxima vez, antes de cair pra ordem fixa abaixo.

**Fora de propósito:** qualquer coisa que precise de API key, tier pago ou conta (firecrawl-scrape, ferramentas x402, skills de proxy pago). Essa corrente fica grátis.

---

## Quando não usar

Pule a corrente pra pergunta factual simples que uma busca web comum já responde.

---

## Como usar

**Sem instalar:**
> "Leia https://github.com/obrenoalvim/unblock e siga a skill Unblock."

**Como plugin (disponível em toda sessão):**
```
/plugin marketplace add obrenoalvim/unblock
/plugin install unblock@unblock
```

Depois peça: "Usa a skill web pra achar X." Ela também dispara sozinha quando busca, scrape ou crawl trava.

**Copiando o arquivo:**
Copie `skills/web/SKILL.md` pro diretório de skills do seu sistema e invoque pelo seu próprio sistema.

---

## Funciona com

Qualquer sessão Claude Code. Cada ferramenta da corrente (agent-reach, crawler, Scrapling e o resto) precisa da própria instalação pro passo dela rodar. Ferramenta não instalada é pulada, e a corrente segue.

Não existe um comando único que instala as 13 de uma vez — elas vêm de fontes/marketplaces diferentes, então o `plugin.json` do `unblock` não pode declarar elas como dependência obrigatória com segurança (marketplace faltando quebraria a instalação pra todo mundo). Instala cada uma do jeito normal do seu setup, depois confere a cobertura (pergunta pro Claude: "quais das 13 ferramentas do unblock estão instaladas?").
