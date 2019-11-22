---
title: Dati in Gatsby
typora-copy-images-to: ./
disableTableOfContents: true
---

Benvenuti nella quarta parte del tutorial! Siete a metà strada! Spero che stiate iniziando a sentirvi a proprio agio 😀

## Riepilogo della prima metà del tutorial

Finora, hai imparato ad utilizzare React.js-quanto è utile essere in grado di creare i _propri_ componenti per utilizzarli nei siti web.

Hai anche esplorato lo stile dei componenti usando i moduli CSS.

## Cosa c'è in questo tutorial?

Nelle prossime quattro parti del tutorial (incluso questo), imparerai a conoscere il data layer di Gatsby, una potente funzionalità di Gatsby che ti consente di creare facilmente siti da Markdown, WordPress, CMS headless e molte altre fonti di dati.

**NOTA:** il data layer di Gatsby è basato su GraphQL. Per un'esercitazione approfondita su GraphQL, consigliamo [How to GraphQL](https://www.howtographql.com/).

## Dati in Gatsby

Un sito Web ha quattro parti: HTML, CSS, JS e dati. La prima metà del tutorial si è concentrata sui primi tre. Ora impareremo come utilizzare i dati nei siti Gatsby.

**Cosa sono i dati?**

Una risposta molto informatica sarebbe: i dati sono cose come `"strings"`, numeri interi (`42`), oggetti (`{ pizza: true }`), ecc.

Allo scopo di lavorare con Gatsby, tuttavia, la risposta più giusta è "tutto ciò che vive al di fuori di un componente React".

Finora hai scritto del testo e aggiunto immagini _direttamente_ nei componenti. È un modo _eccellente_ per creare molti siti web.
Ma spesso si desidera archiviare i dati _all'esterno_ dei componenti e quindi caricarli _nel_ componente quando serve.

Se stai costruendo un sito con WordPress (così gli altri collaboratori avranno un'interfaccia per l'aggiunta e la modifica dei contenuti) e Gatsby, i _dati_ per il sito (pagine e post) saranno in WordPress e tu _caricherai_ i dati, quando
necessario, dentro i componenti.

I dati possono anche vivere in file come Markdown, CSV, ecc. nonché in database e API di ogni tipo.

**Il data layer di Gatsby ti consente di estrarre i dati da questi (e da qualsiasi altra fonte)
direttamente nei tuoi componenti**—nella forma che desideri.

## Utilizzo di dati non strutturati vs GraphQL

### Devo usare GraphQL e i source plugin per estrarre i dati nei siti Gatsby?

Assolutamente no! È possibile utilizzare l' API `createPages` per caricare dati non strutturati direttamente nelle pagine Gatsby, anziché tramite il data layer GraphQL. Questa è un'ottima scelta per i siti di piccole dimensioni, mentre GraphQL e i source plugin possono aiutare a risparmiare tempo con siti più complessi.

Consulta la guida sull'[Utilizzo di Gatsby senza GraphQL](/docs/using-gatsby-without-graphql/) per sapere come estrarre i dati nel tuo sito Gatsby usando l' API `createPages` e per vedere un sito di esempio!

### Quando devo usare dati non strutturati rispetto a GraphQL?

Se stai costruendo un sito di piccole dimensioni, un modo efficace per crearlo è quello di inserire dati non strutturati come indicato in questa guida, utilizzando l' API `createPages`, e se il sito diventa più complesso in seguito, o si passa alla creazione di siti più complessi, o desideri trasformare i tuoi dati, segui questi passaggi:

1.  Controlla la [libreria dei plugin](/plugins/) per vedere se i source plugins e/o i transformer plugins che desideri utilizzare esistono già
2.  Se non esistono, leggi la guida per la guida per [creare plugin](/docs/creating-plugins/) e prendi in considerazione l' idea di crearne uno tuo!

### Come il data layer di Gatsby utilizza GraphQL per inserire i dati nei componenti

