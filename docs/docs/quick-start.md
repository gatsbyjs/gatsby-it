---
title: Quick Start
---

Questa guida quick start è indirizzata a sviluppatori con conoscenze da intermedie ad avanzate. Per una introduzione più graduale a Gatsby, [vai al nostro tutorial](/tutorial/)!

## Uso di Gatsby CLI

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-quick-start-with-gatsby-create-develop-and-build-gatsby-sites-from-the-command-line"
  lessonTitle="Quick Start with Gatsby: Create, Develop, and Build Gatsby Sites From the Command Line"
/>

**Nota**: questo video usa `npx`, che è un tool per eseguire un pacchetto npm senza prima installarlo. Eseguire il comando `npx gatsby new` è come eseguire `gatsby new` dopo aver installato gatsby-cli sul tuo computer.

### Installare Gatsby CLI.

```shell
npm install -g gatsby-cli
```

### Creare un nuovo sito.

```shell
gatsby new gatsby-site
```

### Cambiare cartella sul sito.

```shell
cd gatsby-site
```

### Avvio del server di sviluppo.

```shell
gatsby develop
```

Gatsby avvierà un ambiente di sviluppo hot-reloading accessibile di default da `localhost:8000`.

Prova a modificare le pagine JavaScript in `src/pages`. I cambiamenti salvati verranno ricaricati in tempo reale nel browser.

### Creare una production build.

```shell
gatsby build
```

Gatsby eseguirà una production build ottimizzata per il tuo sito, generando HTML statico e per-route JavaScript code bundles.

### Serve the production build localmente.

```shell
gatsby serve
```

Gatsby avvia un server HTML locale per testare il tuo built site. Ricorda di build il tuo sito usando `gatsby build` prima di usare questo comando.

### Access documentation for CLI commands

Per vedere la documentazione dettagliata per i comandi CLI, esegui `gatsby --help` nel terminale.

Per specifici comandi, esegui `gatsby COMMAND_NAME --help` e.g. `gatsby new --help`.

Per ulteriori informazioni su Gatsby CLI, visita la [CLI reference](/docs/gatsby-cli/) sezione della documentazione.
