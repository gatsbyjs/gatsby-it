---
title: "Ricette: Pagine e Layouts"
tableOfContentsDepth: 1
---

Aggiungi pagine al tuo sito Gatsby e usa i layouts per gestire gli elementi di pagina comuni.

## Project structure

All'interno di un progetto Gatsby, puoi vedere alcune o tutte delle seguenti cartelle o file:

```
|-- /.cache
|-- /plugins
|-- /public
|-- /src
    |-- /pages
    |-- /templates
    |-- html.js
|-- /static
|-- gatsby-config.js
|-- gatsby-node.js
|-- gatsby-ssr.js
|-- gatsby-browser.js
```

Alcuni files degni di nota con la loro definizione:

- `gatsby-config.js` — configura le opzioni di un sito Gatsby, come i metadati per il titolo di progetto, la descrizione, i plugin, ecc.
- `gatsby-node.js` — usa l'API Node.js di Gatsby per personalizzare ed estendere la configurazione di base del processo di _build_
- `gatsby-browser.js` — personalizza ed estendi le configurazioni di base relative al browser, usando l'API del browser di Gatsby
- `gatsby-ssr.js` — usa l'API di server-side rendering di Gatsby per personalizzare ed estendere la configurazione di base relative al server-side rendering

### Ulteriori risorse

- Per una panoramica di tutti i file e cartelle comuni, leggi la documentazione su [La struttura di un progetto Gatsby](/docs/gatsby-project-structure/)
- Per i comandi di base, dai un occhio alla documentazione sulla [Linea di comando di Gatsby](/docs/gatsby-cli)
- Dai un occhio alla [Guida Veloce di Gatsby](/docs/cheat-sheet/) per avere informazioni utili in un attimo

## Pagine create in modo automatico

Il core di Gatsby trasforma automaticamente in componenti React all'interno di `src/pages` in pagine con un URL.
Per esempio, i componenti in `src/pages/index.js` e `src/pages/about.js` genereranno in automatico la pagina principale del sito (`/`) e la pagina `/about`, in base al loro nome del file.

### Prerequisiti

- Un [sito Gatsby](/docs/quick-start)
- La [CLI di Gatsby](/docs/gatsby-cli) installata

### Istruzioni

1. Crea una cartella `src/pages` se il tuo sito non ne ha già una.
2. Aggiungi un _component_ file nella cartella delle pagine:

```jsx:title=src/pages/about.js
import React from "react"

const AboutPage = () => (
  <main>
    <h1>L'autore</h1>
    <p>Benvenuto sul mio sito Gatsby.</p>
  </main>
)

export default AboutPage
```

3. Lancia `gatsby develop` per far partire il server di sviluppo.
4. Visita la tua nuova pagina nel browser: `http://localhost:8000/about`

### Ulteriori risorse

- [Creare e modificare pagine](/docs/creating-and-modifying-pages/)

## Collegamenti tra le pagine

Il routing in Gatsby si basa sul componente `<Link />`.

### Prerequisiti

- Un sito Gatsby con due pagine componente: `index.js` e`contact.js`
- Il componente `<Link />` di Gatsby
- La [CLI di Gatsby](/docs/gatsby-cli/) per lanciare `gatsby develop`

### Istruzioni

1. Apri il componente della pagina principale (`src/pages/index.js`), importa il componente `<Link />`, aggiungi il componente `<Link />` sopra l'header e aggiungi la proprietà `to` con il valore `"/contact/"` per il percorso:

```jsx:title=src/pages/index.js
import React from "react"
import { Link } from "gatsby"

export default () => (
  <div style={{ color: `purple` }}>
    <Link to="/contact/">Contatti</Link>
    <p>Ciao Mondo.</p>
  </div>
)
```

2. Lancia `gatsby develop` e naviga la pagina principale. Dovresti vedere un link che ti porta alla pagina dei contatti quando selezionato!

