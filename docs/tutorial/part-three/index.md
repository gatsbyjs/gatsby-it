---
title: Creazione di layout con componenti nidificati
typora-copy-images-to: ./
disableTableOfContents: true
---

Benvenuti nella terza parte!

## Cosa c'è in questo tutorial?

In questa parte, imparerai a conoscere i plugin di Gatsby ed a creare componenti di "layout".

I plugin Gatsby sono pacchetti JavaScript che aiutano ad aggiungere funzionalità a un sito Gatsby. Gatsby è progettato per essere estensibile, il che significa che i plugin sono in grado di estendere e modificare praticamente tutto ciò che fa Gatsby.

I componenti di layout sono sezioni del tuo sito che desideri condividere su più pagine. Ad esempio, i siti avranno comunemente un componente di layout con un'intestazione e un piè di pagina condivisi. Altre cose comuni da aggiungere ai layout sono una barra laterale e/o un menu di navigazione. In questa pagina, ad esempio, l'intestazione nella parte superiore fa parte del componente di layout di gatsbyjs.org.

Immergiamoci nella terza parte.

## Uso dei plugin

Probabilmente hai già familiarità con l'idea dei plugin. Molti software supportano l'aggiunta di plugin personalizzati per aggiungere nuove funzionalità o persino per modificare il funzionamento principale del software. I plugin di Gatsby funzionano allo stesso modo.

I membri della community (come te!) possono contribuire con plugin (piccole quantità di codice JavaScript) che altri utenti possono utilizzare durante la creazione di siti con Gatsby.

> Esistono già centinaia di plugin! Esplora la [libreria dei plugin](/plugins/) Gatsby.

Il nostro obiettivo dei plugin è renderli semplici da installare ed utilizzare. Probabilmente utilizzerai i plugin in quasi tutti i siti Gatsby che crei. Durante il resto del tutorial avrai molte opportunità di esercitarti nell'installazione e nell'uso dei plugin.

Per una prima introduzione all'uso dei plugin, installeremo e implementeremo il plugin Gatsby per Typography.js.

[Typography.js](https://kyleamathews.github.io/typography.js/) è una libreria JavaScript che genera stili di base globali per la tipografia del tuo sito. La libreria ha un [plugin Gatsby dedicato](/packages/gatsby-plugin-typography/) per semplificarne l'uso in un sito Gatsby.

### ✋ Crea un nuovo sito Gatsby

Come accennato nella [seconda parte](/tutorial/part-two/), a questo punto è probabilmente una buona idea chiudere le finestre dei terminali e i file di progetto del precedente tutorial, per mantenere le cose pulite sul desktop. Ora si può aprire una nuova finestra del terminale ed eseguire i seguenti comandi per creare un nuovo sito Gatsby in una cartella chiamata `tutorial-part-three` e quindi passare a questa nuova cartella:

```shell
gatsby new tutorial-part-three https://github.com/gatsbyjs/gatsby-starter-hello-world
cd tutorial-part-three
```

### ✋ Installa e configura `gatsby-plugin-typography`

Esistono due passaggi principali per l'utilizzo di un plugin: l'installazione e la configurazione.

1. Installa il pacchetto NPM `gatsby-plugin-typography`.

```shell
npm install --save gatsby-plugin-typography react-typography typography typography-theme-fairy-gates
```

> Nota: Typography.js richiede alcuni pacchetti aggiuntivi, quindi sono inclusi nelle istruzioni. Requisiti aggiuntivi come questi saranno elencati nelle istruzioni di "installazione" di ciascun plugin.

2. Modifica il file `gatsby-config.js` alla radice del tuo progetto nel seguente modo:

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [
    {
      resolve: `gatsby-plugin-typography`,
      options: {
        pathToConfigModule: `src/utils/typography`,
      },
    },
  ],
}
```

`gatsby-config.js` è un altro file speciale che Gatsby riconoscerà automaticamente. Qui è dove aggiungerai plugin e altre configurazioni del sito.

> Leggi la [documentazione su gatsby-config.js](/docs/gatsby-config/) per saperne di più, se lo desideri.

3. Typography.js necessita di un file di configurazione. Crea una nuova cartella chiamata `utils` nella cartella `src`. Quindi aggiungi un nuovo file chiamato `typography.js` in `utils` e copia il seguente contenuto nel file:

```javascript:title=src/utils/typography.js
import Typography from "typography"
import fairyGateTheme from "typography-theme-fairy-gates"

const typography = new Typography(fairyGateTheme)

export const { scale, rhythm, options } = typography
export default typography
```

4. Avvia il server di sviluppo.

```shell
gatsby develop
```

Una volta caricato il sito, se controlli il codice HTML generato utilizzando gli strumenti di sviluppo di Chrome, vedrai che il plugin di tipografia ha aggiunto un elemento `<style>` nell'elemento `<head>` con il CSS che ha generato:

![typography-styles](typography-styles.png)

### ✋ Apporta alcune modifiche al contenuto e allo stile

Copia quanto segue nel file `src/pages/index.js` in modo da poter vedere meglio l'effetto del CSS generato da Typography.js.

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  <div>
    <h1>Hi! I'm building a fake Gatsby site as part of a tutorial!</h1>
    <p>
      What do I like to do? Lots of course but definitely enjoy building
      websites.
    </p>
  </div>
)
```

Il tuo sito ora dovrebbe apparire così:

![no-layout](no-layout.png)

