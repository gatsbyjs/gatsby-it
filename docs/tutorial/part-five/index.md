---
title: Source Plugin
typora-copy-images-to: ./
disableTableOfContents: true
---

> Questo tutorial fa parte della serie sul data layer di Gatsby. Assicurati di aver letto il tutorial precedente [part 4](/tutorial/part-four/) prima di iniziare.

## Cosa imparerai in questo tutorial?

In questo tutorial imparerai come inserire i dati nel tuo sito Gatsby usando GraphQL e i source plugin. Prima di conoscere questi plugin, comunque, sarebbe bene familiarizzare con l'utilizzo di GraphiQL, uno strumento che aiuta a strutturare correttamente le query.

## Introduzione a GraphiQL

GraphiQL è l'ambiente di sviluppo integrato (IDE) di GraphQL. È un potente (oltre che fantastico) strumento che userai spesso durante la creazione di siti con Gatsby.

Quando il server di sviluppo è in esecuzione puoi accedervi all'indirizzo <http://localhost:8000/___graphql>.

<video controls="controls" autoplay="true" loop="true">
  <source type="video/mp4" src="/graphiql-explore.mp4"></source>
  <p>Your browser does not support the video element.</p>
</video>

Cerca il "type" `Site` preconfigurato e osserva quali campi sono disponibili -- incluso l'oggetto `siteMetadata` che hai interrogato in precedenza. Prova ad aprire GraphiQL e familiarizzare con i tuoi dati! Premi <kbd>Ctrl + Spazio</kbd> (oppure usa <kbd>Shift + Spazio</kbd> come scorciatoia di tastiera) per visualizzare la finestra di completamento automatico e <kbd>Ctrl + Enter</kbd> per eseguire una query GraphQL. Utilizzerai GraphiQL molto di più durante il resto del tutorial.

## Utilizzare GraphiQL Explorer

GraphiQL explorer consente di creare in modo interattivo query complete, solo cliccando sui campi e sugli input a disposizione, senza il processo ripetitivo di scrivere le query a mano.

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-build-a-graphql-query-using-gatsby-s-graphiql-explorer"
  lessonTitle="Build a GraphQL Query using Gatsby’s GraphiQL Explorer"
/>

## Source plugins

Nei siti Gatsby i dati possono provenire da fonti diverse: API, database, CMS, file locali, etc.

I source plugin recuperano i dati dalla loro origine. Per esempio, il source plugin del filesystem può recuperare i dati dal file system. Il plugin per WordPress sa come ottenere i dati utilizzando la API di WordPress.

Aggiungi [`gatsby-source-filesystem`](/packages/gatsby-source-filesystem/) ed esplora il suo funzionamento.

Innanzitutto, installa il plugin nella root del progetto:

```shell
npm install --save gatsby-source-filesystem
```

Quindi aggiungilo al tuo `gatsby-config.js`:

```javascript:title=gatsby-config.js
module.exports = {
  siteMetadata: {
    title: `Pandas Eating Lots`
  },
  plugins: [
    // highlight-start
    {
      resolve: `gatsby-source-filesystem`,
      options: {
        name: `src`,
        path: `${__dirname}/src/`
      }
    },
    // highlight-end
    `gatsby-plugin-emotion`,
    {
      resolve: `gatsby-plugin-typography`,
      options: {
        pathToConfigModule: `src/utils/typography`
      }
    }
  ]
};
```

Salva tutto e riavvia il server di sviluppo.
Riapri nuovamente GraphiQL.

Nel riquadro di esplorazione (Explorer), vedrai `allFile` e `file` disponibili per la scelta:

![graphiql-filesystem](graphiql-filesystem.png)

Clicca sul menu a discesa `allFile`. Posiziona il cursore dopo `allFile` nell'area di query, e quindi digita <kbd>Ctrl + Enter</kbd>. Verrà pre compilata una query per l'`id` di ogni file. Premi "Play" per eseguire la query:

![filesystem-query](filesystem-query.png)

