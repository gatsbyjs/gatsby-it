---
title: Glossario
disableTableOfContents: true
---

Se sei nuovo di Gatsby ci possono essere molte parole da imparare. Questo glossario mira a darti una panoramica generale dei termini più comuni e del loro significato per dei siti Gatsby.

<HorizontalNavList
items={"ABCDEFGHIJKLMNOPQRSTUVWXYZ".split("")}
slug={props.slug}
/>

## A

### AST

Abstract Syntax Tree: Una rappresentazione ad albero del codice sorgente che viene trovato durante il processo [compilazione](#compiler) tra due linguaggi- Per esempio, [gatsby-transformer-remark](/packages/gatsby-transformer-remark/) crea un AST dal [Markdown](#markdown) per descrivere un documento Markdown in una struttura ad albero usando il _parser_ [Remark](#remark)

### API

Application Programming Interface: Un sistema per una applicazione di comunicare con un altra. Per esempio, un [source plugin](#source-plugin) spesso userà un API per recuperare i propri dati.

### Accessibility

La pratica inclusiva di rimuovere le barriere che impediscono l'interazione o l'accesso ai siti Web da parte delle persone con disabilità. Quando i siti sono progettati, sviluppati e modificati correttamente per garantire l'accessibilità, in genere tutti gli utenti hanno uguale accesso a informazioni e funzionalità. Leggi [Impegno per l'accessibilità di Gatsby](/blog/2019-04-18-gatsby-commitment-to-accessibility/).

## B

### Babel

Uno strumento che ti permette di scrivere molto del [JavaScript](#javascript) moderno, e durante la [build](#build) si ottiene codice [compiled](#compiler) che la maggior parte dei browser web possono capire.

### Backend

La cosa dietro le quinte che il [pubblico](#public) non vede. In genere si fa riferimento al pannello di controllo del tuo [CMS](#cms). Spesso sono basati su linguaggi di programmazione server-side tipo Node.js, PHP, Go, ASP.net, Ruby o Java.

### Build

In Gatsby, è il processo che prende il tuo codice e il tuo contenuto e li mette assieme creando in sito web che può essere pubblicato e visitato. Comunemente denominato _build time_. Vedi anche [backend](#backend) e [server-side](#server-side).

## C

### Cache

Un contenitore di informazioni locali che potrebbero essere usate spesso, in modo che calcoli e ricerce posso essere recuperate pià velocemente da un unico posto. Gatsby usa una cache per salvare le informazioni, in modo che possa creare il tuo sito più velocemente mentre sviluppi, senza la necessità di fare la stessa cosa due volte.

### CLI

Command Line Interface: Una applicazione che gira sul tuo computer tramite la [command line](#command-line) con cui si interagisce tramite tastiera.

Gatsby ha due _command line interface_. Una, [`gatsby`](/docs/gatsby-cli/), per lo sviluppo quotidiano con Gatsby e un altra , [`gatsby-dev`](/contributing/setting-up-your-local-dev-environment/#gatsby-repo-install-instructions), per chi contribuisce al progetto Gatsby.

### Client-side

Si intende per Client-side per operazioni eseguite da un utente tramite il browser in una [relazione client–server](https://en.wikipedia.org/wiki/Client%E2%80%93server_model) in una rete di computer. In Gatsby è da tenere in considerazione quando si [lavora con i pacchetti](/docs/using-client-side-only-packages/) che fanno riferimento su oggetti nel [browser DOM](#dom), tipo `window` o `navigator`. Controlla anche: [server-side](#server-side), [frontend](#frontend) e [backend](#backend).

### CMS

Content Management System: una applicazione con la quale vengono gestiti il tuo contenuto che viene salvato in un database o su file per essere recuperato più tardi. Esempli di Content Management Systems includono WordPress, Drupal, Contentful e Netlify CMS.

### Command Line

Una interfaccia a riga di comando che esegue comandi sul tuo computer. Le applicazione predefinite per Mac e Window sono rispettivamente `Terminal` e `Command Prompt`.

### Compiler

Un compiler è un programma che traduce il codice scritto in un linguaggio in un altro linguaggio. Per esempio [Gatsby](#gatsby) può compilare le applicazioni [React](#react) in file di [HTML](#html) statico.

### Component

I components sono pezzi di codice indipendenti e riusabili creati con [React](#react), che , quando combinati, creano il tuo sito web o la tua app.

A component can include components within it. In fact, [pages](#page) and [templates](#template) are examples of components.

### Config

Il file di configurazione, `gatsby-config.js`, contiene le informazioni per Gatsby sul tuo sito web. Un'opzione comune impostata in config sono i metadati del tuo sito che servono si meta tag per il SEO.

### [Continuous Deployment](/docs/glossary/continuous-deployment)

Continuous deployment (CD) automates the process of releasing changes to your project. A continuous deployment workflow automatically builds and tests your project, and publishes your changes only when they pass the required tests.

### CSS

[CSS](https://developer.mozilla.org/en-US/docs/Web/CSS) sta per Cascading Style Sheets, ed è una parte importante della Piattaforma Web assieme a [HTML](#html) e [JavaScript](#javascript). CSS è il linguaggio per lo stylining delle pagine web progettato per essere il più possibile retro compatibile. Appena una nuova funzionalità viene rilasciata agli utenti finali, i [CSS parser](https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/#CSS_parsing) possono ignorare le funzionalità non più supportate e migliorare con le proprietà che supporta. Il CSS utilizza il _cascading_, fondamentale per lo styling con nuove tecniche tipo [CSS Grid](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout/CSS_Grid_and_Progressive_Enhancement) fornendo allo stesso tempo un fallback per i browser più vecchi. Gatsby supporta molti [approcci allo styling](/docs/styling/), inclusi i normali file CSS, i CSS Module e CSS-in-JS.

## D

### Data Source

Le sorgenti per i contenuti e i dati, tipicamente vengono integrati dentro Gatsby usando i [source plugin](#source-plugin). Spesso come sorgenti si usando [CMS Headless](#headless-cms), ma possono includere Markdown, JSON o file YAML.

### Database

Un database è una collezione di dati o contenuti strutturati. Spesso un [CMS](#cms) salverà in un database usando [tecnologie di backend](#backend). Spesso in Gatsby vi si accede usando un [source plugin](#source-plugin)

### Decoupled

Disaccoppiamento indica la separazione di differenti problemi. In [Gatsby](#gatsby) il più delle volte significa il disaccoppiamento del [frontend](#frontend) dal [backend](#backend), tipo usando [Decoupled Drupal](https://dri.es/how-to-decouple-drupal-in-2019) o [Headless WordPress](https://www.smashingmagazine.com/2018/10/headless-wordpress-decoupled/).

### [Decoupled Drupal](/docs/glossary/decoupled-drupal)

Decoupling refers to the practice of using Drupal as a [headless CMS](#headless-cms). A decoupled Drupal instance functions as a content API that returns JSON for your [frontend](#frontend) to consume.

### Deploy

Il processo di [building](#build) del vostro sito web o app e il caricamento su un [hosting provider](#hosting).

### Development Environment

Si intende [environment](#environment) l'ambiente usato mentre sviluppi il tuo codice. Ci si accede usando la [CLI](#cli) usando `gatsby develop`, e fornisce la visualizzazione di errori extra e cose che ti aiutano a fare il debug prima di creare la versione per la [produzione](#production-environment)

### DOM

Il Document Object Model, normalmente chiamato "il DOM", è una API standard del browser che mette in comunicazione gli script o i linguaggi di programmazione rappresentando la struttura di un documento HTML in memoria. Gli sviluppatori normalmente interagiscono con il DOM attraverso il markup HTML (scritto in [JSX](#jsx) in Gatsby), così come con [React](https://reactjs.org/docs/react-dom.html) e codice [JavaScript vanilla](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction#DOM_and_JavaScript). Un altro importante aspetto dell'utilizzo del DOM al suo massimo potenziale è scrivere markup HTML accessibile per esporre la struttura di una pagina alla tecnologia assistiva

## E

### ECMAScript

ECMAScript (spesso indicato come ES) è una specifica per i linguaggi di scripting. [JavaScript](#javascript) è una implementazione di ECMAScript. Spesso gli sviluppatori si trovano ad usare [Babel](#babel) per compilare codice con le ultime specifiche ECMAScript in un JavaScript ampiamente più supportato.

### Environment

L'ambiente in cui Gatsby gira. Per esempio, quando stai scrivendo il tuo codice, probabilmente vuoi il maggior numeri di informazioni per il debug, ma questo non è desiderabile su un sito web o app pubblicata online. Gatsby può cambiare il suo modo di comportarsi in base all'ambiente in cui si trova.

Di base Gatsby supporta due ambienti, il [development environment](#development-environment) e il [production environment](#production-environment).

### Environment Variables

Le [variabili di ambiente](/docs/environment-variables/) ti permette di modificare il comportamento della tua app in base al proprio [environment](#environment). Per esempio , potresti voler ottenere contenuti da un CMS in staging durante lo sviluppo e collegarti al tuo CMS di produzione quando fai la [build](#build) del tuo sito. Con le variabili di ambiente puoi impostare un diverso URL per ogni ambiente.

## F

### Filesystem

Il modo in cui i file sono organizzati. Con Gatsby significa avere i file nello stesso posto dove si trova il codice del tuo sito web o app, invece che recuperarlo da una [source](#data-source) esterna. In genere Gatsby usa il filesystem per contenuto Markdown, immagini, file di dati e altre assets.

### Frontend

L'interfaccia pubblica del tuo sito web o app, realizzata usando tecnologie web: HTML, CSS e JavaScript. Per maggiori informazioni su come la Piattaforma Web riunisce queste tecnologie, dai un occhio a questo articolo [How Browsers Work](https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/).

## G

### Gatsby

Gastby è un moderno framework per siti web che mette le performance in primo piano in ogni sito web o app, sfruttando le tecnologie web più recenti come [React](#react), [GraphQL](#graphql) e [JavaScript](#javascript) moderno. Gatsby rende semplice la creazione di un esperienza web super veloce senza bisogno di diventare un esperto di performance.

### [GraphQL](/docs/glossary/graphql)

Un [query](#query) language che permette ti recuperare i dati per il tuo sito web o app. È una una [interfaccia che Gatsby usa](/docs/graphql/) per gestire i dati del sito.

## H

### HTML

Un linguaggio di markup che ogni browser web è in grado di capire. Significa Hypertext Markup Language. [HTML](https://developer.mozilla.org/en-US/docs/Web/HTML) fornisce al tuo contenuto web una struttura informativa universale, definendo cose come titoli, paragrafi e altro. È anche la chiave per creare un sito web accessibile.

### [Headless CMS](/docs/glossary/headless-cms)

Un [CMS](#cms) che gestisce solo la parte di backoffice dei contenuti invece di gestire sia frontend che backend. Questo tipo di setup viene anche chiamato [Decoupled](#decoupled).

### [Headless WordPress](/docs/glossary/headless-wordpress)

The practice of using JSON returned from the WordPress REST API as a [headless CMS](#headless-cms). It allows you to use WordPress to write and edit content that can be consumed by any client capable of parsing JSON.

### Hosting

Un hosting provider conserva una copia del tuo sito o app e la rende accessibile al [pubblico](#public). [Alcuni hosting provider tipici per Gatsby](/docs/deploying-and-hosting/) includono Netlify, AWS, S3, Surge, Heroku, e molti altri.

### Hot module replacement

Una funzionalità presente mentre sta girando `gatsby develop` è, al salvataggio del codice in un editor di testo, l'aggiornamento automatico del tuo sito rimpiazzando automaticamente i moduli o i chunks di codice in una finestra aperta del browser.

### Hydration

Una volta che è il sito è stato [generato](#build) da Gatsby e caricato in un browser, gli asset Javascript lato client verranno scaricati e renderanno il sito in una completa applicazione React in grado di manipolare il [DOM](#dom). Questo processo viene spesso chiamato re-hydration in quanto esegue parte dello stesso codice JavaScript utilizzato per generare le pagine di Gatsby, ma questa volta con la disponibilità delle API DOM del browser come `window` per esempio.

## I

### Inference

As part of its data layer and [build](#build) process, Gatsby will automatically **infer** a [schema](#schema), or type-based structure, based on available data sources (e.g. Markdown file nodes, WordPress posts, etc.). More control can be gained over this structure by using Gatsby's [Schema Customization API](/docs/schema-customization/).

## J

### [JAMStack](/docs/glossary/jamstack)

Il termine JAMStack si riferisce ad una moderna architettura web composta da [JavaScript](#javascript), [API](#api), e il markup ([HTML](#html)). Da [JAMStack.org](https://jamstack.org): "È un nuovo modo di creare siti web e applicazioni che offre migliori prestazioni, maggior sicurezza, un basso costo sulla scalabilità e una miglior esperienza di sviluppo"

### JavaScript

Un linguaggio di programmazione che ci aiuta a creare delle pagine web interattive. Il [JavaScript](https://developer.mozilla.org/en-US/docs/Web/Javascript) è una tecnologia web ampiamente diffusa nei browser. È anche usata server side tramite [Node.js](#node). È una implementazione della specifica [ECMAScript](#ECMAScript).

### JSX

JSX è una estensione di JavaScript che permette agi sviluppatori di scrivere HTML e componenti personalizzati nello stesso codice. Il [team di React ne raccomanda](https://reactjs.org/docs/introducing-jsx.html) l'uso per definire come deve apparire la [UI](#UI). JSX dovrebbe essere per te un linguaggio di template, ma dotato del piena potenza di JavaScript. Alcuni cose importanti da tenere in considerazione visto che JSX usa JavaScript, è che alcuni attributi HTML nel tuo markup devono essere sostituiti a causa delle parole riservate in JavaScript (come per esempio `htmlFor` e `className`)

## K

## L

### Linting

Si intende per Linting la procedura di eseguire un programma che analizzerà il codice per la ricerca di potenziali errori. Il progetto Gatsby usa [prettier](https://prettier.io/) per identificare e sistemare alcuni comuni problemi di stile. Un altro esempio comune di linter usando nei progetti React è [eslint-jsx-plugin-a11y](https://github.com/evcohen/eslint-plugin-jsx-a11y), il quale controlla i problemi più comuni per l'[accessibilità](accessibility) durante lo sviluppo.

## M

### MDX

Estende il [Markdown](#markdown) per aggiungere il supporto ai [componenti](#component) [React](#react) nel tuo contenuto

### Markdown

Un sistema per scrivere contenuto HTML usando testo semplice, facendo uso di caratteri speciali per identificare il tipo di contenuto come il cancelletto per i [titoli](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/Heading_Elements), e il sottolineato e l'asterisco per enfatizzare il testo.

## N

### NPM

[Node](#node) [Package](#package) Manager. Ti permette di installare e aggiornare gli altri pacchetti da cui il tuo progetto dipende. Un esempio di dipendenze del tuo progetto sono [Gatsby](#gatsby) e [React](#react). Dai un occhio anche a: [Yarn](#yarn).

### Node

Gastby usa dei [data nodes](/docs/node-interface/) per rappresentare una singola porzione di dato. Una sorgente di dati creerà nodi multipli.

### [Node.js](/docs/glossary/node)

Un programma che ti permette di eseguire [JavaScript](#javascript) sul tuo computer. Node è il motore di Gatsby.

## O

## P

### Package

Un pacchetto normalmente descrive un programma [JavaScript](#javascript) che contiene informazioni addizionali su come dovrebbe essere distribuito e usato, come il numero di versione. [NPM](#npm) e [Yarn](#yarn) gestiscono e installano i pacchetti usati dal tuo progetto. [Gatsby](#gatsby) è esso stesso un pacchetto.

### Page

Una pagina [HTML](#html).

Viene anche spesso fatto riferimento ai [componenti](#component) che risiedono in `/src/pages/` e che sono convertiti in pagine da [Gatsby](#gatsby), nonché alle [pagine create in modo dinamico](/docs/creating-and-modifying-pages/#creating-pages-in-gatsby-nodejs) nel tuo file `gatsby-node.js`.

### Plugin

Codice supplementare che aggiunge a Gatsby delle funzionalità che non erano incluse di base. I [plugin di Gatsby](/plugins/) includono i [source](#source-plugins) e [transformer](#transformer) plugin per recuperare e manipolare i dati.

### Production Environment

L'ambiente della versione generata del sito o app, che viene utilizzata dall'utente una volta fatto il deploy. Vi si può accedere tramite [CLI](#cli) usando `gatsby build` o `gatsby serve`.

### Programmatically

Qualcosa che accade automaticamente in base al tuo codice e a una configurazione. Per esempio, potresti configurare il tuo progetto per creare una pagina per ogni post del tuo blog, o leggere e mostrare l'anno corrente come parte del copyright nel footer del tuo sito.

### Progressive enhancement

Il Progressive enhancement (Miglioramento progressivo) è una strategia atta ad accentuare il contenuto principale di una pagina, caricata dal server, prima di qualsiasi altra cosa, senza la necessità di [JavaScript](#javascript). Questa strategia successivamente aggiunge progressivamente al contenuto più complessi livelli di presentazione e funzionalità, in base a quanto è permesso dal browser / dalla connessione dell'utente. L'approccio di base di Gatsby nel [generare](#build) pagine significa che il contenuto verrà caricato prima e arricchito successivamente dallo scaricamento ed esecuzione di script.

### Public

Normalmente si riferisce sia a un utente esterno (non del tuo team) o alla cartella `/public` nella quale il tuo sito viene [generato](#build) e salvato.

## Q

### Query

Il processo per la richiesta specifica di dati da qualcosa. Con Gatsby in genere farai una query con [GraphQL](#graphql).

## R

### [React](/docs/glossary/react)

Una libreria (scritta con [JavaScript](#javascript)) per creare interfacce utente. È un framework che usa [Gatsby](#gatsby) per generare le pagine e strutturare il contenuto.

### Remark

Un interprete che traduce il [Markdown](#markdown) in altri formati come l'[HTML](#html) o il codice [React](#react).

### Runtime

Si intende runtime un programma quando è in esecuzione (o è eseguibile); può essere riferito ad alcune cose. [Node.js](#nodejs) è un runtime [server-side](#server-side) che esegue codice JavaScript. il [JavaScript Client-side](#client-side),in latre parole, si riferisce al runtime del browser dove il JavaScript e normalmente eseguito. Gatsby genera il tuo sito a [built time](#built) e lo [idrata con il runtime di React](#hydration) per fornire un esperienza utente veloce, interattiva e dinamica.

### Routing

Il routing è il meccanismo per il caricamento del contenuto corretto in un sito web o applicazioni, basato su una richiesta di rete - normalmente un URL. Per esempio, permette di indirizzare un indirizzo come `/about-us` alla [pagina](#page), [template](#template) e [componente](#template) corrispondente.

## S

### Schema

Una rappresentazione esatta di come i dati sono salvati in un sistema, come tabelle e campi in un database o la struttura di un file JSON. In Gatsby , lo schema GraphQL esprime tutti i dati a cui poter fare una query - o ai dati che un componente può richiedere tramite il data layer di Gatsby.

### Server-side

La parte server-side della [relazione client-server](https://en.wikipedia.org/wiki/Client%E2%80%93server_model) si riferisce alle operazioni eseguite da un computer che gestisce gli accessi verso una risorsa centralizzata o a un servizio in una rete di computer. Gatsby usa la tecnologia server-side [Node.js](#nodejs) per generare le pagine a build time, al contrario di servirle a [browser runtime](#runtime) usando il JavaScript [client-side](#client-side). Dai un occhio anche a: [frontend](#frontend) e [backend](#backend).

### Source Code

Il codice sorgente è il codice che risiede nella cartella `/src/` e definisce l'aspetto del tuo sito web o app. È formato da [JavaScript](#javascript) e qualche volta da [CSS](#css) e altri file.

Il codice sorgente viene usato per [generare](#build) il sito che gli utenti vedranno.

### Source Plugin

Un [plugin](#plugin) che aggiunge a Gatsby ulteriori [data sources](#data-source) che posso essere poi [interrogati](#query) dalle tue [pagine](#page) e [componenti](#component).

### Starter

Un progetto Gatsby già configurato che può essere usato come base per il tuo progetto. Possono essere trovati usando la [Gatsby Starter Library](/starters/) e installati usando la [CLI di Gatsby](/docs/starters/).

### Static

Gatsby [genera](#build) una versione statica della tua pagina che può essere facilmente [servita](#hosting). Questo in contrasto coi sistemi dinamici nei quali ogni pagina è generata al volo. Essere statica offre grandi vantaggi in termini di prestazioni, perchè il lavoro è necessario farlo solo quando il contenuto o il codice cambia.

Fa riferimento anche alla cartella `/static` che è automaticamente copiata dentro `/public` ad ogni [generazione](#build) per i file per i quali non serve che vengano processati da Gatsby ma che necessitano di essere pubblicati.

### [Static Site Generator](/docs/glossary/static-site-generator)

A software application that creates HTML pages from templates or [components](#component) and a given content source.

## T

### Template

Un [componente](#component) che viene [programmaticamente](#programmatically) trasformato in una pagina di Gatsby.

### Theme

Un tema di Gatsby è come un tema di Worpdress che è componibile (con altri temi), estendibile (con ulteriori funzionalità) e sostituibili tramite ([oscuramento](/blog/2019-04-29-component-shadowing/)). I temi di Gatsby, pacchettizzate al loro interno, possono avere qualsiasi funzione di una app Gatsy, e possono anche rendere disponibile qualsiasi numero di opzioni che possono essere accese o spente.

### Transformer

Un [plugin](#plugin) che trasforma un tipo di dato in un altro. Per esempio potresti voler trasformare un foglio di calcolo in un array [JavaScript](#javascript).

## U

### UI

La UI fa riferimento alla User Interface (interfaccia utente). Nel campo dell'interazione uomo-computer, la UI è uno spazio dove avvengono le interazioni tra uomini e macchine. Lo scopo di queste interazioni è quello di permettere l'esecuzione di efficaci controlli ed operazioni sulla macchina dalla parte umana, mentre la macchina fornisce simultaneamente informazioni che aiutano il processo decisionale dell'utente (come messaggi di errore o notifiche).

## V

## W

### [webpack](/docs/glossary/webpack)

Un'applicazione [JavaScript] (# javascript) utilizzata da Gatsby generare gli artefatti del tuo sito web. Avviene automaticamente durante la [generazione](# build).

## X

## Y

### Yarn

Un gestore di [pacchetti](#package) che qualcuno preferisce a [NPM](#npm). È un requisito per lo [sviluppo di Gatsby](/contributing/setting-up-your-local-dev-environment/#using-yarn).

## Z
