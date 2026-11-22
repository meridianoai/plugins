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

| Plugin | Repo | Tag | Versione del plugin |
|---|---|---|---|
| `session-tracker` | [fvadicamo/session-tracker-skill](https://github.com/fvadicamo/session-tracker-skill) | v1.0.0 | 1.0.0 |
| `github-workflow` | [fvadicamo/dev-agent-skills, `plugins/github-workflow`](https://github.com/fvadicamo/dev-agent-skills/tree/github-workflow--v1.4.2/plugins/github-workflow) | github-workflow--v1.4.2 | 1.4.2 |
| `skill-authoring` | [fvadicamo/dev-agent-skills, `plugins/skill-authoring`](https://github.com/fvadicamo/dev-agent-skills/tree/skill-authoring--v1.4.0/plugins/skill-authoring) | skill-authoring--v1.4.0 | 1.4.0 |
| `guardrails` | [fvadicamo/dev-agent-skills, `plugins/guardrails`](https://github.com/fvadicamo/dev-agent-skills/tree/guardrails--v1.7.6/plugins/guardrails) | guardrails--v1.7.6 | 1.7.6 |
| `privacy-guard` | [fvadicamo/dev-agent-skills, `plugins/privacy-guard`](https://github.com/fvadicamo/dev-agent-skills/tree/privacy-guard--v1.3.3/plugins/privacy-guard) | privacy-guard--v1.3.3 | 1.3.3 |
| `decision-records` | [fvadicamo/dev-agent-skills, `plugins/decision-records`](https://github.com/fvadicamo/dev-agent-skills/tree/decision-records--v0.3.0/plugins/decision-records) | decision-records--v0.3.0 | 0.3.0 |

I plugin di `dev-agent-skills` stanno in sottocartelle dello stesso repo, quindi le loro voci usano
la fonte `git-subdir` (`url`, `path`, `ref`, `sha`), che scarica solo la cartella del plugin.

## Non installare lo stesso plugin da due cataloghi

Se hai già un plugin da un altro catalogo (per esempio `decision-records@dev-agent-skills`),
disinstallalo prima di installarlo da qui, oppure tienilo solo da lì. Con due copie abilitate dello
stesso plugin le skill si caricano due volte, e gli hook girano per una copia sola, quella che
Claude Code carica per prima, che può non essere la versione fissata qui
([hook con due plugin dello stesso nome](https://code.claude.com/docs/en/plugins/loading#hooks-when-two-enabled-plugins-share-a-name)).
Per vedere da dove viene ciascun plugin: `claude plugin list`.

```bash
claude plugin uninstall decision-records@dev-agent-skills
claude plugin install decision-records@meridianoai-plugins
```

## Aggiornare un plugin

Ogni voce si aggiorna indipendentemente dalle altre quando il suo plugin rilascia una versione nuova:

1. nel repo del plugin, il `version` del suo `plugin.json` sale: con lo stesso `version`, chi ha
   già installato il plugin non riceve l'aggiornamento
   ([versioni e aggiornamenti](https://code.claude.com/docs/en/plugins/loading#versions-and-updates));
2. il rilascio ha un tag. In un repo con più plugin, come `dev-agent-skills`, è un tag per plugin
   nella forma `<plugin>--v<version>`, creato con `claude plugin tag` dalla cartella del plugin
   ([tag dei rilasci](https://code.claude.com/docs/en/plugins/dependencies#tag-plugin-releases-for-version-resolution)); in un repo con un
   plugin solo, come `session-tracker-skill`, è il tag del rilascio (`v1.0.0`);
3. una PR su questo repo cambia la sola voce di quel plugin: `ref` diventa il nuovo tag e `sha`
   l'hash di 40 caratteri del suo commit (quando ci sono entrambi, Claude Code fa il checkout di
   `sha`); se il `plugin.json` ha cambiato `description`, la voce prende quella nuova, e la tabella
   sopra si aggiorna.

Le modifiche ai plugin vanno come PR al repo originale, non qui.

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