Nle riquadro Explorer, il campo `id` è stato selezionato automaticamente. Effettua la selezione per più campi selezionando la casella corrispondente. Premi "Play" per eseguire di nuovo la query, con i nuovi campi:

![filesystem-explorer-options](filesystem-explorer-options.png)

Puoi inoltre aggiungere altri campi usando la scorciatoia di autocompletamento (<kbd>Ctrl + Spazio</kbd>). Questa mostrerà tutti i campi interrogabili sui nodi `File`.

![filesystem-autocomplete](filesystem-autocomplete.png)

Prova ad aggiungere un numero di campi alla tua query, premendo ogni volta <kbd>Ctrl + Enter</kbd> per eseguire di nuovo il comando. Vedrai i risultati di query aggiornati:

![allfile-query](allfile-query.png)

Il risultato è un array di "nodi" appartenenti a `File` (nodo è un nome di fantasia per indicare un oggetto in un grafico). Ogni oggetto di un nodo `File` ha i campi che hai interrogato.

## Creare una pagina con una query GraphQL

La costruzione di nuove pagine con Gatsby inizia spesso con GraphiQL. Puoi creare delle query sperimentali di dati in GraphiQL e successivamente copiarle in un componente di pagina React per iniziare a costruire l'interfaccia utente (UI).

Vediamo un esempio.

Crea un nuovo file `src/pages/my-files.js` con la query GraphQL `allFile` che hai appena costruito:

```jsx:title=src/pages/my-files.js
import React from "react";
import { graphql } from "gatsby";
import Layout from "../components/layout";

export default ({ data }) => {
  console.log(data); // highlight-line
  return (
    <Layout>
      <div>Hello world</div>
    </Layout>
  );
};

export const query = graphql`
  query {
    allFile {
      edges {
        node {
          relativePath
          prettySize
          extension
          birthTime(fromNow: true)
        }
      }
    }
  }
`;
```

La linea `console.log(data)` è sopra evidenziata. Quando crei un nuovo componente è spesso utile visualizzare i dati provenienti da una query GraphQL in modo da poter esplorare questi dati nella console del browser mentre costruisci la UI.

Se visiti la nuova pagina su `/my-files/` e apri la console del browser vedrai qualcosa come:

![data-in-console](data-in-console.png)

La struttura dei dati corrisponde alla struttura della query GraphQL.

Aggiungi qualche linea di codice al componente per eseguire print su File e stampare i dati.

```jsx:title=src/pages/my-files.js
import React from "react";
import { graphql } from "gatsby";
import Layout from "../components/layout";

export default ({ data }) => {
  console.log(data);
  return (
    <Layout>
      {/* highlight-start */}
      <div>
        <h1>My Site's Files</h1>
        <table>
          <thead>
            <tr>
              <th>relativePath</th>
              <th>prettySize</th>
              <th>extension</th>
              <th>birthTime</th>
            </tr>
          </thead>
          <tbody>
            {data.allFile.edges.map(({ node }, index) => (
              <tr key={index}>
                <td>{node.relativePath}</td>
                <td>{node.prettySize}</td>
                <td>{node.extension}</td>
                <td>{node.birthTime}</td>
              </tr>
            ))}
          </tbody>
        </table>
      </div>
      {/* highlight-end */}
    </Layout>
  );
};

export const query = graphql`
  query {
    allFile {
      edges {
        node {
          relativePath
          prettySize
          extension
          birthTime(fromNow: true)
        }
      }
    }
  }
`;
```

Ora visita [http://localhost:8000/my-files](http://localhost:8000/my-files)… 😲

![my-files-page](my-files-page.png)

## Cosa c'è dopo?

Hai quindi imparato come i source plugin portano i dati _nel_ sistema dati di Gatsby. Nel prossimo tutorial imparerai come i transformer plugin _trasformano_ il contenuto non elaborato portato dai source plugin. La combinazione di source plugin e transformer plugin è in grado di gestire tutto il processo di acquisizione e trasformazione dei dati di cui potresti aver bisogno durante la creazione di un sito con Gatsby. Scopri i transformer plugin nella [sesta parte del tutorial](/tutorial/part-six/).
