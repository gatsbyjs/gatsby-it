---
title: Programmatically create pages from data
typora-copy-images-to: ./
disableTableOfContents: true
---

> Questo tutorial fa parte della serie sul data layer di Gatsby. Assicurati di aver letto i tutorial precedenti [part 4](/tutorial/part-four/), [part 5](/tutorial/part-five/), e [part 6](/tutorial/part-six/) prima di continuare qui.

## Cosa c'è in questo tutorial?

Nel tutorial precedente, hai creato una bella pagina principale che interroga i file markdown e produce un elenco di titoli ed estratti dei post del blog. Ma non vuoi solo vedere gli estratti, vuoi pagine concrete per i tuoi file di markdown.

Potresti continuare a creare pagine inserendo i componenti di React in `src/pages`. Tuttavia, ora imparerai come creare _programmaticamente_ pagine da _dati_. Gatsby _non_ si limita a creare pagine da file come molti generatori di siti statici. Gatsby ti consente di utilizzare GraphQL per interrogare i tuoi _dati_ e _mappare_ i risultati della query su _pagine_, tutto in fase di compilazione. Questa è un'idea davvero potente. Esplorerai le sue implicazioni e i modi per usarlo nel resto di questa parte del tutorial.

Iniziamo.

## Creazione di slug per le pagine

Uno ‘slug’ è la parte identificativa univoca di un indirizzo web, come la parte `/tutorial/part-seven` della pagina `https://www.gatsbyjs.com/tutorial/part-seven/`.

Viene anche chiamato ‘path’, ma questo tutorial utilizzerà il termine ‘slug’ per coerenza.

La creazione di nuove pagine prevede due passaggi:

1. Generare il "path" o lo "slug" per la pagina.
2. Creare la pagina.

_**Nota**: Spesso le fonti di dati forniscono direttamente uno slug o un pathname per il contenuto; quando si lavora con uno di questi sistemi (ad es. un CMS), non è necessario creare gli slug da soli come si fa con i file markdown._

