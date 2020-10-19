---
title: "Ricette: Trasformazione dei dati"
tableOfContentsDepth: 1
---

La trasformazione dei dati in Gatsby è gestita da plugin. I plug-in trasformatori prendono i dati recuperati utilizzando i plug-in di origine e li elaborano in qualcosa di più utilizzabile (ad esempio JSON in oggetti JavaScript e altro).

## Trasformare Markdown in HTML

Il plugin `gatsby-transformer-remark` può trasformare i file Markdown in HTML.

### Prerequisiti

- Un sito Gatsby con `gatsby-config.js` e una pagina ʻindex.js`
- Un file Markdown salvato nella directory `src` del tuo sito Gatsby
- Un plugin sorgente installato, come `gatsby-source-filesystem`
- Il plugin `gatsby-transformer-remark` installato

### Indicazioni

1. Aggiungi il plugin del trasformatore nel tuo `gatsby-config.js`:

```js:title=gatsby-config.js
plugins: [
  // not shown: gatsby-source-filesystem for creating nodes to transform
  `gatsby-transformer-remark`
],
```

2. Aggiungi una query GraphQL al file ʻindex.js`del tuo sito Gatsby per recuperare i nodi del `MarkdownRemark`:

```jsx:title=src/pages/index.js
export const query = graphql`
  query {
    allMarkdownRemark {
      totalCount
      edges {
        node {
          id
          frontmatter {
            title
            date(formatString: "DD MMMM, YYYY")
          }
          excerpt
        }
      }
    }
  }
`
```

3. Riavvia il server di sviluppo e apri GraphiQL su `http://localhost:8000/___graphql`. Esplora i campi disponibili nel nodo `MarkdownRemark`.

### Risorse addizionali

- [Tutorial sulla trasformazione di Markdown in HTML](/tutorial/part-six/#transformer-plugins) usando `gatsby-transformer-remark`
- Sfoglia i plug-in Transformer disponibili nella [libreria plug-in Gatsby](/plugins/?=transformer)

## Trasformare le immagini in scala di grigi utilizzando GraphQL

### Prerequisiti

- Un [sito Gatsby](/docs/quick-start) con un file `gatsby-config.js` e una pagina ʻindex.js`
- I pacchetti `gatsby-image`, `gatsby-transformer-sharp` e `gatsby-plugin-sharp` installati
- Un plugin sorgente installato, come `gatsby-source-filesystem`
- Un'immagine (`.jpg`, `.png`, `.gif`, `.svg`, ecc.) Nella cartella `src/images`

### Indicazioni

1. Modifica il tuo file `gatsby-config.js` per generare immagini e configurare i plugin per il livello dati GraphQL di Gatsby. Un approccio comune è quello di estrarli da una directory di immagini usando il plugin `gatsby-source-filesystem`:

```javascript:title=gatsby-config.js

 plugins: [
   {
     resolve: `gatsby-source-filesystem`,
     options: {
       name: `images`,
       path: `${__dirname}/src/images`,
     },
   },
   `gatsby-transformer-sharp`,
   `gatsby-plugin-sharp`,
 ],
```

2. Utilizza una query per ottenere la tua immagine usando GraphQL e applica una trasformazione in scala di grigi all'immagine in linea. Il `relativePath` dovrebbe essere relativo al percorso che hai configurato in `gatsby-source-filesystem`.

```graphql
  query {
     file(relativePath: { eq: "corgi.jpg" }) {
       childImageSharp {
         // highlight-next-line
         fluid(grayscale: true) {
           ...GatsbyImageSharpFluid
         }
       }
     }
   }
```

Nota: puoi trovare questi e altri parametri nel tuo playground GraphQL in `http://localhost:8000/__graphql`

3. Successivamente importa il componente ʻImg` da "gatsby-image". Lo userai all'interno del tuo JSX per visualizzare l'immagine.

```jsx:title=src/pages/index.js
import React from "react"
import { useStaticQuery, graphql } from "gatsby"
import Layout from "../components/layout"
// highlight-next-line
import Img from "gatsby-image"

export default function Home() {
  const data = useStaticQuery(graphql`
    query {
     file(relativePath: { eq: "corgi.jpg" }) {
       childImageSharp {
         // highlight-next-line
         fluid(grayscale: true) {
           ...GatsbyImageSharpFluid
         }
       }
     }
   }
  `)
  return (
    <Layout>
      <h1>I love my corgi!</h1>
      // highlight-start
      <Img
        fluid={data.file.childImageSharp.fluid}
        alt="A corgi smiling happily"
      />
      // highlight-end
    </Layout>
  )
}
```

4. Esegui `gatsby Develop` per avviare il server di sviluppo.

5. Visualizza la tua immagine nel browser: `http://localhost:8000/`

### Risorse addizionali

- [Documenti API, inclusi suggerimenti per query in scala di grigi e in due tonalità](/docs/gatsby-image/#shared-query-parameters)
- [Gatsby Image docs](/docs/gatsby-image/)
- [Esempi di elaborazione delle immagini](https://github.com/gatsbyjs/gatsby/tree/master/examples/image-processing)
