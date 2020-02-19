---
title: Commands (Gatsby CLI)
tableOfContentsDepth: 2
---

La Gatsby Command Line Interface (CLI) è il principale strumento per installare e far funzionare una applicazione Gatsby e per l'utilizzo di funzionalità come l'esecuzione di un server di sviluppo e la creazione della tua applicazione Gatsby per il deployment.

_Forniamo una documentazione simile disponibile con il gatsby-cli [README](https://github.com/gatsbyjs/gatsby/blob/master/packages/gatsby-cli/README.md), e il nostro [cheat sheet](/docs/cheat-sheet/) contiene tutti i principali comandi CLI pronti per essere stampati._

## Come usare gatsby-cli

La Gatsby CLI (`gatsby-cli`) è costituito da un eseguibile che può essere utilizzato globalmente. La Gatsby CLI è disponibile tramite [npm](https://www.npmjs.com/) e può essere installato globalmente eseguendo `npm install -g gatsby-cli` per utilizzarlo localmente.

Esegui `gatsby --help` per l'aiuto completo.

Puoi anche utilizzare la variante script di questi comandi, `package.json`, tipicamente mostrata _per voi_ con la maggior parte degli [starters](/docs/starters/). Per esempio, se vuoi rendere il comando [`gatsby develop`](#develop) disponibile nella tua applicazione, apri `package.json` e aggiungi uno script in questo modo:

```json:title=package.json
{
  "scripts": {
    "develop": "gatsby develop"
  }
}
```

## Comandi API

### `new`

```shell
gatsby new [<site-name> [<starter-url>]]
```

#### Argomenti

| Argomento   | Descrizione                                                                                                                                                                                                                                                         |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| site-name   | Il nome del tuo sito Gatsby, che è usato anche per creare la directory del progetto.                                                                                                                                                                                |
| starter-url | Un Gatsby starter URL oppure un percorso locale ad un file. L'impostazione predefinita è [gatsby-starter-default](https://github.com/gatsbyjs/gatsby-starter-default); vedi la documentazione [Gatsby starters](/docs/gatsby-starters/) per ulteriori informazioni. |

> Nota: Il `site-name` deve essere composto di lettere e numeri. Se specifichi un `.`, `./` o uno `<spazio>` nel nome, `gatsby new` produrrà un errore.

#### Esempi

- Creare un sito Gatsby chiamato `my-awesome-site` utilizzando lo starter predefinito:

```shell
gatsby new my-awesome-site
```

- Creare un sito Gatsby chiamato `my-awesome-blog-site`, utilizzando [gatsby-starter-blog](https://www.gatsbyjs.org/starters/gatsbyjs/gatsby-starter-blog/):

```shell
gatsby new my-awesome-blog-site https://github.com/gatsbyjs/gatsby-starter-blog
```

- Se ometti entrambi gli argomenti, la CLI eseguirà una shell intereattiva che richiederà questi input:

```shell
gatsby new
? What is your project called? › my-gatsby-project
? What starter would you like to use? › - Use arrow-keys. Return to submit.
❯  gatsby-starter-default
   gatsby-starter-hello-world
   gatsby-starter-blog
   (Use a different starter)
```

Vedi la [documentazione Gatsby starters](https://www.gatsbyjs.org/docs/gatsby-starters/) per maggiori dettagli.

### `develop`

Una volta installato un sito Gatsby, vai alla cartella root del tuo progetto e avvia il server di sviluppo:

`gatsby develop`

#### Opzioni

<<<<<<< HEAD
|     Opzione     | Descrizione                                          |
| :-------------: | ---------------------------------------------------- |
| `-H`, `--host`  | Imposta host. L'impostazione predefinita è localhost |
| `-p`, `--port`  | Imposta la porta. L'impostazione predefinita è 8000  |
| `-o`, `--open`  | Apri il sito sul tuo browser (di default)            |
| `-S`, `--https` | Usa HTTPS                                            |
=======
|     Option      | Description                                     |
| :-------------: | ----------------------------------------------- |
| `-H`, `--host`  | Set host. Defaults to localhost                 |
| `-p`, `--port`  | Set port. Defaults to env.PORT or 8000          |
| `-o`, `--open`  | Open the site in your (default) browser for you |
| `-S`, `--https` | Use HTTPS                                       |
>>>>>>> 90932a06db2e297cf416552b84e48b4b82e56fbc

Segui la [guida Local HTTPS](/docs/local-https/) per scoprire come impostare un server di sviluppo HTTPS usando Gatsby.

#### Anteprima dei cambiamenti su altri dispositivi

Puoi utilizzare il comando Gatsby develop insieme all'opzione host per accedere al tuo ambiente di sviluppo su altri dispositivi sulla stessa rete, esegui:

```shell
gatsby develop -H 0.0.0.0
```

Dopo di che il terminal mostrerà come sempre il log delle informazioni, ma aggiungerà in più un URL al quale puoi accedere da un client sulla stessa rete per vedere come appare il sito.

<<<<<<< HEAD
```
Puoi adesso vedere gatsbyjs.org sul tuo browser.
=======
```shell
You can now view gatsbyjs.org in the browser.
>>>>>>> 90932a06db2e297cf416552b84e48b4b82e56fbc
⠀
  Locale:            http://0.0.0.0:8000/
  Sulla Tua Rete:  http://192.168.0.212:8000/ // highlight-line
```

<<<<<<< HEAD
**Note**: non puoi visitare 0.0.0.0:8000 su Windows (ma funzionerà o usando localhost:8000 o l'URL "Sulla Tua Rete" su Windows)
=======
**Note**: To access Gatsby on your local machine, use either `http://localhost:8000` or the "On Your Network" URL.
>>>>>>> 90932a06db2e297cf416552b84e48b4b82e56fbc

### `build`

Dalla root del tuo sito Gatsby, compila la tua applicazione e preparala per il deployment:

`gatsby build`

#### Opzioni

|           Opzione            | Descrizione                                                                                                                 |
| :--------------------------: | --------------------------------------------------------------------------------------------------------------------------- |
|       `--prefix-paths`       | Build del sito con i percorsi dei collegamenti prefissati (imposta pathPrefix nella tua configurazione)                     |
|        `--no-uglify`         | Build del sito senza che che i bundles JS passino per uglify (per il debugging)                                             |
| `--open-tracing-config-file` | File di configurazione per il tracing (compatibile con OpenTracing). Vedi [Performance Tracing](/docs/performance-tracing/) |
| `--no-color`, `--no-colors`  | Disabilita l'output colorato sul terminale                                                                                  |

In aggiunta a queste operazioni di build, ci sono alcune [variabili d'ambiente opzionali per il build](/docs/environment-variables/#build-variables) per configurazioni più avanzate che possono decidere il modo in cui viene eseguita una build. Ad esempio, settando `CI=true` come variabile d'ambiente si adatterà l'output per i [dumb terminals](https://en.wikipedia.org/wiki/Computer_terminal#Dumb_terminals).

### `serve`

Dalla root di un sito Gatsby, fai girare la build di produzione sul tuo sito per testarla:

`gatsby serve`

#### Opzioni

|     Opzione      | Descrizione                                                                                                                       |
| :--------------: | --------------------------------------------------------------------------------------------------------------------------------- |
|  `-H`, `--host`  | Impsta host. L'impostazione predefinita è localhost                                                                               |
|  `-p`, `--port`  | Imposta la porta. L'impostazione predefinita è 9000                                                                               |
|  `-o`, `--open`  | Apri il sito sul tuo browser (di default)                                                                                         |
| `--prefix-paths` | Fai girare il sito con i percorsi dei collegamenti prefissati(se la build è stata fatta con pathPrefix nel tuo gatsby-config.js). |

### `info`

Dalla root del tuo sito Gatsby, ottieni informazioni utili sull'ambiente che saranno richieste quando riporti un bug:

`gatsby info`

#### Opzioni

|       Opzione       | Descrizione                                                         |
| :-----------------: | ------------------------------------------------------------------- |
| `-C`, `--clipboard` | Automagicamente copia le informazioni sull'ambiente sulla clipboard |

### `clean`

Dalla root del tuo sito Gatsby, svuota la cache (cartella `.cache`) e le cartelle pubbliche:

`gatsby clean`

Questo è utile come ultima risorsa quando il tuo progetto locale sembra avere dei problemi o il contenuto non sembra aggiornarsi. Problemi che possono essere risolti in questo modo includono:

- Dati vecchi, e.g. questo file/risorsa/ecc. non appare
- Errore GraphQL, e.g. questa risorsa GraphQL dovrebbe essere presente ma non lo è
- Problemi di dipendenza, e.g. versione non valida, errori criptici in console, etc.
- Problemi di plugin, e.g. sviluppo di un plugin locale i cui cambiamenti sembrano non avere effetto

### `plugin`

Esegui i comandi relativi ai plugin di gatsby.

#### `docs`

`gatsby plugin docs`

Ti porta alla documentazione sull'uso e sulla creazione di plugin.

### Repl

Ottieni un REPL Node.js (shell interattiva) con il contesto del tuo ambiente Gatsby:

`gatsby repl`

Gatsby ti chiederà di digitare dei comandi ed esplorare. Quando mostra questo: `gatsby >`

Puoi digitare un comando, come uno dei seguenti:

`babelrc`

`components`

`dataPaths`

`getNodes()`

`nodes`

`pages`

`schema`

`siteConfig`

`staticQueries`

Quando combinati con il [GraphQL explorer](/docs/introducing-graphiql/), questi comandi REPL possono essere molto utili per comprendere i dati del tuo sito Gatsby.

Per ulteriori informazioni, controlla la [documentazione Gatsby REPL](/docs/gatsby-repl/).

### Disabilitare l'output colorato

In aggiunta all'opzione esplicita `--no-color`, la CLI considera la presenza della variabile d'ambiente `NO_COLOR` (vedi [no-color.org](https://no-color.org/)).

## Come cambiare il tuo package manager predefinito per il tuo prossimo progetto?

Quando usi`gatsby new` per la prima volta per creare un nuovo progetto, ti viene chiesto di scegliere il tuo package manager tra yarn ed npm.

```shell
Which package manager would you like to use ? › - Use arrow-keys. Return to submit.
❯  yarn
   npm
```

Una volta fatta la scelta, la CLI non ti chiederà più la tua preferenza per ogni progetto successivo. 

Se la vuoi cambiare per il prossimo progetto devi modificare il file di configurazione creato automaticamente dalla CLI.
Questo file è disponibile sul tuo sistema qui: `~/.config/gatsby/config.json`

Al suo interno vedrai qualcosa del genere.

```json:title=config.json
{
  "cli": {
    "packageManager": "yarn"
  }
}
```

Modifica il tuo valore `packageManager`, salva e puoi andare al tuo prossimo progetto usando `gatsby new`.