Facciamo un rapido miglioramento. Molti siti hanno una singola colonna di testo centrata al centro della pagina. Per creare questo, aggiungi i seguenti stili all'elemento `<div>` nel file `src/pages/index.js`.

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  // highlight-next-line
  <div style={{ margin: `3rem auto`, maxWidth: 600 }}>
    <h1>Hi! I'm building a fake Gatsby site as part of a tutorial!</h1>
    <p>
      What do I like to do? Lots of course but definitely enjoy building
      websites.
    </p>
  </div>
)
```

![with-layout2](with-layout2.png)

Ottimo. Hai installato e configurato il tuo primissimo plugin Gatsby!

## Creazione di componenti di layout

Iniziamo ora a conoscere i componenti di layout. Per prepararti a questa parte, aggiungi un paio di nuove pagine al tuo progetto: una pagina di informazioni e una pagina di contatto.

```jsx:title=src/pages/about.js
import React from "react"

export default () => (
  <div>
    <h1>About me</h1>
    <p>I’m good enough, I’m smart enough, and gosh darn it, people like me!</p>
  </div>
)
```

```jsx:title=src/pages/contact.js
import React from "react"

export default () => (
  <div>
    <h1>I'd love to talk! Email me at the address below</h1>
    <p>
      <a href="mailto:me@example.com">me@example.com</a>
    </p>
  </div>
)
```

Vediamo come appare la nuova pagina:

![about-uncentered](about-uncentered.png)

Hmm. Sarebbe bello se il contenuto delle due nuove pagine fosse centrato come la prima pagina. E sarebbe bello avere una sorta di navigazione globale per permettere ai visitatori di trovare e visitare ciascuna delle sottopagine.

And it would be nice to have some sort of global navigation so it's easy for visitors to find and visit each of the sub-pages.

Affronterai queste modifiche creando il tuo primo componente di layout.

### ✋ Crea il tuo primo componente di layout

1. Crea una nuova cartella `src/components`.

2. Crea un componente di layout molto semplice nel file `src/components/layout.js`:

```jsx:title=src/components/layout.js
import React from "react"

export default ({ children }) => (
  <div style={{ margin: `3rem auto`, maxWidth: 650, padding: `0 1rem` }}>
    {children}
  </div>
)
```

3. Importa questo nuovo componente di layout nel file `src/pages/index.js`:

```jsx:title=src/pages/index.js
import React from "react"
import Layout from "../components/layout" // highlight-line

export default () => (
  <Layout> {/* highlight-line */}
    <h1>Hi! I'm building a fake Gatsby site as part of a tutorial!</h1>
    <p>
      What do I like to do? Lots of course but definitely enjoy building
      websites.
    </p>
  </Layout> {/* highlight-line */}
)
```

![with-layout2](with-layout2.png)

Ottimo, il layout funziona! Il contenuto della pagina iniziale è ancora centrato.

Ma prova a navigare in `/about/` o `/contact/`. Il contenuto di quelle pagine non sarà ancora centrato.

4. Importa il componente layout nei file `about.js` e `contact.js` (come hai fatto per `index.js` nel passaggio precedente).

Il contenuto di tutte e tre le tue pagine è centrato grazie a questo singolo componente di layout condiviso!

### ✋ Aggiungi un titolo al sito

1. Aggiungi la seguente riga al tuo nuovo componente di layout:

```jsx:title=src/components/layout.js
import React from "react"

export default ({ children }) => (
  <div style={{ margin: `3rem auto`, maxWidth: 650, padding: `0 1rem` }}>
    <h3>MySweetSite</h3> {/* highlight-line */}
    {children}
  </div>
)
```

Se navighi in una qualsiasi delle tue tre pagine, vedrai lo stesso titolo che abbiamo aggiunto, ad esempio la pagina `/about/`:

![with-title](with-title.png)

### ✋ Aggiungi collegamenti di navigazione tra le pagine

1. Copia quanto segue nel file del componente di layout:

```jsx:title=src/components/layout.js
import React from "react"
// highlight-start
import { Link } from "gatsby"

const ListLink = props => (
  <li style={{ display: `inline-block`, marginRight: `1rem` }}>
    <Link to={props.to}>{props.children}</Link>
  </li>
)
// highlight-end

export default ({ children }) => (
  <div style={{ margin: `3rem auto`, maxWidth: 650, padding: `0 1rem` }}>
    {/* highlight-start */}
    <header style={{ marginBottom: `1.5rem` }}>
      <Link to="/" style={{ textShadow: `none`, backgroundImage: `none` }}>
        <h3 style={{ display: `inline` }}>MySweetSite</h3>
      </Link>
      <ul style={{ listStyle: `none`, float: `right` }}>
        <ListLink to="/">Home</ListLink>
        <ListLink to="/about/">About</ListLink>
        <ListLink to="/contact/">Contact</ListLink>
      </ul>
    </header>
    {/* highlight-end */}
    {children}
  </div>
)
```

![with-navigation2](with-navigation2.png)

E il gioco è fatto! Un sito di tre pagine con navigazione globale di base.

_Sfida:_ Con il tuo nuovo potere "componenti di layout", prova ad aggiungere intestazioni, piè di pagina, navigazione globale, barre laterali, ecc. ai tuoi siti Gatsby!

## Cosa verrà dopo?

Passa alla [quarta parte del tutorial](/tutorial/part-four/) in cui inizierai a conoscere il data layer di Gatsby e a creare programmaticamente pagine!
