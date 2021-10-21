---
title: Transformer plugins
typora-copy-images-to: ./
disableTableOfContents: true
---

> Questo tutorial fa parte della serie sul data layer di Gatsby. Assicurati di aver letto i tutorial precedenti [part 4](/tutorial/part-four/) e [part 5](/tutorial/part-five/) prima di continuare qui.

## Cosa c'è in questo tutorial?

Il tutorial precedente ha mostrato come i source plugin portano i dati _nel_ sistema dati di Gatsby. In questo tutorial, imparerai come i transformer plugin _trasformano_ il contenuto non elaborato portato dai source plugin. La combinazione di source plugin e di transformer plugin può gestire tutte le fonti di dati e la trasformazione dei dati di cui potresti aver bisogno durante la creazione di un sito Gatsby.

## Transformer plugin

Spesso, il formato dei dati che ottieni dai source plugin non è quello che desideri utilizzare per creare il tuo sito web. Il filesystem source plugin ti consente di interrogare i dati _riguardo_ ai file, ma cosa succede se vuoi interrogare i dati _dentro_ i file?

Per renderlo possibile, Gatsby supporta i transformer plugin che prendono il contenuto non elaborato dai source plugin e lo _trasformano_ in qualcosa di più utilizzabile.

Ad esempio, i file markdown. Markdown è piacevole da scrivere, ma quando crei una pagina usandolo, è necessario che il markdown diventi HTML.

Aggiungi un file in markdown al tuo sito in `src/pages/sweet-pandas-eating-sweets.md` (questo diventerà il tuo primo post sul blog in markdown) e scopri come _trasformarlo_ in HTML usando i transformer plugin e GraphQL.

```markdown:title=src/pages/sweet-pandas-eating-sweets.md
---
title: "Sweet Pandas Eating Sweets"
date: "2017-08-10"
---

Pandas are really sweet.

Here's a video of a panda eating sweets.

<iframe width="560" height="315" src="https://www.youtube.com/embed/4n0xNbfJLR8" frameborder="0" allowfullscreen></iframe>
```

Una volta salvato il file, guarda di nuovo `/my-files/`: il nuovo file markdown è nella tabella. Questa è una caratteristica molto potente di Gatsby. Come il precedente esempio "siteMetadata", i source plugin possono ricaricare i dati in tempo reale. `gatsby-source-filesystem` è sempre alla ricerca di nuovi file da aggiungere e quando ce ne sono, riesegue le tue query.

Aggiungi un transformer plugin che può trasformare i file markdown:

```shell
npm install gatsby-transformer-remark
```

Poi aggiungilo al `gatsby-config.js` come al solito:

```javascript:title=gatsby-config.js
module.exports = {
  siteMetadata: {
    title: `Pandas Eating Lots`,
  },
  plugins: [
    {
      resolve: `gatsby-source-filesystem`,
      options: {
        name: `src`,
        path: `${__dirname}/src/`,
      },
    },
    `gatsby-transformer-remark`, // highlight-line
    `gatsby-plugin-emotion`,
    {
      resolve: `gatsby-plugin-typography`,
      options: {
        pathToConfigModule: `src/utils/typography`,
      },
    },
  ],
}
```

Riavvia il server di sviluppo, quindi aggiorna (o riapri) GraphiQL e guarda il completamento automatico:

![Schermata di GraphiQL che mostra le nuove opzioni `gatsby-transformer-remark` di completamento automatico](markdown-autocomplete.png)

Seleziona di nuovo "allMarkdownRemark" ed eseguilo come hai fatto per "allFile". Vedrai lì il file markdown che hai aggiunto di recente. Esplora i campi disponibili nel nodo `MarkdownRemark`.

![Schermata di GraphiQL che mostra il risultato di una query](markdown-query.png)

Ok! Si spera che alcune basi stiano iniziando a prendere posto. I source plugin portano i dati _nel_ sistema dati di Gatsby e i _transformer_ plugin trasformano il contenuto non elaborato portato dai source plugin. Questo modello è in grado di gestire tutte le fonti di dati e la trasformazione dei dati di cui potresti aver bisogno durante la creazione di un sito Gatsby.

