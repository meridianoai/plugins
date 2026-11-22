# Catalogo dei plugin Claude Code di Meridiano AI

Marketplace di plugin per [Claude Code](https://code.claude.com/docs/en/overview) distribuiti da
Meridiano AI. Il file è `.claude-plugin/marketplace.json`, con una voce per plugin: ogni voce punta
al repo originale del plugin ed è fissata su un tag e sul suo commit, quindi chi installa da qui
riceve lo stesso codice finché la voce non cambia.

## Installazione

```bash
claude plugin marketplace add meridianoai/plugins
claude plugin install <plugin>@meridianoai-plugins
```

Senza una chiave SSH per GitHub, imposta la variabile `CLAUDE_CODE_PLUGIN_PREFER_HTTPS=1` in modo
permanente (nel profilo della shell, o sotto `env` in `~/.claude/settings.json`) prima di questi
comandi: le installazioni da `/plugin` dentro una sessione e gli aggiornamenti non ripiegano da
soli su HTTPS
([hosting di un marketplace](https://code.claude.com/docs/en/plugins/host-marketplace)).

## Plugin

| Plugin | Repo | Versione |
|---|---|---|
| `session-tracker` | [fvadicamo/session-tracker-skill](https://github.com/fvadicamo/session-tracker-skill) | v1.0.0 |

## Aggiornare un plugin

Passare a un nuovo rilascio è una PR su questo repo che cambia `ref` e `sha` della voce: `sha` è
l'hash di 40 caratteri del commit del tag e, quando ci sono entrambi, Claude Code fa il checkout di
`sha`. Il nuovo tag deve portare un `version` diverso nel `plugin.json` del plugin: con lo stesso
`version`, chi ha già installato il plugin non riceve l'aggiornamento
([versioni e aggiornamenti](https://code.claude.com/docs/en/plugins/loading)). Le modifiche ai
plugin vanno come PR al repo originale, non qui.

## Perché si chiama così

- Il repo si chiama `plugins` e non `claude-plugins` perché lo stesso repo può ospitare i
  cataloghi di più strumenti: Claude Code legge `.claude-plugin/marketplace.json`, e la
  [documentazione dei plugin di OpenAI](https://developers.openai.com/plugins/build/plugins)
  indica `.agents/plugins/marketplace.json` e dice che l'app desktop di ChatGPT legge anche
  `.claude-plugin/marketplace.json` come «legacy-compatible». Che cosa succeda quando nello stesso
  repo ci sono entrambi i file non è stato misurato.
- Il suffisso `-plugins` è quello che Anthropic usa per le raccolte di plugin
  (`claude-plugins-official`, `knowledge-work-plugins`), e `acme-corp/plugins` compare come
  esempio nella
  [marketplace reference](https://code.claude.com/docs/en/plugins/marketplace-reference).
- Il marketplace si chiama `meridianoai-plugins`, che è anche il nome della cartella in cui
  Claude Code scarica un marketplace aggiunto come `meridianoai/plugins`: così il marketplace
  occupa un nome solo, perché con un nome diverso Claude Code rifiuterebbe comunque un altro
  marketplace chiamato come la cartella.