> **Nota**: Il componente `<Link />` di Gatsby è un _wrapper_ attorno al [componente Link di `@reach/router`](https://reach.tech/router/api/Link). Per ulteriori informazioni sul componente `<Link />`di Gatsby, consulta [l'API di relativa per `<Link />`](/docs/gatsby-link/).

## Creare un componente di layout

È pratica comune inserire attorno a una pagina un componente React per il _layout_, il quale rende possibile la condivisione del _markup_, stile e funzionalità a cavallo di diverse pagine.

### Prerequisiti

- [Un sito Gatsby](/docs/quick-start/)

### Istruzioni

1. Crea un componente layout in `src/components`, dove i componenti figli saranno passati come prop:

```jsx:title=src/components/layout.js
import React from "react"

export default ({ children }) => (
  <div style={{ margin: `0 auto`, maxWidth: 650, padding: `0 1rem` }}>
    {children}
  </div>
)
```

2. Importa e usa il componente layout nella pagina

```jsx:title=src/pages/index.js
import React from "react"
import { Link } from "gatsby"
import Layout from "../components/layout"

export default () => (
  <Layout>
    <Link to="/contact/">Contatti</Link>
    <p>Ciao mondo.</p>
  </Layout>
)
```

### Ulteriori risorse

- Crea un componente layout nel [tutorial parte terza](/tutorial/part-three/#your-first-layout-component)
- Dai uno stile ai [Componenti di layout](/docs/layout-components/)

## Creare pagine in modo programmatico con createPage

Puoi creare pagine in modo programmatico nel file `gatsby-node.js` usando i metodi che Gatsby mette a disposizione.

### Prerequisiti

- Un [sito Gatsby](/docs/quick-start)
- Un file `gatsby-node.js`

### Istruzioni

1. Dentro `gatsby-node.js`, aggiungi un export per `createPages`

```javascript:title=gatsby-node.js
// highlight-start
exports.createPages = ({ actions }) => {
  // ...
}
// highlight-end
```

2. Destruttura l'azione `createPage` dalle azioni disponibili, in modo da eseguirla da sola, e aggiungi o ricava i dati

```javascript:title=gatsby-node.js
exports.createPages = ({ actions }) => {
  // highlight-start
  const { createPage } = actions
  // pull in or use whatever data
  const dogData = [
    {
      name: "Fido",
      breed: "Sheltie",
    },
    {
      name: "Sparky",
      breed: "Corgi",
    },
  ]
  // highlight-end
}
```

3. Itera attraverso i dati in `gatsby-node.js` e fornisci il _path_, il _template_ e il _context_ (dati che verranno passati nelle _prop_ di pageContext) a `createPage` per ogni iterazione

```javascript:title=gatsby-node.js
exports.createPages = ({ actions }) => {
  const { createPage } = actions

  const dogData = [
    {
      name: "Fido",
      breed: "Sheltie",
    },
    {
      name: "Sparky",
      breed: "Corgi",
    },
  ]
  // highlight-start
  dogData.forEach(dog => {
    createPage({
      path: `/${dog.name}`,
      component: require.resolve(`./src/templates/dog-template.js`),
      context: { dog },
    })
  })
  // highlight-end
}
```

4. Crea un componente React che funga da template per la tua pagina che sarà usato da `createPage`

```jsx:title=src/templates/dog-template.js
import React from "react"

export default ({ pageContext: { dog } }) => (
  <section>
    {dog.name} - {dog.breed}
  </section>
)
```

5. Lancia `gatsby develop` e naviga il percorso di una delle pagine create (per esempio `http://localhost:8000/Fido`) per vedere i dati che hai passato, visualizzarsi nella pagina

### Ulteriori risorse

- Sezione tutorial in [Creare pagine in modo programmatico dai dati](/tutorial/part-seven/)
- Guida di riferimento su come [usare Gatsby senza GraphQL](/docs/using-gatsby-without-graphql/)
- [Repository degli esempi](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-createPage) per questa ricetta