## Creare un elenco dei file markdown del tuo sito in `src/pages/index.js`

Ora dovrai creare un elenco dei tuoi file markdown in prima pagina. Come molti blog, vuoi avere un elenco di link in prima pagina che puntino a ciascun post del blog. Con GraphQL puoi _interrogare_ l'elenco corrente dei post del blog markdown in modo da non dover mantenere l'elenco manualmente.

Come con la pagina `src/pages/my-files.js`, sostituisci `src/pages/index.js` con quanto segue per aggiungere una query GraphQL con alcuni elementi HTML e di stile iniziali.

```jsx:title=src/pages/index.js
import React from "react"
import { graphql } from "gatsby"
import { css } from "@emotion/core"
import { rhythm } from "../utils/typography"
import Layout from "../components/layout"

export default function Home({ data }) {
  console.log(data)
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
            <h3
              css={css`
                margin-bottom: ${rhythm(1 / 4)};
              `}
            >
              {node.frontmatter.title}{" "}
              <span
                css={css`
                  color: #bbb;
                `}
              >
                — {node.frontmatter.date}
              </span>
            </h3>
            <p>{node.excerpt}</p>
          </div>
        ))}
      </div>
    </Layout>
  )
}

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

Ora la prima pagina dovrebbe apparire così:

![Screenshot della prima pagina](frontpage.png)

Ma il tuo unico post sul blog sembra un po' solitario. Quindi aggiungiamone un altro in `src/pages/pandas-and-bananas.md`

```markdown:title=src/pages/pandas-and-bananas.md
---
title: "Pandas and Bananas"
date: "2017-08-21"
---

Do Pandas eat bananas? Check out this short video that shows that yes! pandas do seem to really enjoy bananas!

<iframe width="560" height="315" src="https://www.youtube.com/embed/4SZl1r2O_bY" frameborder="0" allowfullscreen></iframe>
```

![Prima pagina che mostra due post](two-posts.png)

Che sembra fantastico! Tranne che... l'ordine dei post è sbagliato.

Ma questo è facile da risolvere. Quando si esegue una query su una connessione di qualche tipo, è possibile passare una varietà di argomenti alla query GraphQL. Puoi ordinare (`sort`) e filtrare (`filter`) i nodi, impostare il numero di nodi da saltare (`skip`) e scegliere il limite (`limit`) di quanti nodi recuperare. Con questo potente set di operatori, puoi selezionare tutti i dati che desideri, nel formato che ti serve.

Nella query GraphQL della tua pagina iniziale, sostituisci `allMarkdownRemark` con `allMarkdownRemark(sort: { fields: [frontmatter___date], order: DESC })`. _Nota: Ci sono 3 underscore tra `frontmatter` e `date`._ Salva il file e l'ordinamento dovrebbe essere corretto.

Prova ad aprire GraphiQL e a sperimentare con diverse opzioni di ordinamento. Puoi ordinare la connessione `allFile` insieme ad altre connessioni.

Per ulteriore documentazione sui nostri operatori di query, esplora la nostra [GraphQL reference guide.](/docs/graphql-reference/)

## Sfida

Prova a creare una nuova pagina contenente un post del blog e guarda cosa succede all'elenco dei post del blog sulla pagina principale!

## Cosa arriverà dopo?

Fantastico! Hai appena creato una bella pagina principale in cui stai interrogando i tuoi file markdown e producendo un elenco di titoli ed estratti dei post del blog. Ma non vuoi solo vedere gli estratti, vuoi pagine reali per i tuoi file markdown.

Puoi continuare a creare pagine posizionando i componenti di React in `src/pages`. Tuttavia, imparerai come creare _programmaticamente_ pagine da _dati_. Gatsby _non_ si limita a creare pagine da file come molti generatori di siti statici. Gatsby ti consente di utilizzare GraphQL per interrogare i tuoi _dati_ e _mappare_ i risultati della query su _pagine_ - tutto questo in fase di compilazione. Questa è un'idea davvero potente. Esplorerai le sue implicazioni e i modi per usarlo nel prossimo tutorial, dove imparerai come [creare pagine programmaticamente dai dati](/tutorial/part-seven/).
