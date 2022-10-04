---
title: Preparazione di un sito per la pubblicazione
typora-copy-images-to: ./
disableTableOfContents: true
---

Wow! Hai fatto molta strada! Hai imparato a:

- creare nuovi siti Gatsby
- creare pagine e componenti
- dare uno stile ai componenti
- aggiungere plugin ad un sito
- ottenere e trasformare i dati
- usare GraphQL per effettuare query sui dati per le pagine
- creare pagine programmaticamente dai tuoi dati

In questa sezione finale, seguirai alcuni passaggi comuni per preparare un sito alla pubblicazione introducendo un potente strumento diagnostico per il sito chiamato [Lighthouse](https://developers.google.com/web/tools/lighthouse/). Lungo la strada, introdurremo alcuni altri plugin che spesso vorrai utilizzare nei tuoi siti Gatsby.

## Audit con Lighthouse

Citando il [sito web di Lighthouse](https://developers.google.com/web/tools/lighthouse/):

> Lighthouse is an open-source, automated tool for improving the quality of web pages. You can run it against any web page, public or requiring authentication. It has audits for performance, accessibility, progressive web apps, SEO and more.

Che tradotto significa:

> Lighthouse è uno strumento automatizzato open source per migliorare la qualità delle pagine web. Puoi eseguirlo su qualsiasi pagina Web, pubblica o che richiede l'autenticazione. Dispone di audit per prestazioni, accessibilità, progressive web app, SEO e altro ancora.

Lighthouse è incluso nei Chrome DevTools. L'esecuzione del suo audit, e quindi la correzione degli errori rilevati e l'implementazione dei miglioramenti suggeriti, è un ottimo modo per preparare il tuo sito alla pubblicazione. Ti aiuta a darti la confidenza che il tuo sito sia il più veloce e accessibile possibile.

Provalo!

Innanzitutto, devi creare una build di produzione del tuo sito Gatsby. Il server di sviluppo Gatsby è ottimizzato per velocizzare lo sviluppo; ma il sito che genera, pur somigliando molto a una versione di produzione del sito, non è così ottimizzato.

### ✋ Creare una build di produzione

1. Arresta il server di sviluppo (se è ancora in esecuzione) ed esegui il comando seguente:

```shell
gatsby build
```

> 💡 Come hai appreso nella [parte 1](/tutorial/part-one/), questo crea una build di produzione del tuo sito e posiziona i file statici costruiti nella directory `public`.

2. Visualizza il sito nella versione di produzione in locale. Esegui:

```shell
gatsby serve
```

Once this starts, you can view your site at `http://localhost:9000`.

### Esegui un'audit di Lighthouse

Ora eseguirai il tuo primo test con Lighthouse.

1. Se non l'hai già fatto, apri il sito in modalità di navigazione in incognito di Chrome in modo che nessuna estensione interferisca con il test. Quindi, apri Chrome DevTools.

2. Fai clic sulla scheda "Audit" dove vedrai una schermata simile a questa:

![Avvio audit Lighthouse](./lighthouse-audit.png)

3. Fai clic su "Esegui un audit..." (tutti i tipi di audit disponibili dovrebbero essere selezionati per impostazione predefinita). Quindi fare clic su "Esegui audit". (Ci vorrà quindi circa un minuto per eseguire l'audit). Una volta completato l'audit, dovresti vedere i risultati simili a questi:

![Risultati audit Lighthouse](./lighthouse-audit-results.png)

Come puoi vedere, le prestazioni di Gatsby sono eccellenti, ma ti mancano alcune cose per PWA, Accessibilità, Best Practices e SEO che miglioreranno i tuoi punteggi (e renderanno il tuo sito molto più amichevole per i visitatori e i motori di ricerca).

## Aggiungere un file manifest

Sembra che tu abbia un punteggio piuttosto scarso nella categoria "Progressive Web App". Risolviamolo.

Ma prima, cosa _sono_ esattamente le PWA?

Sono regolari siti web che sfruttano le moderne funzionalità del browser per aumentare l'esperienza web con funzionalità e vantaggi simili ad un'app. Dai un'occhiata alla [panoramica di Google](https://developers.google.com/web/progressive-web-apps/) di ciò che definisce un'esperienza PWA.

L'inclusione di un manifest di una web app è uno dei tre [requisiti di base per una PWA](https://alistapart.com/article/yes-that-web-project-should-be-a-pwa#section1) generalmente accettati.

Citando [Google](https://developers.google.com/web/fundamentals/web-app-manifest/):

> The web app manifest is a JSON file that tells the browser about your Progressive Web App and how it should behave when installed on the user's desktop or mobile device. A typical manifest file includes the app name, the icons the app should use, and the URL that should be opened when the app is launched, among other things.

Che tradotto significa:

> Il manifest della web app è un file JSON che informa il browser della progressive web app e di come dovrebbe comportarsi una volta installata sul desktop o sul dispositivo mobile dell'utente. Un tipico file manifest include, tra le altre cose, il nome dell'app, le icone che l'app deve utilizzare e l'URL che deve essere aperto all'avvio dell'app.

[Il manifest plug-in di Gatsby](/packages/gatsby-plugin-manifest/) configura Gatsby per creare un file `manifest.webmanifest` su ogni build del sito.

### ✋ Usare `gatsby-plugin-manifest`

1. Installa il plugin:

```shell
npm install gatsby-plugin-manifest
```

2. Aggiungi una favicon per la tua app in `src/images/icon.png`. Ai fini di questo tutorial puoi utilizzare [questa icona di esempio](https://raw.githubusercontent.com/gatsbyjs/gatsby/master/docs/tutorial/part-eight/icon.png), se non ne avessi una disponibile. L'icona è necessaria per creare tutte le immagini per il manifest. Per ulteriori informazioni, guarda la documentazione per [`gatsby-plugin-manifest`](https://github.com/gatsbyjs/gatsby/blob/master/packages/gatsby-plugin-manifest/README.md).

3. Aggiungi il plugin all'array `plugins` nel tuo file `gatsby-config.js`.

```javascript:title=gatsby-config.js
{
  plugins: [
    {
      resolve: `gatsby-plugin-manifest`,
      options: {
        name: `GatsbyJS`,
        short_name: `GatsbyJS`,
        start_url: `/`,
        background_color: `#6b37bf`,
        theme_color: `#6b37bf`,
        // Enables "Add to Homescreen" prompt and disables browser UI (including back button)
        // see https://developers.google.com/web/fundamentals/web-app-manifest/#display
        display: `standalone`,
        icon: `src/images/icon.png`, // This path is relative to the root of the site.
      },
    },
  ]
}
```

Questo è tutto ciò di cui hai bisogno per iniziare ad aggiungere un web manifest a un sito Gatsby. L'esempio fornito riflette una configurazione di base: controlla il [plugin reference](/packages/gatsby-plugin-manifest/?=gatsby-plugin-manifest#automatic-mode) per ulteriori opzioni.

## Aggiungi il supporto offline

Un altro requisito affinché un sito web possa qualificarsi come PWA è l'uso di un [service worker](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API). Un service worker viene eseguito in background, decide di serve contenuto dalla rete oppure dalla cache in base alla connessione, consentendo un'esperienza offline senza interruzioni e gestita.

Il [Gatsby plugin offline](/packages/gatsby-plugin-offline/) fa in modo che un sito Gatsby funzioni offline e sia più resistente a cattive condizioni di rete creando un service worker per il tuo sito.

### ✋ Usare il `gatsby-plugin-offline`

1. Installa il plugin:

```shell
npm install gatsby-plugin-offline
```

2. Aggiungi il plugin all'array `plugins` nel tuo file `gatsby-config.js`.

```javascript:title=gatsby-config.js
{
  plugins: [
    {
      resolve: `gatsby-plugin-manifest`,
      options: {
        name: `GatsbyJS`,
        short_name: `GatsbyJS`,
        start_url: `/`,
        background_color: `#6b37bf`,
        theme_color: `#6b37bf`,
        // Enables "Add to Homescreen" prompt and disables browser UI (including back button)
        // see https://developers.google.com/web/fundamentals/web-app-manifest/#display
        display: `standalone`,
        icon: `src/images/icon.png`, // This path is relative to the root of the site.
      },
    },
    // highlight-next-line
    `gatsby-plugin-offline`,
  ]
}
```

Questo è tutto ciò di cui hai bisogno per iniziare con i service worker con Gatsby.

> 💡 Il plug-in offline dovrebbe essere elencato _dopo_ il plug-in manifest in modo che il plug-in offline possa memorizzare nella cache il `manifest.webmanifest` creato.

## Aggiungere i metadata di una pagina

L'aggiunta di metadati alle pagine (come un titolo o una descrizione) è fondamentale per aiutare i motori di ricerca come Google a comprendere i tuoi contenuti e decidere quando mostrarli nei risultati di ricerca.

[React Helmet](https://github.com/nfl/react-helmet) è un pacchetto che fornisce un'interfaccia componente React per gestire il tuo [document head](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/head).

Il [react helmet plugin](/packages/gatsby-plugin-react-helmet/) di Gatsby fornisce supporto drop-in per i dati di rendering del server aggiunti con React Helmet. Usando il plugin, gli attributi che aggiungi a React Helmet verranno aggiunti alle pagine HTML statiche che Gatsby costruisce.

### ✋ Usare `React Helmet` e `gatsby-plugin-react-helmet`

1. Installa entrambi i pacchetti:

```shell
npm install gatsby-plugin-react-helmet react-helmet
```

2. Assicurati di avere una `description` e un `author` configurati all'interno del tuo oggetto `siteMetadata`. Inoltre, aggiungi il plugin `gatsby-plugin-react-helmet` all'array `plugins` nel tuo file `gatsby-config.js`.

```javascript:title=gatsby-config.js
module.exports = {
  siteMetadata: {
    title: `Pandas Eating Lots`,
    // highlight-start
    description: `A simple description about pandas eating lots...`,
    author: `gatsbyjs`,
    // highlight-end
  },
  plugins: [
    {
      resolve: `gatsby-plugin-manifest`,
      options: {
        name: `GatsbyJS`,
        short_name: `GatsbyJS`,
        start_url: `/`,
        background_color: `#6b37bf`,
        theme_color: `#6b37bf`,
        // Enables "Add to Homescreen" prompt and disables browser UI (including back button)
        // see https://developers.google.com/web/fundamentals/web-app-manifest/#display
        display: `standalone`,
        icon: `src/images/icon.png`, // This path is relative to the root of the site.
      },
    },
    `gatsby-plugin-offline`,
    // highlight-next-line
    `gatsby-plugin-react-helmet`,
  ],
}
```

3. Nella directory `src/components`, crea un file chiamato `seo.js` e aggiungi questo:

```jsx:title=src/components/seo.js
import React from "react"
import PropTypes from "prop-types"
import { Helmet } from "react-helmet"
import { useStaticQuery, graphql } from "gatsby"

function SEO({ description, lang, meta, title }) {
  const { site } = useStaticQuery(
    graphql`
      query {
        site {
          siteMetadata {
            title
            description
            author
          }
        }
      }
    `
  )

  const metaDescription = description || site.siteMetadata.description

  return (
    <Helmet
      htmlAttributes={{
        lang,
      }}
      title={title}
      titleTemplate={`%s | ${site.siteMetadata.title}`}
      meta={[
        {
          name: `description`,
          content: metaDescription,
        },
        {
          property: `og:title`,
          content: title,
        },
        {
          property: `og:description`,
          content: metaDescription,
        },
        {
          property: `og:type`,
          content: `website`,
        },
        {
          name: `twitter:card`,
          content: `summary`,
        },
        {
          name: `twitter:creator`,
          content: site.siteMetadata.author,
        },
        {
          name: `twitter:title`,
          content: title,
        },
        {
          name: `twitter:description`,
          content: metaDescription,
        },
      ].concat(meta)}
    />
  )
}

SEO.defaultProps = {
  lang: `en`,
  meta: [],
  description: ``,
}

SEO.propTypes = {
  description: PropTypes.string,
  lang: PropTypes.string,
  meta: PropTypes.arrayOf(PropTypes.object),
  title: PropTypes.string.isRequired,
}

export default SEO
```

Il codice sopra imposta le impostazioni predefinite per i tag di metadati più comuni e ti fornisce un componente `<SEO>` per funzionare all'interno del resto del tuo progetto. Affascinante, vero?

4. Ora puoi usare il componente `<SEO>` nei tuoi template e pagine e passargli dei props. Ad esempio, aggiungilo al tuo template `blog-post.js` in questo modo:

```jsx:title=src/templates/blog-post.js
import React from "react"
import { graphql } from "gatsby"
import Layout from "../components/layout"
// highlight-next-line
import SEO from "../components/seo"

export default function BlogPost({ data }) {
  const post = data.markdownRemark
  return (
    <Layout>
      // highlight-start
      <SEO title={post.frontmatter.title} description={post.excerpt} />
      // highlight-end
      <div>
        <h1>{post.frontmatter.title}</h1>
        <div dangerouslySetInnerHTML={{ __html: post.html }} />
      </div>
    </Layout>
  )
}

export const query = graphql`
  query($slug: String!) {
    markdownRemark(fields: { slug: { eq: $slug } }) {
      html
      frontmatter {
        title
      }
      // highlight-next-line
      excerpt
    }
  }
`
```

L'esempio sopra è basato sul [Gatsby Starter Blog](/starters/gatsbyjs/gatsby-starter-blog/). Passando i props al componente `<SEO>`, puoi modificare dinamicamente i metadati di un post. In questo caso, verranno utilizzati il `title` e l'`excerpt` del post del blog (se esiste nel file markdown del post del blog) al posto delle proprietà predefinite `siteMetadata` nel tuo file `gatsby-config.js`.

Ora, se esegui di nuovo l'audit di Lighthouse come indicato sopra, dovresti avvicinarti a (se non addirittura a un punteggio perfetto di) 100!

> 💡 Per ulteriori letture ed esempi, dai un'ochiata ad [Aggiunta di un SEO component](/docs/add-seo-component/) e ai [React Helmet docs](https://github.com/nfl/react-helmet#example)!

## Continua a migliorarlo

In questa sezione, ti abbiamo mostrato alcuni strumenti specifici di Gatsby per migliorare le prestazioni del tuo sito e prepararti alla pubblicazione.

Lighthouse è un ottimo strumento per il miglioramento del sito e l'apprendimento: continua a guardare il feedback dettagliato che fornisce e continua a migliorare il tuo sito!

## Prossimi passi

### Documentazione ufficiale

- [Documentazione Ufficiale](https://www.gatsbyjs.com/docs/): Guarda la nostra documentazione ufficiale per _[Avvio Rapido](https://www.gatsbyjs.com/docs/quick-start/)_, _[Guide Dettagliate](https://www.gatsbyjs.com/docs/preparing-your-environment/)_, _[Riferimenti per le API](https://www.gatsbyjs.com/docs/gatsby-link/)_, e molto altro ancora.

### Plugin ufficiali

- [Plugin Ufficiali](https://github.com/gatsbyjs/gatsby/tree/master/packages): L'elenco completo di tutti i plugin ufficiali gestiti da Gatsby.

### Starter ufficiali

1. [Starter di default di Gatsby](https://github.com/gatsbyjs/gatsby-starter-default): Dai il via al tuo progetto con questo boilerplate predefinito. Questo starter barebone viene fornito con i principali file di configurazione di Gatsby di cui potresti aver bisogno. _[esempio funzionante](https://gatsbyjs.github.io/gatsby-starter-default/)_
2. [Starter per un blog di Gatsby](https://github.com/gatsbyjs/gatsby-starter-blog): Starter Gatsby per la creazione di un blog fantastico e velocissimo. _[esempio funzionante](https://gatsbyjs.github.io/gatsby-starter-blog/)_
3. [Starter Hello-World di Gatsby](https://github.com/gatsbyjs/gatsby-starter-hello-world): Starter di Gatsby con gli elementi essenziali necessari per un sito Gatsby. _[esempio funzionante](https://gatsby-starter-hello-world-demo.netlify.app/)_

## È tutto gente

Beh, non proprio; solo per questo tutorial. Ci sono [esercitazioni aggiuntive](/tutorial/additional-tutorials/) da controllare per casi d'uso più guidati.

Questo è solo l'inizio. Continua!

- Hai costruito qualcosa di interessante? Condividilo su Twitter, tagga [#buildwithgatsby](https://twitter.com/search?q=%23buildwithgatsby), e [@menzionaci](https://twitter.com/gatsbyjs)!
- Hai scritto un bel post sul blog su ciò che hai imparato? Condividi anche quello!
- Contribuisci! Dai un'occhiata alle [issue aperte](https://github.com/gatsbyjs/gatsby/issues?q=is%3Aissue+is%3Aopen+label%3A%22help+wanted%22) nella repo di Gatsby e [diventa un contributore](/contributing/how-to-contribute/).

Dai un'occhiata alla documentazione ["come contribuire"](/contributing/how-to-contribute/) per ulteriori idee.

Non vediamo l'ora di vedere cosa farai 😄.
