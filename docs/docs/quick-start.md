---
title: Quick Start
---

Questa guida rapida è indirizzata a sviluppatori con conoscenze intermedie o avanzate. Per una introduzione più graduale a Gatsby, [vai al nostro tutorial](/tutorial/)!

## Uso di Gatsby CLI

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-quick-start-with-gatsby-create-develop-and-build-gatsby-sites-from-the-command-line"
  lessonTitle="Quick Start with Gatsby: Create, Develop, and Build Gatsby Sites From the Command Line"
/>

**Nota**: questo video usa `npx`, che è un tool per eseguire un pacchetto npm senza prima installarlo. Eseguire il comando `npx gatsby new` è come eseguire `gatsby new` dopo aver installato gatsby-cli sul tuo computer.

### Installazione di Gatsby CLI.

```shell
npm install -g gatsby-cli
```

<<<<<<< HEAD
### Creazione di un nuovo sito.
=======
> The above command installs Gatsby CLI globally on your machine.

### Create a new site
>>>>>>> 90932a06db2e297cf416552b84e48b4b82e56fbc

```shell
gatsby new gatsby-site
```

### Andare alla cartella del sito.

```shell
cd gatsby-site
```

### Avvio del server di sviluppo.

```shell
gatsby develop
```

<<<<<<< HEAD
Gatsby avvierà un ambiente di sviluppo hot-reloading accessibile di default su `localhost:8000`.
=======
Gatsby will start a hot-reloading development environment accessible by default at `http://localhost:8000`.
>>>>>>> 90932a06db2e297cf416552b84e48b4b82e56fbc

Prova a modificare le pagine JavaScript in `src/pages`. I cambiamenti salvati verranno ricaricati in tempo reale nel browser.

### Creazione di una production build.

```shell
gatsby build
```

Gatsby eseguirà una production build ottimizzata per il tuo sito, generando HTML statico e dei bundle di codice JavaScript per-route.

### Far girare la production build localmente.

```shell
gatsby serve
```

Gatsby avvia un server HTML locale per testare il tuo sito. Ricorda di fare il build del tuo sito usando `gatsby build` prima di usare questo comando.

### Accesso alla documentazione per i comandi CLI

Per vedere la documentazione dettagliata per i comandi CLI, esegui `gatsby --help` nel terminale.

Per specifici comandi, esegui `gatsby COMMAND_NAME --help` e.g. `gatsby new --help`.

Per ulteriori informazioni su Gatsby CLI, visita la [CLI reference](/docs/gatsby-cli/) sezione della documentazione.
