# Catalogo dei plugin Claude Code di Meridiano AI

Marketplace di plugin per [Claude Code](https://code.claude.com/docs/en/plugins/marketplace-reference.md)
distribuiti da Meridiano AI. Il file è `.claude-plugin/marketplace.json`, con una voce per plugin:
ogni voce punta al repo originale del plugin ed è fissata su un tag e sul suo commit, quindi chi
installa da qui riceve lo stesso codice finché la voce non cambia.

## Installazione

```bash
claude plugin marketplace add meridianoai/plugins
claude plugin install <plugin>@meridianoai-plugins
```

## Plugin

| Plugin | Repo | Versione |
|---|---|---|
| `session-tracker` | [fvadicamo/session-tracker-skill](https://github.com/fvadicamo/session-tracker-skill) | v1.0.0 |

## Aggiornare un plugin

Passare a un nuovo rilascio è una PR su questo repo che cambia `ref` e `sha` della voce: `sha` è
il commit di 40 caratteri del tag e, quando ci sono entrambi, Claude Code fa il checkout di `sha`.
Le modifiche ai plugin vanno come PR al repo originale, non qui.

## Perché si chiama così

- Il repo si chiama `plugins` e non `claude-plugins` perché lo stesso repo può ospitare i
  cataloghi di più strumenti: Claude Code legge `.claude-plugin/marketplace.json`, e la
  [documentazione dei plugin Codex](https://developers.openai.com/codex/plugins/build) indica
  `.agents/plugins/marketplace.json` e accetta `.claude-plugin/marketplace.json` come
  «legacy-compatible». Che cosa succeda quando nello stesso repo ci sono entrambi i file non è
  stato misurato.
- Il suffisso `-plugins` è quello che Anthropic usa per le raccolte di plugin
  (`claude-plugins-official`, `knowledge-work-plugins`), e `acme-corp/plugins` è l'esempio di
  repo di un'organizzazione nella
  [marketplace reference](https://code.claude.com/docs/en/plugins/marketplace-reference.md).
- Il marketplace si chiama `meridianoai-plugins`, che è anche il nome della cartella in cui
  Claude Code scarica un marketplace aggiunto come `meridianoai/plugins`: la reference segnala che
  un altro marketplace con quel nome verrebbe rifiutato.