Per creare le tue pagine markdown, imparerai a utilizzare due API di Gatsby: [`onCreateNode`](/docs/node-apis/#onCreateNode) e [`createPages`](/docs/node-apis/#createPages). Queste sono due API considerate cavalli da battaglia e che vedrai utilizzate in molti siti e plugin.

Facciamo del nostro meglio per rendere le API Gatsby semplici da implementare. Per implementare un'API, esporta una funzione con il nome dell'API da `gatsby-node.js`.

Quindi, ecco dove lo farai. Nella root del tuo sito, crea un file chiamato `gatsby-node.js`. Quindi aggiungi quanto segue:

```javascript:title=gatsby-node.js
exports.onCreateNode = ({ node }) => {
  console.log(`Node created of type "${node.internal.type}"`)
}
```

Questa funzione `onCreateNode` verrà chiamata da Gatsby ogni volta che viene creato (o aggiornato) un nuovo nodo.

Arresta e riavvia il server di sviluppo. Mentre lo fai, vedrai che alcuni nodi appena creati verranno registrati nella console del terminale.

Nella prossima sezione, utilizzerai questa API per aggiungere slug per le tue pagine markdown ai nodi `MarkdownRemark`.

Cambia la tua funzione in modo che ora registri solo i nodi `MarkdownRemark`.

```javascript:title=gatsby-node.js
exports.onCreateNode = ({ node }) => {
  // highlight-start
  if (node.internal.type === `MarkdownRemark`) {
    console.log(node.internal.type)
  }
  // highlight-end
}
```

Si desidera utilizzare ogni nome dei file markdown per creare lo slug della pagina. Quindi `pandas-and-bananas.md` diventerà `/pandas-and-bananas/`. Ma come si ottiene il nome del file dal nodo `MarkdownRemark`? Per ottenerlo, devi _attraversare_ il "node graph" fino al suo nodo `File` _padre_, siccome i nodi `File` contengono i dati che ti servono sui file su disco. Per farlo, utilizzerai l'helper [`getNode()`](/docs/node-api-helpers/#getNode). Aggiungilo ai parametri della funzione di `onCreateNode` e chiamalo per ottenere il nodo del file:

```javascript:title=gatsby-node.js
// highlight-next-line
exports.onCreateNode = ({ node, getNode }) => {
  if (node.internal.type === `MarkdownRemark`) {
    // highlight-start
    const fileNode = getNode(node.parent)
    console.log(`\n`, fileNode.relativePath)
    // highlight-end
  }
}
```

Dopo aver riavviato il server di sviluppo, dovresti vedere i percorsi relativi per i tuoi due file markdown stampati sullo schermo del terminale.

![path-relativo-markdown](markdown-relative-path.png)

Ora dovrai creare gli slug. Poiché la logica per la creazione di slug dai nomi dei file può diventare complicata, il plugin `gatsby-source-filesystem` viene fornito con una funzione per la creazione di slug. Usiamolo.

```javascript:title=gatsby-node.js
const { createFilePath } = require(`gatsby-source-filesystem`) // highlight-line

exports.onCreateNode = ({ node, getNode }) => {
  if (node.internal.type === `MarkdownRemark`) {
    console.log(createFilePath({ node, getNode, basePath: `pages` })) // highlight-line
  }
}
```

La funzione gestisce la ricerca del nodo `File` padre insieme alla creazione dello slug. Riavvia il server di sviluppo e dovresti vedere registrati sul terminale due slug, uno per ogni file di markdown.

Ora puoi aggiungere i tuoi nuovi slug direttamente sui nodi `MarkdownRemark`. Questo è potente, poiché tutti i dati che aggiungi ai nodi sono disponibili per essere interrogati in seguito con GraphQL. Quindi, sarà facile ottenere lo slug quando arriverà il momento di creare le pagine.

Per fare ciò, utilizzerai una funzione chiamata [`createNodeField`](/docs/actions/#createNodeField) passata alla tua implementazione dell'API. Questa funzione permette di creare campi aggiuntivi sui nodi creati da altri plugin. Solo il creatore originale di un nodo può modificare direttamente il nodo: tutti gli altri plugin (incluso il tuo `gatsby-node.js`) devono utilizzare questa funzione per creare campi aggiuntivi.

```javascript:title=gatsby-node.js
const { createFilePath } = require(`gatsby-source-filesystem`)
// highlight-next-line
exports.onCreateNode = ({ node, getNode, actions }) => {
  const { createNodeField } = actions // highlight-line
  if (node.internal.type === `MarkdownRemark`) {
    // highlight-start
    const slug = createFilePath({ node, getNode, basePath: `pages` })
    createNodeField({
      node,
      name: `slug`,
      value: slug,
    })
    // highlight-end
  }
}
```

Riavvia il server di sviluppo e apri o aggiorna GraphiQL. Quindi esegui questa query GraphQL per vedere i tuoi nuovi slug.

```graphql
{
  allMarkdownRemark {
    edges {
      node {
        fields {
          slug
        }
      }
    }
  }
}
```

Ora che gli slug sono stati creati, puoi creare le pagine.

## Creazione delle pagine

Nello stesso file `gatsby-node.js`, aggiungi quanto segue.

```javascript:title=gatsby-node.js
const { createFilePath } = require(`gatsby-source-filesystem`)

exports.onCreateNode = ({ node, getNode, actions }) => {
  const { createNodeField } = actions
  if (node.internal.type === `MarkdownRemark`) {
    const slug = createFilePath({ node, getNode, basePath: `pages` })
    createNodeField({
      node,
      name: `slug`,
      value: slug,
    })
  }
}

// highlight-start
exports.createPages = async ({ graphql, actions }) => {
  // **Note:** The graphql function call returns a Promise
  // see: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise for more info
  const result = await graphql(`
    query {
      allMarkdownRemark {
        edges {
          node {
            fields {
              slug
            }
          }
        }
      }
    }
  `)

  console.log(JSON.stringify(result, null, 4))
}
// highlight-end
```

Hai aggiunto un'implementazione dell'API [`createPages`](/docs/node-apis/#createPages) che Gatsby chiama in modo che i plugin possano aggiungere pagine.

Come menzionato nell'introduzione a questa parte del tutorial, i passaggi per creare pagine programmaticamente sono:

1. Interroga i dati con GraphQL
2. Mappa i risultati della query sulle pagine

Il codice sopra è il primo passo per creare pagine dal tuo markdown poiché stai usando la funzione `graphql` fornita per interrogare gli slug markdown che hai creato. Quindi stai tenendo il log del risultato della query che dovrebbe essere simile a:

![query-markdown-slugs](query-markdown-slugs.png)

Hai bisogno di una cosa in più oltre ad uno slug per creare pagine: un componente template di pagina. Come tutto in Gatsby, le pagine programmatiche sono alimentate da componenti React. Quando si crea una pagina, è necessario specificare quale componente utilizzare.

Crea una cartella `src/templates`, e quindi aggiungi quanto segue in un file denominato `src/templates/blog-post.js`.

```jsx:title=src/templates/blog-post.js
import React from "react"
import Layout from "../components/layout"

export default function BlogPost() {
  return (
    <Layout>
      <div>Hello blog post</div>
    </Layout>
  )
}
```

Quindi aggiorna `gatsby-node.js`

```javascript:title=gatsby-node.js
const path = require(`path`) // highlight-line
const { createFilePath } = require(`gatsby-source-filesystem`)

exports.onCreateNode = ({ node, getNode, actions }) => {
  const { createNodeField } = actions
  if (node.internal.type === `MarkdownRemark`) {
    const slug = createFilePath({ node, getNode, basePath: `pages` })
    createNodeField({
      node,
      name: `slug`,
      value: slug,
    })
  }
}

exports.createPages = async ({ graphql, actions }) => {
  const { createPage } = actions // highlight-line
  const result = await graphql(`
    query {
      allMarkdownRemark {
        edges {
          node {
            fields {
              slug
            }
          }
        }
      }
    }
  `)

  // highlight-start
  result.data.allMarkdownRemark.edges.forEach(({ node }) => {
    createPage({
      path: node.fields.slug,
      component: path.resolve(`./src/templates/blog-post.js`),
      context: {
        // Data passed to context is available
        // in page queries as GraphQL variables.
        slug: node.fields.slug,
      },
    })
  })
  // highlight-end
}
```

Riavvia il server di sviluppo e le tue pagine verranno create! Un modo semplice per trovare le nuove pagine che crei durante lo sviluppo è andare su un percorso casuale dove Gatsby ti mostrerà in maniera utile un elenco di pagine presenti sul sito. Se vai su `http://localhost:8000/sdf`, vedrai le nuove pagine che hai creato.

![pagine-nuove](new-pages.png)

Visitane una e vedrai:

![hello-world-blog-post](hello-world-blog-post.png)

Che è un po' noioso e non è quello che vuoi. Ora puoi estrarre i dati dal tuo post markdown. Cambia `src/templates/blog-post.js` in:

```jsx:title=src/templates/blog-post.js
import React from "react"
import { graphql } from "gatsby" // highlight-line
import Layout from "../components/layout"

// highlight-start
export default function BlogPost({ data }) {
  const post = data.markdownRemark
  // highlight-end
  return (
    <Layout>
      {/* highlight-start */}
      <div>
        <h1>{post.frontmatter.title}</h1>
        <div dangerouslySetInnerHTML={{ __html: post.html }} />
      </div>
      {/* highlight-end */}
    </Layout>
  )
}

// highlight-start
export const query = graphql`
  query($slug: String!) {
    markdownRemark(fields: { slug: { eq: $slug } }) {
      html
      frontmatter {
        title
      }
    }
  }
`
// highlight-end
```

E…

![post-del-blog](blog-post.png)

Fantastico!

L'ultimo passaggio consiste nel creare un collegamento alle nuove pagine dalla pagina principale.

Ritorna a `src/pages/index.js`, esegui una query per i tuoi slug markdown, e crea i link.

```jsx:title=src/pages/index.js
import React from "react"
import { css } from "@emotion/core"
import { Link, graphql } from "gatsby" // highlight-line
import { rhythm } from "../utils/typography"
import Layout from "../components/layout"

export default function Home({ data }) {
  return (
    <Layout>
      <div>
        <h1
          css={css`
            display: inline-block;
            border-bottom: 1px solid;
          `}
        >
          Amazing Pandas Eating Things
        </h1>
        <h4>{data.allMarkdownRemark.totalCount} Posts</h4>
        {data.allMarkdownRemark.edges.map(({ node }) => (
          <div key={node.id}>
            {/* highlight-start */}
            <Link
              to={node.fields.slug}
              css={css`
                text-decoration: none;
                color: inherit;
              `}
            >
              {/* highlight-end */}
              <h3
                css={css`
                  margin-bottom: ${rhythm(1 / 4)};
                `}
              >
                {node.frontmatter.title}{" "}
                <span
                  css={css`
                    color: #555;
                  `}
                >
                  — {node.frontmatter.date}
                </span>
              </h3>
              <p>{node.excerpt}</p>
            </Link> {/* highlight-line */}
          </div>
        ))}
      </div>
    </Layout>
  )
}

export const query = graphql`
  query {
    allMarkdownRemark(sort: { fields: [frontmatter___date], order: DESC }) {
      totalCount
      edges {
        node {
          id
          frontmatter {
            title
            date(formatString: "DD MMMM, YYYY")
          }
          # highlight-start
          fields {
            slug
          }
          # highlight-end
          excerpt
        }
      }
    }
  }
`
```

Ed ecco fatto! Un blog funzionante, seppur piccolo!

## Sfida

Prova a sperimentare di più con il sito. Prova ad aggiungere altri file markdown. Esplora interrogando altri dati dai nodi `MarkdownRemark` e aggiungendoli alla prima pagina o alle pagine dei post del blog.

In questa parte del tutorial, hai appreso le basi della creazione con il data layer di Gatsby. Hai imparato come _procurarti_ e _trasformare_ i dati usando plugin, come utilizzare GraphQL per _mappare_ i dati sulle pagine e quindi come creare _componenti template di pagina_ in cui eseguire query per i dati per ogni pagina.

## Cosa arriverà dopo?

Ora che hai costruito un sito di Gatsby, dove andrai dopo?

- Condividi il tuo sito Gatsby su Twitter e guarda cosa hanno creato gli altri cercando #gatsbytutorial! Assicurati di menzionare @gatsbyjs nel tuo Tweet e includi l'hashtag #gatsbytutorial :)
- Potresti dare un'occhiata ad alcuni [siti di esempio](https://github.com/gatsbyjs/gatsby/tree/master/examples#gatsby-example-websites)
- Esplorare ulteriori [plugin](/docs/plugins/)
- Guardare cosa [altre persone stanno costruendo con Gatsby](/showcase/)
- Guardare la documentazione sull'[API di Gatsby](/docs/api-specification/), sui [nodi](/docs/node-interface/), o su [GraphQL](/docs/graphql-reference/)
