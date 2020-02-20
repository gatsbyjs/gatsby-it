---
title: Imposta il tuo Ambiente di Sviluppo
typora-copy-images-to: ./
disableTableOfContents: true
---

Prima di iniziare a creare il tuo primo sito Gatsby, avrai bisogno di familiarizzare con alcune tecnologie web di base e assicurarti di aver installato tutti gli strumenti software richiesti.

## Familiarizzare con la linea di comando

La linea di comando è una interfaccia testuale che esegue comandi sul tuo computer. Sarà anche fatto riferimento ad essa come terminale. In questo tutorial, useremo entrambi. È un po come usare il Finder sul Mac o Explorer su Windows. Il Finder e Explorer sono esempi di interfaccia grafica (GUI). La linea di comando è un modo potente, basato sul testo, per interagire con il computer.

Prenditi un attimo per cercare e aprire la linea di comando (CLI) sul tuo computer. A seconda del sistema operativo che stai usando, controlla [**istruzioni per Mac**](http://www.macworld.co.uk/feature/mac-software/how-use-terminal-on-mac-3608274/), [**istruzioni per Windows**](https://www.quora.com/How-do-I-open-terminal-in-windows) o [**istruzioni per Linux**](https://www.howtogeek.com/140679/beginner-geek-how-to-start-using-the-linux-terminal/).

_Nota: Se sei nuovo alla, "lanciare" un comando, significa "scrivere un dato set di istruzioni nel prompt dei comandi, e premere il tasto Enter". I comandi saranno evidenziati, una cosa del tipo `node --version`, ma non ogni cosa evidenziata è un comando! Se qualcosa è un comando, esso verrà indicato come qualcosa da lanciare/eseguire._

## Installa Node.js per il tuo sistema operativo

Node.js è un un ambiente che può eseguire codice JavaScript al di fuori del browser. Gatsby è basato su Node.js. Per iniziare ad usare Gatsby, avrai bisogno di una versione recente installate sul tuo computer. npm viene fornito assieme a Node.js, quindi se non hai npm, è probabile che tu non abbia nemmeno Node.js.

### Istruzioni per il Mac

Per installare Gatsby e Node.js su un Mac, è raccomandato di usare [Homebrew](https://brew.sh/). Un piccolo set-up all'inizio ti può aiutare ad evitare mal di testa dopo!

#### Come installare e verificare Homebrew sul tuo computer:

1. Apri il Terminale
2. Controlla se Homebrew è installato eseguendo `brew -v`. Dovresti vedere "Homebrew" e un numero di versione.
3. In caso contrario, scarica e installa [Homebrew seguendo queste istruzioni](https://docs.brew.sh/Installation).
4. Una volta che hai installato Homebrew, ripeti il punto 2 per verifica.

#### Installare Xcode Command Line Tools:

1. Apri il Terminale
2. Installa Xcode Command Line Tools con il comando `xcode-select --install`.
   - Se fallisce, scaricalo [direttamente dal sito Apple](https://developer.apple.com/download/more/), dopo aver eseguito l'accesso con un account Apple da sviluppatore
3. Dopo la richiesta di inizio installazione, ti verrà richiesto nuovamente di accettare una licenza software per gli strumenti da scaricare.

#### Installare Node

1. Apri il Terminale
2. Esegui `brew install node`
   - Se non vuoi installarlo tramte Homebrew, scarica l'ultima version di Node.js dal [sito ufficiale di Node.js](https://nodejs.org/en/), fai doppio click sul file scaricato e segui il processo di installazione

### Istruzioni per Windows

- Scarica e installa l'ultima version di Node.js dal [sito ufficiale di Node.js](https://nodejs.org/en/),

### Istruzioni per Linux

Installa nvm (Node Version Manager) e le dipendenze necessarie. nvm è usato per gestire tutte le versioni di Node.js.

_💡 Se quando installi un pacchetto, ti chiede conferma, scrivi `y` e premi invio._

#### Ubuntu, Debian, e altre distro basate su `apt`:

1. Lancia `sudo apt update` e poi `sudo apt -y upgrade` per essere sicuro che la tua distribuzione Linux sia aggiornata.
2. Lancia `sudo apt-get install curl` per installare curl che ti permette di trasferire dati e scaricare ulteriori dipendenze.
3. Appena finito di installarsi, lancia `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.1/install.sh | bash` per scaricare l'ultima versione di nvm.
4. Per controllare che abbia funzionato, usa questo comando. `nvm --version`. L'output dovrebbe essere un numero di versione.
5. [Imposta la versione di default di Node.js](#imposta-la-versione-di-default di-nodejs)

#### Arch, Manjaro e altre distro basate su `pacman`:

1. Lancia `sudo pacman -Sy` per essere sicuro che la tua distribuzione sia aggiornata.
2. Queste distro hanno curl già installato, così puoi usarlo per scaricare nvm.
   `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.1/install.sh | bash`
3. Prima di usare nvm, devi installare ulteriori dipendenze lanciando `sudo pacman -S grep awk tar`.
4. Per controllare che abbia funzionato, usa questo comando. `nvm --version`.
5. [Imposta la versione di default di Node.js](#imposta-la-versione-di-default di-nodejs)

#### Fedora, RedHat, e altre distro basate su `dnf`:

1. Queste distro hanno curl già installato, così puoi usarlo per scaricare nvm.
   `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.1/install.sh | bash`
2. Per controllare che abbia funzionato, usa questo comando. `nvm --version`.
3. [Imposta la versione di default di Node.js](#imposta-la-versione-di-default di-nodejs)

Se la distribuzione Linux che stai usando non è in quetso elenco, per favore cerca le istruzioni sul web.

#### Imposta la versione di default di Node.js

Quando nvm è installato, non ha una versione di default di node impostata. Dovrai installare la versione che desideri e dare a nvm le istruzioni per usarla. Questo esempio utilizza l'ultima versione della versione 10, ma è possibile utilizzare invece numeri di versione più recenti.

```shell
nvm install 10
nvm use 10
```

Per controllare che tutto funzioni, puoi lanciare `npm --version` e `node --version`. L'output dovrebbe essere simile all'immagine qua sotto, mostrando i numeri di versione.

![Controlla nel terminale la versione di node e npm](01-node-npm-versions.png)

Dopo aver seguito i passaggi dell'installazione e aver verificato che tutto sia installato correttamente, puoi continuare con il punto successivo.

## Installa Git

Git è un sistema di controllo di versione libero e open source progettato per gestire qualsiasi cosa, dai progetti piccolo a quelli di grande dimensione, in maniera veloce ed efficiente. Quando installi uno "starter" site di Gatsby, Gatsby usa Git dietro le quinte per scaricare e installare i file necessari al tuo starter. Devi avere Git installato per configurare il tuo primo sito Gatsby.

I passi per scaricare e installare Git dipendono dal tuo sistema operativo. Segui la guida per il tuo sistema:

- [Installa Git su macOS](https://www.atlassian.com/git/tutorials/install-git#mac-os-x)
- [Installa Git su Windows](https://www.atlassian.com/git/tutorials/install-git#windows)
- [Installa Git su Linux](https://www.atlassian.com/git/tutorials/install-git#linux)

## Utilizzo della Gatsby CLI

Lo strumento Gatsby CLI ti permette in modo rapido di creare dei nuovi siti basati su Gatsby ed eseguire comandi per lo sviluppo dei siti Gatsby. È pubblicato come pacchetto npm.

La Gatsby CLI è disponibile tramite npm e dovrebbe essere installata globalmente eseguendo `npm install -g gatsby-cli`.

_**Nota**: quando installi Gatsby ed lo esegui per la prima volta, vedrai un breve messaggio che ti informa sulla raccolta di dati di utilizzo anonimi per i comandi Gatsby, puoi leggere di più su come i dati vengono collezionati e usati nel [documento telemetria](/docs/telemetry)._

Per vedere i comandi disponibili, esegui `gatsby --help`.

![Controlla nel terminale i comandi di gatsby](05-gatsby-help.png)

> 💡 Se non riesci ad eseguire Gatsby CLI a causa di un problema di permessi, prova a controllare il [documento su come sistemare i permessi di npm](https://docs.npmjs.com/getting-started/fixing-npm-permissions), o [questa guida](https://github.com/sindresorhus/guides/blob/master/npm-global-without-sudo.md).

## Crea un sito Gatsby

Ora sei pronto per usare la Gastby CLI per creare il tuo primo sito Gatsby. Usando questo strumento, puoi scaricare gli “starters” (siti costrutiti in modo parziale con una configruazione di base) per aiutarti a creare determinati tipologie di sito in maniera più veloce. Lo starter “Hello World” che utilizzerai qui è uno starter con gli elementi essenziali necessari per un sito Gatsby.

1.  Apri il Terminale.
2.  Esegui `gatsby new hello-world https://github.com/gatsbyjs/gatsby-starter-hello-world`. (_Nota: In base alla velocità della tua connessione, il tempo necessario varierà. Per brevità, la gif di seguito è stata messa in pausa durante parte dell'installazione_)
3.  Esegui `cd hello-world`.
4.  Esegui `gatsby develop`.

<video controls="controls" autoplay="true" loop="true">
  <source type="video/mp4" src="./03-create-site.mp4" />
  <p>Spiacente! Il tuo browser non supporta questo video</p>
</video>

Cosa è appena successo?

```shell
gatsby new hello-world https://github.com/gatsbyjs/gatsby-starter-hello-world
```

- `new` è un comando di gastby che crea un nuovo progetto Gatsby
- Qui, `hello-world` è un titolo di esempio - puoi scegliere quello che vuoi. Lo strumento CLI creerà il codice del tuo nuovo sito in una nuova cartella chiamata “hello-world”.
- Infine, l'URL di GitHub inserito, punta ad un repository che contiene il codice dello starter che vuoi usare.

```shell
cd hello-world
```

- Questo dice 'Io voglio cambiare cartella (`cd`) nella cartella “hello-world”'. Ogni volta che vuoi eseguire qualsiasi comando per il tuo sito, devi essere nel contesto del sito (ovvero, il tuo terminale deve essere puntato alla cartella dove risiede il codice del tuo sito).

```shell
gatsby develop
```

- Questo comando lancia il server di sviluppo. Sarai in grado di vedere e interagire con il tuo nuovo sito in un ambiente di sviluppo - locale (sul tuo computer, non pubblicato su internet)

### Visualizza il tuo sito localmente

Apri una nuova scheda nel tuo browser e visita `http://localhost:8000`

![Controlla l'homepage](04-home-page.png)

Congratulazioni! Questo è l'inizio del tuo primo sito Gatsby! 🎉

Ora puoi vedere il sito in local su `http://localhost:8000` fino a quando il tuo server di sviluppo è attivo. Questo è il processo che hai fatto partire lanciando il comando `gatsby develop`. Per fermare questo processo (o per "fermare il server di sviluppo"), torna alla tua finestra del terminale, tieni premuto il tasto “control“ e premi “c“ (ctrl-c). Per farlo partire nuovamente, lancia di nuovo `gatsby develop`!

**Nota:** Se stai usando una VM come `vagrant` e/o desideri mettere in ascolto il tuo indirizzo IP locale, esegui `gatsby develop - --host = 0.0.0.0`. Ora, il server di sviluppo è in ascolto sia su `http://localhost` che sul tuo IP locale.

## Impostare un editor di codice

Un editor di codice è un programma progettato specificamente per modificare il codice del computer. Ce ne sono tanti fantastici in giro.

### Scaricare VS Code

La documentazione di Gatsby a volte include screenshot che sono stati catturati in VS Code, quindi se non hai ancora un editor di codice preferito, l'uso di VS Code farà in modo che il tuo schermo assomigli agli screenshot nei tutorial e nei documenti. Se scegli di utilizzare VS Code, visita il [sito di VS Code](https://code.visualstudio.com/#alt-downloads) e scarica la versione appropriata per la tua piattaforma.

### Install il plugin Prettier

Consigliamo inoltre di utilizzare [Prettier](https://github.com/prettier/prettier), uno strumento che aiuta a formattare il codice per evitare errori.

Puoi utilizzare Prettier direttamente nel tuo editor utilizzando il [plugin di Prettier per VS Code](https://github.com/prettier/prettier-vscode):

1. Aprire la sezione estensioni su VS Code (Visualizza => Estensioni).
2. Cercare "Prettier - Code formatter".
3. Fare clic su "Installa". (Dopo l'installazione ti verrà chiesto di riavviare VS Code per abilitare l'estensione. Le versioni più recenti di VS Code abiliteranno automaticamente l'estensione dopo il download.)

> 💡 Se non stai utilizzando VS Code, controlla la documentazione di Prettier per le [istruzioni di installazione](https://prettier.io/docs/en/install.html) o per [altre integrazioni con gli editor](https://prettier.io /docs/en/editors.html).

## ➡️ Cos'altro?

Riassumendo, in questa sezione hai:

- Appreso della riga di comando e di come usarla
- Installato e appreso di Node.js e dello strumento CLI npm, del sistema di controllo della versione Git e dello strumento CLI Gatsby
- Generato un nuovo sito Gatsby utilizzando lo strumento Gatsby CLI
- Gestito il server di sviluppo di Gatsby e visitato il tuo sito in locale
- Scaricato un editor per il codice
- Installato un formattatore di codice chiamato Prettier

Ora, passa a [**conoscere i pezzi fondamentali di Gatsby**](/tutorial/part-one/).

## Riferimenti

### Panoramica delle tecnologie di base

Non è necessario essere già un esperto - se non lo sei, non preoccuparti! Imparerai molto nel corso di questa serie di tutorial. Queste sono alcune delle principali tecnologie web che utilizzerai durante la creazione di un sito Gatsby:

- **HTML**: Un linguaggio di markup che ogni browser web è in grado di comprendere. È l'acronimo di HyperText Markup Language. L'HTML fornisce ai tuoi contenuti web una struttura informativa universale, definendo cose come titoli, paragrafi e altro.
- **CSS**: Un linguaggio di presentazione utilizzato per definire l'aspetto del tuo contenuto web (caratteri, colori, layout, ecc.). Sta per Cascading Style Sheets.
- **JavaScript**: Un linguaggio di programmazione che ci aiuta a rendere il web dinamico e interattivo.
- **React**: Una libreria (in JavaScript) per la creazione di interfacce utente. È il framework che Gatsby utilizza per creare pagine e strutturare i contenuti.
- **GraphQL**: Un linguaggio di query che ti consente di inserire dati nel tuo sito web. È l'interfaccia che Gatsby utilizza per la gestione dei dati del sito.

### Cos'è un sito web?

Per un'introduzione completa a cosa sia un sito web -- inclusa un'introduzione a HTML e CSS -- consulta “[**Building your first web page**](https://learn.shayhowe.com/html-css/building-your-first-web-page/)”. È un ottimo posto per iniziare a conoscere il Web. Per un'introduzione più pratica sul [**HTML**](https://www.codecademy.com/learn/learn-html), [**CSS**](https://www.codecademy.com/learn/learn-css), e [**JavaScript**](https://www.codecademy.com/learn/introduction-to-javascript), dai un occhio ai tutorial di Codecademy. [**React**](https://reactjs.org/tutorial/tutorial.html) and [**GraphQL**](http://graphql.org/graphql-js/) hanno anche il loro tutorial introduttivo.

### Ulteriori informazioni sulla riga di comando

Per un'ottima introduzione all'uso della riga di comando, dai un'occhiata al [**Tutorial di Codecademy, Command Line**](https://www.codecademy.com/courses/learn-the-command-line/lessons/navigation/exercises/your-first-command) per utenti Mac e Linux e [**questo tutorial**](https://www.computerhope.com/issues/chusedos.htm) per utenti Windows. Anche se sei un utente Windows, la prima pagina del tutorial Codecademy è una lettura preziosa. Spiega cos'è la riga di comando, non solo come interfacciarsi con essa.

### Ulteriori informazioni su npm

npm è un gestore di pacchetti JavaScript. Un pacchetto è un modulo di codice che puoi scegliere di includere nei tuoi progetti. Se hai appena scaricato e installato Node.js, npm è stato installato con esso!

npm ha tre componenti distinti: il sito Web di npm, il registro di npm e l'interfaccia a riga di comando (CLI) di npm.

- Sul sito web di npm, puoi esplorare quali pacchetti JavaScript sono disponibili nel registro di npm.
- Il registro npm è un ampio database di informazioni sui pacchetti JavaScript disponibili su npm.
- Una volta identificato un pacchetto che desideri, puoi utilizzare la CLI di npm per installarlo nel tuo progetto o globalmente (come altri strumenti CLI). La CLI di npm è la cos che comunica con il registro: generalmente interagisci solo con il sito Web di npm o con la CLI di npm.

> 💡 Dai un'occhiata all'introduzione di npm, "[**What is npm?**](https://docs.npmjs.com/getting-started/what-is-npm)".

### Ulteriori informazioni su Git

Non avrai bisogno di conoscere Git per completare questo tutorial, ma è uno strumento molto utile. Se sei interessato a saperne di più sul controllo della versione, Git e GitHub, dai un'occhiata al [Git Handbook](https://guides.github.com/introduction/git-handbook/) di GitHub.