Esistono molte opzioni per caricare i dati nei componenti di React. Uno dei più popolari e potenti di questi è una tecnologia chiamata [GraphQL](http://graphql.org/).

GraphQL è stato inventato Facebook per aiutare gli ingegneri _a caricare_ i dati necessari nei componenti.

GraphQL è un **q**uery **l**anguage (la parte _QL_ del nome). Se hai familiarità con SQL, funziona in un modo molto simile.
Utilizzando una sintassi speciale, descrivi i dati desiderati nel tuo componente e quindi tali dati ti vengono forniti.

Gatsby utilizza GraphQL per consentire ai componenti di dichiarare i dati di cui hanno bisogno.

## Crea un nuovo sito di esempio

Crea un altro sito per questa parte del tutorial. Costruirai un blog Markdown chiamato "Pandas Eating Lots". È dedicato a mostrare le migliori foto e video dei panda che mangiano molto cibo. Seguendo il tutorial, scoprirai il supporto Markdown di GraphQL e Gatsby.

Apri una nuova finestra del terminale ed esegui i seguenti comandi per creare un nuovo sito Gatsby in una directory chiamata `tutorial-part-four`. Quindi vai alla nuova directory:

```shell
gatsby new tutorial-part-four https://github.com/gatsbyjs/gatsby-starter-hello-world
cd tutorial-part-four
```

Quindi andiamo ad installare alcune dipendenze necessarie alla root del progetto. Utilizzerai il tema Tipografia
"Kirkham", e proverai una libreria CSS-in-JS, ["Emotion"](https://emotion.sh/):

```shell
npm install --save gatsby-plugin-typography typography react-typography typography-theme-kirkham gatsby-plugin-emotion @emotion/core
```

Crea un sito simile a quello che hai concluso nella [Part Three](/tutorial/part-three). Questo sito avrà un componente di layout e due componenti di pagina:

```jsx:title=src/components/layout.js
import React from "react"
import { css } from "@emotion/core"
import { Link } from "gatsby"

import { rhythm } from "../utils/typography"

export default ({ children }) => (
  <div
    css={css`
      margin: 0 auto;
      max-width: 700px;
      padding: ${rhythm(2)};
      padding-top: ${rhythm(1.5)};
    `}
  >
    <Link to={`/`}>
      <h3
        css={css`
          margin-bottom: ${rhythm(2)};
          display: inline-block;
          font-style: normal;
        `}
      >
        Pandas Eating Lots
      </h3>
    </Link>
    <Link
      to={`/about/`}
      css={css`
        float: right;
      `}
    >
      About
    </Link>
    {children}
  </div>
)
```

```jsx:title=src/pages/index.js
import React from "react"
import Layout from "../components/layout"

export default () => (
  <Layout>
    <h1>Amazing Pandas Eating Things</h1>
    <div>
      <img
        src="https://2.bp.blogspot.com/-BMP2l6Hwvp4/TiAxeGx4CTI/AAAAAAAAD_M/XlC_mY3SoEw/s1600/panda-group-eating-bamboo.jpg"
        alt="Group of pandas eating bamboo"
      />
    </div>
  </Layout>
)
```

```jsx:title=src/pages/about.js
import React from "react"
import Layout from "../components/layout"

export default () => (
  <Layout>
    <h1>About Pandas Eating Lots</h1>
    <p>
      We're the only site running on your computer dedicated to showing the best
      photos and videos of pandas eating lots of food.
    </p>
  </Layout>
)
```

```javascript:title=src/utils/typography.js
import Typography from "typography"
import kirkhamTheme from "typography-theme-kirkham"

const typography = new Typography(kirkhamTheme)

export default typography
export const rhythm = typography.rhythm
```

`gatsby-config.js` (deve essere nella root del tuo progetto, non in src)

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [
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

Aggiungi i file sopra e poi esegui `gatsby develop`, come al solito, dovresti vedere quanto segue:

![start](start.png)

Hai un altro piccolo sito con un layout e due pagine.

Ora puoi iniziare a fare qualche query 😋

## La tua prima query GraphQL

Durante la realizzazione di siti, ti consigliamo di riutilizzare dati comuni -- come ad esempio il _titolo del sito_. Guarda la pagina `/about/`. Noterai che ha il titolo (`Pandas Eating Lots`) sia nel componente di layout (l'intestazione del sito) che nella parte `<h1 />` della pagina `about.js` (l'intestazione della pagina).

Ma cosa succede se si desidera modificare il titolo del sito in futuro? Dovresti cercare il titolo in tutti i tuoi componenti e modificarlo. Questo è sia che scomodo che soggetto a errori, specialmente per siti più grandi e complessi. Invece, è possibile memorizzare il titolo in un punto e fare riferimento a tale punto in altri file; modificare il titolo in quell'unico punto, e Gatsby _caricherà_ il titolo aggiornato nei file che hanno il riferimento.

Il posto per questi dati comuni è l'oggetto `siteMetadata` nel file `gatsby-config.js`. Aggiungi il titolo del tuo sito al file `gatsby-config.js`:

```javascript:title=gatsby-config.js
module.exports = {
  // highlight-start
  siteMetadata: {
    title: `Title from siteMetadata`,
  },
  // highlight-end
  plugins: [
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

Riavvia il server di sviluppo.

### Usa una query di pagina

Ora il titolo del sito è disponibile per essere caricato; Aggiungilo al file `about.js` utilizzando una [query di pagina](/docs/page-query):

```jsx:title=src/pages/about.js
import React from "react"
import { graphql } from "gatsby" // highlight-line
import Layout from "../components/layout"

// highlight-next-line
export default ({ data }) => (
  <Layout>
    <h1>About {data.site.siteMetadata.title}</h1> {/* highlight-line */}
    <p>
      We're the only site running on your computer dedicated to showing the best
      photos and videos of pandas eating lots of food.
    </p>
  </Layout>
)

// highlight-start
export const query = graphql`
  query {
    site {
      siteMetadata {
        title
      }
    }
  }
`
// highlight-end
```

Ha funzionato! 🎉

![Page title pulling from siteMetadata](site-metadata-title.png)

La semplice query GraphQL che recupera il `title` nel file `about.js` è questa:

```graphql:title=src/pages/about.js
{
  site {
    siteMetadata {
      title
    }
  }
}
```

> 💡 Nella [parte cinque](/tutorial/part-five/#introducing-graphiql), incontrerai uno strumento che ci consente di esplorare in modo interattivo i dati disponibili tramite GraphQL e aiuta a creare query come quella sopra.

Le query di pagina vivono al di fuori della definizione del componente -- per convenzione alla fine di un file del componente di pagina -- e sono disponibili solo sui componenti di pagina.

### Usa una StaticQuery

[StaticQuery](/docs/static-query/) è una nuova API introdotta in Gatsby v2 che consente ai componenti non di pagina (come il componente `layout.js`), di recuperare dati tramite query GraphQL.
Usiamo la versione hook appena introdotta — [`useStaticQuery`](/docs/use-static-query/).

Vai avanti e apporta alcune modifiche al file `src/components/layout.js` per utilizzare l'hook `useStaticQuery` e un riferimento `{data.site.siteMetadata.title}` che utilizzerà questi dati. Al termine, il file sarà simile al seguente:

```jsx:title=src/components/layout.js
import React from "react"
import { css } from "@emotion/core"
// highlight-next-line
import { useStaticQuery, Link, graphql } from "gatsby"

import { rhythm } from "../utils/typography"
// highlight-start
export default ({ children }) => {
  const data = useStaticQuery(
    graphql`
      query {
        site {
          siteMetadata {
            title
          }
        }
      }
    `
  )
  return (
    // highlight-end
    <div
      css={css`
        margin: 0 auto;
        max-width: 700px;
        padding: ${rhythm(2)};
        padding-top: ${rhythm(1.5)};
      `}
    >
      <Link to={`/`}>
        <h3
          css={css`
            margin-bottom: ${rhythm(2)};
            display: inline-block;
            font-style: normal;
          `}
        >
          {data.site.siteMetadata.title} {/* highlight-line */}
        </h3>
      </Link>
      <Link
        to={`/about/`}
        css={css`
          float: right;
        `}
      >
        About
      </Link>
      {children}
    </div>
    // highlight-start
  )
}
// highlight-end
```

Un altro successo! 🎉

![Page title and layout title both pulling from siteMetadata](site-metadata-two-titles.png)

Perché abbiamo usato due query diverse? Questi esempi sono stati una rapida introduzione ai tipi di query, come sono formattate e dove possono essere utilizzate. Per ora, tieni presente che solo le pagine possono eseguire query sulle pagine. I componenti non di pagina, come Layout, possono utilizzare StaticQuery. La [parte 7](/tutorial/part-seven/) del tutorial spiega questo in modo più approfondito.

Ma ripristiniamo il vero titolo.

Uno dei principi fondamentali di Gatsby è che _i creatori hanno bisogno di una connessione immediata con ciò che stanno creando_ ([grazie a Bret Victor](http://blog.ezyang.com/2012/02/transcript-of-inventing-on-principle/)). In altre parole, quando si apportano modifiche al codice si dovrebbe vedere immediatamente l'effetto di tale modifica. Modifichi un input di Gatsby e vedi il nuovo output apparire sullo schermo.

Quindi quasi ovunque, le modifiche apportate avranno immediatamente effetto. Modifica ancora il file `gatsby-config.js`, questa volta cambiando la parte `title` in "Pandas Eating Lots". La modifica dovrebbe essere visualizzata molto rapidamente nelle pagine del tuo sito.

![Both titles say Pandas Eating Lots](pandas-eating-lots-titles.png)

## Cosa verrà dopo?

Successivamente, imparerai come caricare i dati nel tuo sito Gatsby usando GraphQL con i source plugin nella [parte cinque](/tutorial/part-five/) del tutorial.
