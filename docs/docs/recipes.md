---
title: Ricette
tableOfContentsDepth: 2
---

<!-- Basic template for a Gatsby recipe:

## Task to accomplish.
1-2 sentences about it. The more concise and focused, the better!

### Prerequisites
- System/version requirements
- Everything necessary to set up the task
- Including setting up accounts at other sites, like Netlify
- See [docs templates](/docs/docs-templates/) for formatting tips

### Directions
Step-by-step directions. Each step should be repeatable and to-the-point. Anything not critical to the task should be omitted.

#### Live example (optional)
A live example may not be possible depending on the nature of the recipe, in which case it is fine to omit.

### Additional resources
- Tutorials
- Docs pages
- Plugin READMEs
- etc.

See [docs templates](/docs/docs-templates/) in the contributing docs for more help.
-->

Stai cercando una via di mezzo tra dei [tutorial completi](/tutorial/) e lo spulciare tra la [documentazione](/docs/)? Ecco un libro di ricette guida su come creare cose, alla Gatsby.

## 1. Pagine e Layouts

### Struttura del progetto

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

Alcuni degni di nota con la loro definizione:

- `gatsby-config.js` — configura le opzioni di un sito Gatsby, come i metadati per il titolo di progetto, la descrizione, i plugin, ecc.
- `gatsby-node.js` — usa l'API Node.js di Gatsby per personalizzare ed estendere la configurazione di base del processo di _build_
- `gatsby-browser.js` — personalizza ed estendi le configurazioni di base relative al browser, usando l'API del browser di Gatsby
- `gatsby-ssr.js` — usa l'API di server-side rendering di Gatsby per personalizzare ed estendere la configurazione di base relative al server-side rendering

#### Ulteriori risorse

- Per una panoramica di tutti i file e cartelle comuni, leggi la documentazione su [La struttura di un progetto Gatsby](/docs/gatsby-project-structure/)
- Per i comandi di base, dai un occhio alla documentazione sulla [Linea di comando di Gatsby](/docs/gatsby-cli)
- Dai un occhio alla [Guida Veloce di Gatsby](/docs/cheat-sheet/) per avere informazioni utili in un attimo

### Pagine create in modo automatico

Il core di Gatsby trasforma automaticamente in componenti React all'interno di `src/pages` in pagine con un URL.
Per esempio, i componenti in `src/pages/index.js` e `src/pages/about.js` genereranno in automatico la pagina principale del sito (`/`) e la pagina `/about`, in base al loro nome del file.

#### Prerequisiti

- Un [sito Gatsby](/docs/quick-start)
- La [CLI di Gatsby](/docs/gatsby-cli) installata

#### Istruzioni

1. Crea una cartella `src/pages` se il tuo sito non ne ha già una.
2. Aggiungi un _component_ file nella cartella delle pagine:

```jsx:title=src/pages/about.js
import React from "react";

const AboutPage = () => (
  <main>
    <h1>L'autore</h1>
    <p>Benvenuto sul mio sito Gatsby.</p>
  </main>
);

export default AboutPage;
```

3. Lancia `gatsby develop` per far partire il server di sviluppo.
4. Visita la tua nuova pagina nel browser: `http://localhost:8000/about`

#### Ulteriori risorse

- [Creare e modificare pagine](/docs/creating-and-modifying-pages/)

### Collegamenti tra le pagine

Il routing in Gatsby si basa sul componente `<Link />`.

#### Prerequisiti

- Un sito Gatsby con due pagine componente: `index.js` e`contact.js`
- Il componente `<Link />` di Gatsby
- La [CLI di Gatsby](/docs/gatsby-cli/) per lanciare `gatsby develop`

#### Istruzioni

1. Apri il componente della pagina principale (`src/pages/index.js`), importa il componente `<Link />`, aggiungi il componente `<Link />` sopra l'header e aggiungi la proprietà `to` con il valore `"/contact/"` per il percorso:

```jsx:title=src/pages/index.js
import React from "react";
import { Link } from "gatsby";

export default () => (
  <div style={{ color: `purple` }}>
    <Link to="/contact/">Contatti</Link>
    <p>Ciao Mondo.</p>
  </div>
);
```

2. Lancia `gatsby develop` e naviga la pagina principale. Dovresti vedere un link che ti porta alla pagina dei contatti quando selezionato!

> **Nota**: Il componente `<Link />` di Gatsby è un _wrapper_ attorno al [componente Link di `@reach/router`](https://reach.tech/router/api/Link). Per ulteriori informazioni sul componente `<Link />`di Gatsby, consulta [l'API di relativa per `<Link />`](/docs/gatsby-link/).

### Creare un componente di layout

È pratica comune inserire attorno a una pagina un componente React per il _layout_, il quale rende possibile la condivisione del _markup_, stile e funzionalità a cavallo di diverse pagine.

#### Prerequisiti

- Un sito Gatsby

#### Istruzioni

1. Crea un componente layout in `src/components`, dove in componenti figlio saranno passati come _prop_:

```jsx:title=src/components/layout.js
import React from "react";

export default ({ children }) => (
  <div style={{ margin: `0 auto`, maxWidth: 650, padding: `0 1rem` }}>
    {children}
  </div>
);
```

2. Importa e usa il componente layout nella pagina

```jsx:title=src/pages/index.js
import React from "react";
import Layout from "../components/layout";

export default () => (
  <Layout>
    <Link to="/contact/">Contatti</Link>
    <p>Ciao mondo.</p>
  </Layout>
);
```

#### Ulteriori risorse

- Crea un componente layout nel [tutorial parte terza](/tutorial/part-three/#your-first-layout-component)
- Dai uno stile ai [Componenti di layout](/docs/layout-components/)

### Creare pagine in modo programmatico con createPage

Puoi creare pagine in modo programmatico nel file `gatsby-node.js` usando i metodi che Gatsby mette a disposizione.

#### Prerequisiti

- Un [sito Gatsby](/docs/quick-start)
- Un file `gatsby-node.js`

#### Istruzioni

1. Dentro `gatsby-node.js`, aggiungi un export per `createPages`

```javascript:title=gatsby-node.js
// highlight-start
exports.createPages = ({ actions }) => {
  // ...
};
// highlight-end
```

2. Destruttura l'azione `createPage` dalle azioni disponibili, in modo da eseguirla da sola, e aggiungi o ricava i dati

```javascript:title=gatsby-node.js
exports.createPages = ({ actions }) => {
  // highlight-start
  const { createPage } = actions;
  // pull in or use whatever data
  const dogData = [
    {
      name: "Fido",
      breed: "Sheltie"
    },
    {
      name: "Sparky",
      breed: "Corgi"
    }
  ];
  // highlight-end
};
```

3. Itera attraverso i dati in `gatsby-node.js` e fornisci il _path_, il _template_ e il _context_ (dati che verranno passati nelle _prop_ di pageContext) a `createPage` per ogni iterazione

```javascript:title=gatsby-node.js
exports.createPages = ({ actions }) => {
  const { createPage } = actions;

  const dogData = [
    {
      name: "Fido",
      breed: "Sheltie"
    },
    {
      name: "Sparky",
      breed: "Corgi"
    }
  ];
  // highlight-start
  dogData.forEach(dog => {
    createPage({
      path: `/${dog.name}`,
      component: require.resolve(`./src/templates/dog-template.js`),
      context: { dog }
    });
  });
  // highlight-end
};
```

4. Crea un componente React che funga da template per la tua pagina che sarà usato da `createPage`

```jsx:title=src/templates/dog-template.js
import React from "react";

export default ({ pageContext: { dog } }) => (
  <section>
    {dog.name} - {dog.breed}
  </section>
);
```

5. Lancia `gatsby develop` e naviga il percorso di una delle pagine create (per esempio `http://localhost:8000/Fido`) per vedere i dati che hai passato, visualizzarsi nella pagina

#### Ulteriori risorse

- Sezione tutorial in [Creare pagine in modo programmatico dai dati](/tutorial/part-seven/)
- Guida di riferimento su come [usare Gatsby senza GraphQL](/docs/using-gatsby-without-graphql/)
- [Repository degli esempi](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-createPage) per questa ricetta

## 2. Dare uno stile con il CSS

Ci sono molti modi per aggiungere lo stile al tuo sito web; Gatsby supporta praticamente qualsiasi metodo, attraverso i plugin ufficiali e della community.

### Usare file CSS globali senza un componente Layout

#### Prerequisiti

- Un [sito Gatsby](/docs/quick-start/) già esistente con un componente di pagina principale
- Il file `gatsby-browser.js`

#### Istruzioni

1. Crea un file CSS globale tipo `src/styles/global.css` e incolla il codice seguente nel file:

```css:title=src/styles/styles/global.css
html {
  background-color: lavenderblush;
}

p {
  color: maroon;
}
```

2. Importa il file CSS globale dentro il file `gatsby-browser.js` come di seguito:

```javascript:gatsby-browser.js
import "./src/styles/global.css";
```

> **Nota:** Per importare il file CSS globale dentro `gatsby-config.js` puoi anche fare uso di `require('./src/styles/global.css')`

3. Lancia `gatsby-develop` per vedere che lo stile globale viene applicato su tutto il sito.

> **Nota:** Questo approccio non è il migliore se stai usando CSS-inJS per dare lo stile al tuo sito, in quel caso andrebbe usata una pagina layout con tutti i componenti condivisi. Il tutto verrà spiegato nella prossima ricetta.

#### Ulteriori risorse

- Scopri di più su [come aggiungere lo stile senza un componente layout](/docs/global-css/#adding-global-styles-without-a-layout-component)

### Usare uno stile globale in un componente layout

#### Prerequisiti

- Un [sito Gatsby](/docs/quick-start/) già esistente con un componente di pagina principale

#### Istruzioni

Puoi inserire uno stile globale ad un [componente di layout condiviso](/tutorial/part-three/#your-first-layout-component). Questo componente è usato per le cose in comune nel sito, come _header_ o _footer_.

1. Se non ne hai già una, crea una nuova cartella nel tuo sito in `/src/components`

2. All'interno della cartella dei componenti, crea due file: `layout.css` e `layout.js`.

3. Aggiungi il codice seguente a `layout.css`:

```css:title=/src/components/layout.css
body {
  background: red;
}
```

4. Modifica `layout.js` per importare il file CSS ed esporre il markup del layout:

```jsx:title=/src/components/layout.js
import React from "react";
import "./layout.css";

export default ({ children }) => <div>{children}</div>;
```

5. Ora modifica l'homepage del tuo sito in `/src/pages/index.js` e usa il nuovo componente layout:

```jsx:title=/src/pages/index.js
import React from "react";
import Layout from "../components/layout";

export default () => <Layout>Ciao mondo!</Layout>;
```

#### Ulteriori risorse

- [Stile standard con i file di CSS globali](/docs/global-css/)
- [Approfondisci i componenti layout](/tutorial/part-three)

### Usare gli Styled Components

#### Prerequisiti

- Un [sito Gatsby](/docs/quick-start/) già esistente con un componente di pagina principale
- [gatsby-plugin-styled-components, styled-components, e babel-plugin-styled-components](/packages/gatsby-plugin-styled-components/) installati in `package.json`

#### Istruzioni

1. All'interno del tuo file `gatsby-config.js` aggiungi `gatsby-plugin-styled-components`

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [`gatsby-plugin-styled-components`]
};
```

2. Apri il componente della pagina principale (`src/pages/index.js`) e importa il pacchetto `styled-components`

3. Crea degli _style components_ per ogni tipo di elemento, aggiungendo lo stile per ogni blocco.

4. Aggiungili alla pagine inserendo gli _styled components_ nel JSX

```jsx:title=src/pages/index.js
import React from "react";
import styled from "styled-components"; //highlight-line

const Container = styled.div`
  margin: 3rem auto;
  max-width: 600px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
`;

const Avatar = styled.img`
  flex: 0 0 96px;
  width: 96px;
  height: 96px;
  margin: 0;
`;

const Username = styled.h2`
  margin: 0 0 12px 0;
  padding: 0;
`;

const User = props => (
  <>
    <Avatar src={props.avatar} alt={props.username} />
    <Username>{props.username}</Username>
  </>
);

export default () => (
  <Container>
    <h1>Gli Styled Components</h1>
    <p>Styled Components spacca</p>
    <User
      username="Jane Doe"
      avatar="https://s3.amazonaws.com/uifaces/faces/twitter/adellecharles/128.jpg"
    />
    <User
      username="Bob Smith"
      avatar="https://s3.amazonaws.com/uifaces/faces/twitter/vladarbatov/128.jpg"
    />
  </Container>
);
```

4. Lancia `gatsby develop` per vedere le modifiche

#### Ulteriori risorse

- [Approfondisci l'uso degli Styled Components](/docs/styled-components/)
- [Lezioni su Egghead](https://egghead.io/lessons/gatsby-style-gatsby-sites-with-styled-components)

### Usare CSS Modules

#### Prerequisiti

- Un [sito Gatsby](/docs/quick-start/) già esistente con un componente di pagina principale

#### Istruzioni

1. Crea un CSS _module_ nel file `src/pages/index.module.css` e incollaci dentro il codice di seguito:

```css:title=src/components/index.module.css
.feature {
  margin: 2rem auto;
  max-width: 500px;
}
```

2. Importa il CSS _module_ come un oggetto `style` JSX nel file `index.js` modificando la pagine come mostrati di seguito:

```jsx:title=src/pages/index.js
import React from "react";

// highlight-start
import style from "./index.module.css";

export default () => (
  <section className={style.feature}>
    <h1>Usare i CSS Modules</h1>
  </section>
);
// highlight-end
```

3. Lancia `gatsby develop` per vedere le modifiche.

**Nota:**
Nota che l'estensione del file è `.module.css` invece di `.css`, questo dice a Gatsby che il file è un CSS _module_

#### Ulteriori risorse

- Scopri di più in [Usare i CSS Module](/tutorial/part-two/#css-modules)
- [Esempi pratici di Usare i CSS Module](https://github.com/gatsbyjs/gatsby/blob/master/examples/using-css-modules)

### Usare Sass/SCSS

Sass è un estensione di CSS che aggiunge molte funzionalità avanzate come le regole innestate, le variabili, i _mixins_, e molto altro.

Sass ha 2 sintassi. La sintassi più usata è "SCSS", ed un _superset_ di CSS. Questo significa che tutta la sitassi del CSS, è sintassi valida SCSS. I file SCSS usano l'estensione .scss

Sass processa i file .scss e .sass in file .css, in questo modo puoi scrivere i tuoi stili usando molte funzionalità avanzante.

#### Prerequisiti

- Un [sito Gatsby](/docs/quick-start/)

#### Istruzioni

1. Installa il plugin di Gatsby [gatsby-plugin-sass](https://www.gatsbyjs.org/packages/gatsby-plugin-sass/) e `node-sass`.

`npm install --save node-sass gatsby-plugin-sass`

2. Includi il plugin nel tuo file `gatsby-config.js`;

```javascript:title=gatsby-config.js
plugins: [`gatsby-plugin-sass`],
```

3. Scrivi i tuoi file di stile come `.sass` o `.scss` and importali. Se non sai come importare gli stili, dai un occhio a [Dare uno stile con il CSS](/docs/recipes/#2-styling-with-css)

```css:title=styles.scss
$font-stack: Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}
```

```css:title=styles.sass
$font-stack:    Helvetica, sans-serif
$primary-color: #333

body
  font: 100% $font-stack
  color: $primary-color
```

```javascript
import "./styles.scss";
import "./styles.sass";
```

_Note: Puoi usare Sass/SCSS files anche come moduli, come indicato nella ricetta precedente sui CSS module, con la differenza che invece di usare .css l'estensione deve essere .scss o .sass_

#### Ulteriori risorse

- [Differenze tra .sass e .scss](https://responsivedesign.is/articles/difference-between-sass-and-scss/)
- [Guida ufficiale di Sass dal sito ufficiale](https://sass-lang.com/guide)
- [Un tutorial più completo sull'installazione di Sass con alcune spiegazioni in più e ulteriori risorse](https://www.gatsbyjs.org/docs/sass/)

### Aggiungere un Font locale

#### Prerequisiti

- Un [sito Gatsby](/docs/quick-start/)
- Un file font: `.woff2`, `.ttf`, ecc.

#### Istruzioni

1. Copia un file font dentro il tuo progetto Gatsby, per esempio `src/fonts/fontname.woff2`

```
src/fonts/fontname.woff2
```

2. Importa la risorsa dentro un file CSS per inserirla dentro il sito Gatsby:

```css:title=src/css/typography.css
@font-face {
  font-family: "Font Name";
  src: url("../fonts/fontname.woff2");
}
```

**Nota:** Accertati che il nome del font abbia una referenza nel CSS relativo, per es;

```css:title=src/components/layout.css
body {
  font-family: "Font Name", sans-serif;
}
```

Usando l'elemento HTML `body`, il tuo font verrà applicato alla maggior parte del test della pagina. Altri CSS posso indicare altri elementi, come `button` o `textarea`.

Se i font non vengono aggiornavi, segui gli step di sopra, assicurati di sostituire la _font-family_ esistente nel relativo CSS.

#### Ulteriori risorse

- Scopri di più [su come importare risorse nei file](/docs/importing-assets-into-files/)

### Usare Emotion

[Emotion](https://emotion.sh) è una potente libreria CSS-inJS che supporta sia il CSS inline che gli _styled component_. È possibile utilizzare ogni caratteristica indipendentemente o insieme nello stesso file.

#### Prerequisiti

- Un [sito Gatsby](/docs/quick-start)

#### Istruzioni

1. Installa il [plugin Emotion di Gatsby](/packages/gatsby-plugin-emotion/) e i pacchetti Emotion.

```shell
npm install --save gatsby-plugin-emotion @emotion/core @emotion/styled
```

2. Aggiungi il plugin `gatsby-plugin-emotion` al tuo file `gatsby-config.js`:

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [`gatsby-plugin-emotion`]
};
```

3. Se non ne hai già una, crea una pagina nel tuo sito Gatsby in `src/pages/emotion-sample.js`.

Importa `css` dal pacchetto core di Emotion. Dopodiché puoi usare la _prop_ `css` per aggiungere un [oggetto style di Emotion](https://emotion.sh/docs/object-styles) ad ogni elemento dentro un componente:

```jsx:title=src/pages/emotion-sample.js
import React from "react";
import { css } from "@emotion/core";

export default () => (
  <div>
    <p
      css={{
        background: "pink",
        color: "blue"
      }}
    >
      Questa pagina usa Emotion.
    </p>
  </div>
);
```

4. Per usare gli [styled component](https://emotion.sh/docs/styled) di Emotion, importa il pacchetto e definiscili usando la funzione `styled`.

```jsx:title=src/pages/emotion-sample.js
import React from "react";
import styled from "@emotion/styled";

const Content = styled.div`
  text-align: center;
  margin-top: 10px;
  p {
    font-weight: bold;
  }
`;

export default () => (
  <Content>
    <p>Questa pagina usa Emotion.</p>
  </Content>
);
```

#### Ulteriori risorse

- [Usare Emotion con Gatsby](/docs/emotion/)
- [Il sito di Emotion](https://emotion.sh)
- [Per iniziare con Emotion e Gatsby](https://egghead.io/lessons/gatsby-getting-started-with-emotion-and-gatsby)

### Usare i Google Font

Hostare i tuo [font Google](https://fonts.google.com/) localmente in un progetto, significa che essi non avranno bisogno di essere recuperati dalla rete quando il tuo sito si carica, migliorando lo _speed index_ del tuo sito fino a ~300 millisecondi su desktop and 1+ millisecondi in 3G. È anche raccomandato, per le performance, di limitare l'uso dei _font custom_ al minimo.

#### Prerequisiti

- Un [sito Gatsby](/docs/quick-start)
- La [CLI di Gatsby](/docs/gatsby-cli/) installata
- Scegliere un pacchetto font da [il progetto typefaces](https://github.com/KyleAMathews/typefaces)

#### Istruzioni

1. Lancia `npm install --save typeface-il-font-da-te-scelto`, cambiando `il-font-da-te-scelto` con il nome del font che vuoi installare da [il progetto typefaces](https://github.com/KyleAMathews/typefaces).

Un esempio per caricare uno dei font più popolari, il 'Source Sans Pro', sarebbe: `npm install --save typeface-source-sans-pro`.

2. Aggiungi `import "typeface-your-chosen-font"` al tuo _layout template_, _page component_, o `gatsby-browser.js`.

```jsx:title=src/components/layout.js
import "typeface-your-chosen-font";
```

3. Una volta importato, puoi utilizzare il nome del font all'interno del tuo stile CSS, CSS module, o con CSS-in-JS.

```css:title=src/components/layout.css
body {
  font-family: "Il Nome del Font Da Te Scelto";
}
```

_NOTA: Riguardo all'esempio di cui sopra, la dichiarazione nel CSS sarebbe `font-family: 'Source Sans Pro';`_

#### Ulteriori risorse

- [Typography.js](/docs/typography-js/) - Un altro modo per usare i Google font in un sito Gatsby
- [La Documentazione del progetto Typefaces](https://github.com/KyleAMathews/typefaces/blob/master/README.md)
- [Esempi sul blog di Kyle Mathews](https://www.bricolage.io/typefaces-easiest-way-to-self-host-fonts/)

## 3. Working with starters

Gli [Starters](/docs/starters/) sono dei moduli predefiniti di siti Gatsby mantenuti ufficialmente, o dalla comunità.

### Usare uno starter

#### Prerequisiti

- La [CLI di Gatsby](/docs/gatsby-cli/) installata

#### Istruzioni

1. Cerca lo _starter_ che vorresti usare. (_La [Libreari degli Starter](/starters/?v=2) è un buon posto dove cercare!_)

2. Genera un nuovo sito basato sullo starter. Nel terminale lancia:

```shell
gatsby new {il-nome-del-tuo-progetto} {link-allo-starter}
```

> _Non lanciare questo comando così com'è -- ricordati di cambiare {il-nome-del-tuo-progetto} e {link-allo-starter}!_

3. Lancia il tuo nuovo sito:

```shell
cd {il-nome-del-tuo-progetto}
gatsby develop
```

#### Ulteriori risorse

- Segui [guide più dettagliate](/docs/starters/) su come usare gli _starter_ di Gatsby.
- Impara ad usare la [CLI di Gatsby](/docs/gatsby-cli) per usare gli _starter_ nel [tutorial parte uno](/tutorial/part-one/#using-gatsby-starters)
- Esplora la [libreria degli Starter](/starters/?v=2)
- Controlla lo [starter ufficiale di base di Gatsby](https://github.com/gatsbyjs/gatsby-starter-default)

## 4. Lavorare con i temi

Un tema di Gatsby astrae tutte le configurazioni di base (funzionalità condivise, acquisizione dati, progettazione) del tuo sito e le inserisce in un pacchetto installabile. Questo significa che le configurazioni e le funzionalità non sono scritte direttamente dentro il tuo progetto, vengono invece versionate, gestite in modo centrale, e installate come una dipendenza. Puoi aggiornare in modo trasparente un tema, comporre temi insieme, ed eventualmente scambiare un tema compatibile con un altro.

- Approfondisci su [Cos'è un tema di Gatsby?](/docs/themes/what-are-gatsby-themes)

### Creare un nuovo sito usando uno theme starter

Creare un sito partendo da uno _starter_ che configura un tema segue la stessa procedura della creazione di un sito che parte da uno starter che **non** configura un tema. In questo esempio puoi usare lo [starter per creare un nuovo sito che usa il tema ufficiale per blog di Gatsby](https://github.com/gatsbyjs/gatsby-starter-blog-theme).

#### Prerequisiti

- La [CLI di Gatsby](/docs/gatsby-cli/) installata

#### Istruzioni

1. Genera un nuovo sito basato sullo starter del tema per blog:

```shell
gatsby new {il-nome-del-tuo-progetto} https://github.com/gatsbyjs/gatsby-starter-blog-theme
```

2. Lancia il tuo nuovo sito:

```shell
cd {il-nome-del-tuo-progetto}
gatsby develop
```

#### Ulteriori risorse

- Impara come usare un tema di Gatsby esistente in questa [breve guida concettuale](/docs/themes/using-a-gatsby-theme) o con il [tutorial passo dopo passo](/tutorial/using-a-theme) più dettagliato.

### Creare un nuovo tema

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-use-the-gatsby-theme-workspace-starter-to-begin-building-a-new-theme"
  lessonTitle="Use the Gatsby Theme Workspace Starter to Begin Building a New Theme"
/>

#### Prerequisiti

- La [CLI di Gatsby](/docs/gatsby-cli/) installata

* [Yarn](https://yarnpkg.com/lang/en/docs/install/#mac-stable) installato

#### Istruzioni

1. Genera un nuovo _workspace_ per il tema usando lo [starter workspace per i temi di Gatsby](https://github.com/gatsbyjs/gatsby-starter-theme-workspace):

```shell
gatsby new {il-nome-del-tuo-progetto} https://github.com/gatsbyjs/gatsby-starter-theme-workspace
```

2. Lancia il sito d'esempio nel _workspace_:

```shell
yarn workspace example develop
```

#### Ulteriori risorse

- Segui una [guida più dettagliata](/docs/themes/building-themes/) su come usare lo _starter workspace_ per i temi di Gatsby.
- Impara come creare il tuo tema con il [video corso Gatsby Theme Authoring su Egghead](https://egghead.io/courses/gatsby-theme-authoring), o con il [tutorial scritto come complemento al video corso](/tutorial/building-a-theme).

## 5. Reperire i dati

Per reperire i dati in Gatsby vengono utilizzati i plugin; i Source plugin recuperano i dati dalla loro sorgente (es. il plugin `gatsby-source-filesystem` recupera i dati dal file system, il plugin `gatsby-source-wordpress` recupera i dati dalle API di Wordpress, ecc). Puoi anche reperire i dati da solo.

### Aggiungere dati in GraphQL

Il [data layer GraphQL](/docs/querying-with-graphql/) di Gatsby usa nodi per modellare porzioni di dati. I plugin source di Gatsby aggiungono nodi _source_ che puoi interrogare, ma puoi anche creare nodi _source_ da solo. Per aggiungere dati _custom_ al data layer di GraphQL, potrai sfruttare i metodi che Gatsby mette a disposizione.

Questa ricetta ti mostra come aggiungere dati _custom_ usando `createNode()`.

#### Istruzioni

1. In `gatsby-node.js` usa `sourceNodes()` e `actions.createNode()` per creare ed esportare nodi per essere in grado di interrogare i dati.

```javascript:title=gatsby-node.js
exports.sourceNodes = ({ actions, createNodeId, createContentDigest }) => {
  const pokemons = [
    { name: "Pikachu", type: "electric" },
    { name: "Squirtle", type: "water" }
  ];

  pokemons.forEach(pokemon => {
    const node = {
      name: pokemon.name,
      type: pokemon.type,
      id: createNodeId(`Pokemon-${pokemon.name}`),
      internal: {
        type: "Pokemon",
        contentDigest: createContentDigest(pokemon)
      }
    };
    actions.createNode(node);
  });
};
```

2. Lanciate `gatsby develop`.

   > \_Nota: Ogni volta che viene modificato `gatsby-node.js` è necessario rilanciare `gatsby develop` per vedere le modifiche.

3. Interrogare i dati (in GraphiQL o nei componenti).

```graphql
query MyPokemonQuery {
  allPokemon {
    nodes {
      name
      type
      id
    }
  }
}
```

#### Ulteriori risorse

- Guida dettagliata su come usare il plugin `gatsby-source-filesystem` nel [tutorial parte cinque](/tutorial/part-five/#source-plugins)
- Cerca i _source plugin_ disponibili nella [libreria di Gatsby](/plugins/?=source)
- Comprendi i _source plugin_ costruendone uno nel [tutorial del source plugin Pixabay](/docs/pixabay-source-plugin-tutorial/)
- La [documentazione](/docs/actions/#createNode) per la funzione createNode

### Recuperare i dati Markdown con GraphQL per gli articoli e le pagine di un blog.

Puoi recuperare i dati Markdown e usare l'[API `createPages`](/docs/actions/#createPage) di Gatsby per creare le pagine in modo dinamico.

Questa ricetta mostra come creare le pagine partendo da file Markdown presenti sul tuo filesystem locale, usando il layer GraphQL di Gatsby.

#### Prerequisiti

- Un [sito Gatsby](/docs/quick-start) with a `gatsby-config.js` file
- La [CLI di Gatsby](/docs/gatsby-cli/) installata
- Il [plugin gatsby-source-filesystem](/packages/gatsby-source-filesystem) installato
- Il [plugin gatsby-transformer-remark](/packages/gatsby-transformer-remark) installato
- Un file `gatsby-node.js`

#### Istruzioni

1. In `gatsby-config.js`, configura `gatsby-transformer-remark` insieme a `gatsby-source-filesystem` per recuperare i file Markdown dalla cartella _source_. Questo in aggiunta a qualsiasi elemento di `gatsby-source-filesystem`, come le immagini:

```js:title=gatsby-config.js
module.exports = {
  plugins: [
    `gatsby-transformer-remark`,
    {
      resolve: `gatsby-source-filesystem`,
      options: {
        name: `content`,
        path: `${__dirname}/src/content`,
      },
    },
  ]
```

2. Aggiungi un articolo Markdown in `src/content`, compreso il _frontmatter_ per il titolo, la data, il percorso, con un contenuto iniziale per il copro dell'articolo:

```markdown:title=src/content/my-first-post.md
---
title: Il mio primo articolo
date: 2019-07-10
path: /il-mio-primo-articolo
---

Questo è il mio primo articolo di Gatsby scritto in Markdown!
```

3 Fai partire il server di sviluppo con `gatsby develop`, apri il GraphiQL explorer su `http://localhost:8000/___graphql`, e scrivi una _query_ per recuperare tutti i dati markdown:

```graphql
{
  allMarkdownRemark {
    edges {
      node {
        frontmatter {
          path
        }
      }
    }
  }
}
```

<iframe
  title="Recuperare tutto il markdown"
  src="https://q4xpb.sse.codesandbox.io/___graphql?explorerIsOpen=false&query=%7B%0A%20%20allMarkdownRemark%20%7B%0A%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20frontmatter%20%7B%0A%20%20%20%20%20%20%20%20%20%20path%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D"
  width="600"
  height="300"
/>

4. Aggiungi il codice JavaScript per generare, durante la fase di _build_, le pagine partendo dagli articoli in Markdown, copiando la query GraphQL dentro `gatsby-node.js` e fai un loop sui risultati:

```js:title=gatsby-node.js
const path = require(`path`);

exports.createPages = async ({ actions, graphql }) => {
  const { createPage } = actions;

  const result = await graphql(`
    {
      allMarkdownRemark {
        edges {
          node {
            frontmatter {
              path
            }
          }
        }
      }
    }
  `);
  if (result.errors) {
    console.error(result.errors);
  }

  result.data.allMarkdownRemark.edges.forEach(({ node }) => {
    createPage({
      path: node.frontmatter.path,
      component: path.resolve(`src/templates/post.js`)
    });
  });
};
```

5. Aggiungi un template di articolo dentro `src/templates`, includendo una query GraphQL per generare, durante la fase _build_, le pagine in modo dinamico dal contenuto Markdown:

```jsx:title=src/templates/post.js
import React from "react";
import { graphql } from "gatsby";

export default function Template({ data }) {
  const { markdownRemark } = data; // data.markdownRemark holds your post data
  const { frontmatter, html } = markdownRemark;
  return (
    <div className="blog-post">
      <h1>{frontmatter.title}</h1>
      <h2>{frontmatter.date}</h2>
      <div
        className="blog-post-content"
        dangerouslySetInnerHTML={{ __html: html }}
      />
    </div>
  );
}

export const pageQuery = graphql`
  query($path: String!) {
    markdownRemark(frontmatter: { path: { eq: $path } }) {
      html
      frontmatter {
        date(formatString: "MMMM DD, YYYY")
        path
        title
      }
    }
  }
`;
```

6. Lancia `gatsby develop` per far ripartire il server di sviluppo. Visualizza il tuo articolo nel browser: `http://localhost:8000/il-mio-primo-articolo`

#### Ulteriori risorse

- [Tutorial: Creare pagine in modo programmatico dai dati](/tutorial/part-seven/)
- [Creare e modificare pagine](/docs/creating-and-modifying-pages/)
- [Aggiungere pagine Markdown](/docs/adding-markdown-pages/)
- [Guida per creare pagine da dei dati in modo programmatico](/docs/programmatically-create-pages-from-data/)
- [Repository si esempio](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-sourcing-markdown) per questa ricetta

### Sourcing from WordPress

#### Prerequisites

- An existing [Gatsby site](/docs/quick-start/) with a `gatsby-config.js` and `gatsby-node.js` file
- A WordPress instance, either self-hosted or on Wordpress.com

#### Directions

1. Install the `gatsby-source-wordpress` plugin by running the following command:

```shell
npm install gatsby-source-wordpress --save
```

2. Configure the plugin by modifying the `gatsby-config.js` file such that it includes the following:

```javascript:title=gatsby-config.js
module.exports = {
  ...
  plugins: [
    {
      resolve: `gatsby-source-wordpress`,
      options: {
        // baseUrl will need to be updated with your WordPress source
        baseUrl: `wpexample.com`,
        protocol: `https`,
        // is it hosted on wordpress.com, or self-hosted?
        hostingWPCOM: false,
        // does your site use the Advanced Custom Fields Plugin?
        useACF: false
      }
    },
  ]
}
```

> **Note:** Refer to the [`gatsby-source-wordpress` plugin docs](/packages/gatsby-source-wordpress/?=wordpre#how-to-use) to know more about configuring your plugins.

3. Create a template component such as `src/templates/post.js` with the following code in it:

```javascript:title=post.js
import React, { Component } from "react"
import { graphql } from "gatsby"
import PropTypes from "prop-types"

class Post extends Component {
  render() {
    const post = this.props.data.wordpressPost

    return (
      <>
        <h1>{post.title}</h1>
        <div dangerouslySetInnerHTML={{ __html: post.content }} />
      </>
    )
  }
}

Post.propTypes = {
  data: PropTypes.object.isRequired,
  edges: PropTypes.array,
}

export default Post

export const pageQuery = graphql`
  query($id: String!) {
    wordpressPost(id: { eq: $id }) {
      title
      content
    }
  }
`
```

4. Create dynamic pages for your WordPress posts by pasting the following sample code in `gatsby-node.js`:

```javascript:title=gatsby-node.js
const path = require(`path`)
const slash = require(`slash`)

exports.createPages = async ({ graphql, actions }) => {
  const { createPage } = actions

  // query content for WordPress posts
  const result = await graphql(`
    query {
      allWordpressPost {
        edges {
          node {
            id
            slug
          }
        }
      }
    }
  `)

  const postTemplate = path.resolve(`./src/templates/post.js`)
  result.data.allWordpressPost.edges.forEach(edge => {
    createPage({
      // `path` will be the url for the page
      path: edge.node.slug,
      // specify the component template of your choice
      component: slash(postTemplate),
      // In the ^template's GraphQL query, 'id' will be available
      // as a GraphQL variable to query for this posts's data.
      context: {
        id: edge.node.id,
      },
    })
  })
}
```

5. Run `gatsby-develop` to see the newly generated pages and navigate through them.

6. Open the `GraphiQL IDE` at `localhost:8000/__graphql` and open the Docs or Explorer to observe the queryable fields for `allWordpressPosts`.

The dynamic pages created above in `gatsby-node.js` have unique paths for navigating to particular posts, using a template component for the posts and a sample GraphQL query to source WordPress post content.

#### Additional resources

- [Getting Started with WordPress and Gatsby](/blog/2019-04-26-how-to-build-a-blog-with-wordpress-and-gatsby-part-1/)
- More on [Sourcing from WordPress](/docs/sourcing-from-wordpress/)
- [Live example on Sourcing from WordPress](https://github.com/gatsbyjs/gatsby/tree/master/examples/using-wordpress)

### Sourcing data from Contentful

#### Prerequisites

- A [Gatsby site](/docs/quick-start/)
- A [Contentful account](https://www.contentful.com/)
- The [Contentful CLI](https://www.npmjs.com/package/contentful-cli) installed

#### Directions

1. Log in to Contentful with the CLI and follow the steps. It will help you create an account if you don't have one.

```shell
contentful login
```

2. Create a new space if you don't already have one. Make sure to save the space ID given to you at the end of the command. If you already have a Contentful space and space ID, you can skip steps 2 and 3.

Note: for new accounts, you can overwrite the default onboarding space. Check to see the [spaces included with your account](https://app.contentful.com/account/profile/space_memberships).

```shell
contentful space create --name 'Gatsby example'
```

3. Seed the new space with example blog content using the new space ID returned from the previous command, in place of `<space ID>`.

```shell
contentful space seed -s '<space ID>' -t blog
```

For example, with a space ID in place: `contentful space seed -s '22fzx88spbp7' -t blog`

4. Create a new access token for your space. Remember this token, as you will need it in step 6.

```shell
contentful space accesstoken create -s '<space ID>' --name 'Example token'
```

5. Install the `gatsby-source-contentful` plugin in your Gatsby site:

```shell
npm install --save gatsby-source-contentful
```

6. Edit the file `gatsby-config.js` and add the `gatsby-source-contentful` to the `plugins` array to enable the plugin. You should strongly consider using [environment variables](/docs/environment-variables/) to store your space ID and token for security purposes.

```javascript:title=gatsby-config.js
plugins: [
   // add to array along with any other installed plugins
   // highlight-start
   {


    resolve: `gatsby-source-contentful`,
    options: {
      spaceId: `<space ID>`, // or process.env.CONTENTFUL_SPACE_ID
      accessToken: `<access token>`, // or process.env.CONTENTFUL_TOKEN
    },
  },
  // highlight-end
],
```

7. Run `gatsby develop` and make sure the site compiled successfully.

8. Query data with the [GraphiQL editor](/docs/introducing-graphiql/) at <https://localhost:8000/___graphql>. The Contentful plugin adds several new node types to your site, including every content type in your Contentful website. Your example space with a "Blog Post" content type produces a `allContentfulBlogPost` node type in GraphQL.

![the graphql interface, with a sample query outlined below](./images/recipe-sourcing-contentful-graphql.png)

To query for Blog Post titles from Contentful, use the following GraphQL query:

```graphql
{
  allContentfulBlogPost {
    edges {
      node {
        title
      }
    }
  }
}
```

Contentful nodes also include several metadata fields like `createdAt` or `node_locale`.

9. To show a list of links to the blog posts, create a new file in `/src/pages/blog.js`. This page will display all posts, sorted by updated date.

```jsx:title=src/pages/blog.js
import React from "react"
import { graphql, Link } from "gatsby"

const BlogPage = ({ data }) => (
  <div>
    <h1>Blog</h1>
    <ul>
      {data.allContentfulBlogPost.edges.map(({ node, index }) => (
        <li key={index}>
          <Link to={`/blog/${node.slug}`}>{node.title}</Link>
        </li>
      ))}
    </ul>
  </div>
)

export default BlogPage

export const query = graphql`
  {
    allContentfulBlogPost(sort: { fields: [updatedAt] }) {
      edges {
        node {
          title
          slug
        }
      }
    }
  }
`
```

To continue building out your Contentful site including post detail pages, check out the rest of the [Gatsby docs](/docs/sourcing-from-contentful/) and additional resources below.

#### Additional resources

- [Building a Site with React and Contentful](/blog/2018-1-25-building-a-site-with-react-and-contentful/)
- [More on Sourcing from Contentful](/docs/sourcing-from-contentful/)
- [Contentful source plugin](/packages/gatsby-source-contentful/)
- [Long-text field types returned as objects](/packages/gatsby-source-contentful/#a-note-about-longtext-fields)
- [Example repository for this recipe](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-sourcing-contentful)

### Recuperare i dati da una sorgente esterna e creare pagine senza GraphQL

Non sei costretto ad un usare il data layer GraphQL per inserire dati all'interno delle pagine, [anche se ci sono ragioni per cui dovresti considerare GraphQL](/docs/why-gatsby-uses-graphql/). Puoi usare la API `createPages` per reperire dati non strutturati direttamente all'interno dei siti Gatsby anziché attraverso GraphQL e i _source_ plugin.

In questa ricetta, creerai pagine dinamiche dai dati recuperati da un [endpoint REST di PokéAPI](https://www.pokeapi.co/). [L'esempio completo](https://github.com/jlengstorf/gatsby-with-unstructured-data/) è disponibile su Github.

#### Prerequisiti

- Un sito Gatsby con un file `gatsby-node.js`
- La [CLI di Gatsby](/docs/gatsby-cli/) installata
- Il pacchetto [axios](https://www.npmjs.com/package/axios) installato tramite npm

#### Istruzioni

1. In `gatsby-node.js`, aggiungi il codice JavaScript per recuperare i dati da PokéAPI e creare in modo programmatico la pagina iniziale:

```js:title=gatsby-node.js
const axios = require("axios");

const get = endpoint => axios.get(`https://pokeapi.co/api/v2${endpoint}`);

const getPokemonData = names =>
  Promise.all(
    names.map(async name => {
      const { data: pokemon } = await get(`/pokemon/${name}`);
      return { ...pokemon };
    })
  );
exports.createPages = async ({ actions: { createPage } }) => {
  const allPokemon = await getPokemonData(["pikachu", "charizard", "squirtle"]);

  // Create a page that lists Pokémon.
  createPage({
    path: `/`,
    component: require.resolve("./src/templates/all-pokemon.js"),
    context: { allPokemon }
  });
};
```

2. Crea un template per visualizzare i Pokémon nella homepage:

```js:title=src/templates/all-pokemon.js
import React from "react";

export default ({ pageContext: { allPokemon } }) => (
  <div>
    <h1>Ecco, i Pokémon!</h1>
    <ul>
      {allPokemon.map(pokemon => (
        <li key={pokemon.id}>
          <img src={pokemon.sprites.front_default} alt={pokemon.name} />
          <p>{pokemon.name}</p>
        </li>
      ))}
    </ul>
  </div>
);
```

3. Lancia `gatsby develop` per recuperare i dati, creare le pagine e far partire il server di sviluppo.
4. Visualizza la tua homepage nel browser: `http://localhost:8000`

#### Ulteriori risorse

- [Repository completo dei dati Pokemon](https://github.com/jlengstorf/gatsby-with-unstructured-data/)
- Maggiori informazioni sull'uso dei dati non strutturati in [Usare Gatsby senza GraphQL](/docs/using-gatsby-without-graphql/)
- Quando e come [interrogare i dati con GraphQL](/docs/querying-with-graphql/) per siti più complessi con Gatsby

### Recuperare contenuti da Drupal

#### Prerequisiti

- Un [sito Gatsby](/docs/quick-start)
- Un sito [Drupal](http://drupal.org)
- Il [modulo JSON:API](https://www.drupal.org/project/jsonapi) installato e attivo sul sito Drupal

#### Istruzioni

1. Installa il plugin `gatsby-source-drupal`.

```
npm install --save gatsby-source-drupal
```

2. Modifica il tuo file `gatsby-config.js` per attivare e configurare il plugin.

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [
    {
      resolve: `gatsby-source-drupal`,
      options: {
        baseUrl: `https://your-website/`,
        apiBase: `api` // optional, defaults to `jsonapi`
      }
    }
  ]
};
```

3. Fai partire il server di sviluppo con `gatsby develop`, e apri GraphiQL explorer all'indirizzo `http://localhost:8000/___graphql`. Sotto il tab Explorer, dovresti vedere dei nuovi tipi di nodi, come `allBlockBlock` per i _blocks_ di Drupal, e uno per ogni tipo di contenuto del tuo sito Drupal. Per esempio, se hai un contenuto di tipo "Page", sarà disponibile come `allNodePage`. Per eseguire una query per tutti i nodi "Page" per _title_ e _body_, usa una query come questa:

```graphql
{
  allNodePage {
    edges {
      node {
        title
        body {
          value
        }
      }
    }
  }
}
```

4. Per usare i dati di Drupal, crea una nuova pagina nel tuo sito Gastby in `src/pages/drupal.js`. Questa pagina elencherà tutti i nodi "Page" di Drupal.

_**Nota:** lo schema esatto di GraphQL dipenderà su come è strutturata la tua istanza di Drupal._

```jsx:title=src/pages/drupal.js
import React from "react"
import { graphql } from "gatsby"

const DrupalPage = ({ data }) => (
  <div>
    <h1>Pagine di Drupal</h1>
    <ul>
    {data.allNodePage.edges.map(({ node, index }) => (
      <li key={index}>
        <h2>{node.title}</h2>
        <div>
          {node.body.value}
        </div>
      </li>
    ))}
   </ul>
  </div>
)

export default DrupalPage

export const query = graphql`
  {
  allNodePage {
    edges {
      node {
        title
        body {
          value
        }
      }
    }
  }
}
```

5. Mentre il server di sviluppo è in esecuzione, puoi vedere le nuove pagine visitando `http://localhost:8000/drupal`.

#### Ulteriori risorse

- [Utilizzare Drupal in modo disaccoppiato con Gatsby](/blog/2018-08-13-using-decoupled-drupal-with-gatsby/)
- [Ulteriori informazioni su recuperare dati da Drupal](/docs/sourcing-from-drupal)
- [Tutorial: Creare pagine in modo programmatico dai dati](/tutorial/part-seven/)

## 6. Recuperare i dati

### Recuperare i dati con una Page Query

Puoi usare il tag `graphql` per interrogare i dati nella pagina del tuo sito Gatsby. In questo modo hai accesso a ogni cosa inclusa nel _data layer_ di Gatsby, come i metadati del sito, i source plugin, le immagini e molto altro.

#### Istruzioni

1. Importa `graphql` da `gatsby`.

2. Esporta un costante con il nome `query` e imposta il suo valore con un template `graphql` con la query tra due virgolette _backticks_.

3. Aggiungi `data` come prop al componente.

4. La variabile `data` contiene i dati della query e può essere usato in JSX per generare HTML.

```jsx:title=src/pages/index.js
import React from "react";
// highlight-next-line
import { graphql } from "gatsby";

import Layout from "../components/layout";

// highlight-start
export const query = graphql`
  query HomePageQuery {
    site {
      siteMetadata {
        title
      }
    }
  }
`;
// highlight-end

// highlight-next-line
const IndexPage = ({ data }) => (
  <Layout>
    // highlight-next-line
    <h1>{data.site.siteMetadata.title}</h1>
  </Layout>
);

export default IndexPage;
```

#### Ulteriori risorse

- [GraphQL e Gatsby](/docs/graphql/): capire la forma prevista dei tuoi dati
- [Ulteriori informazioni su come recuperare i dati nelle pagine usando GraphQL](/docs/page-query/)
- [Stringhe template su MDN](https://developer.mozilla.org/it/docs/Web/JavaScript/Reference/template_strings) come quelle usate in GraphQL

### Recuperare i dati con il componente StaticQuery

`StaticQuery` è un Componente per recuperare i dati dal _data layer_ di Gatsby nei [componenti non-pagine](/docs/static-query/), come header, la navigazione o qualsiasi altro componente figlio.

#### Istruzioni

1. Il Componente `StaticQuery` utilizza due _render prop_: `query` e `render`.

```jsx:title=src/components/NonPageComponent.js
import React from "react";
import { StaticQuery, graphql } from "gatsby";

const NonPageComponent = () => (
  <StaticQuery
    query={graphql` // highlight-line
      query NonPageQuery {
        site {
          siteMetadata {
            title
          }
        }
      }
    `}
    render={(
      data // highlight-line
    ) => (
      <h1>
        Recuperare il titolo da un NonPageComponent usando StaticQuery:
        {data.site.siteMetadata.title}
      </h1>
    )}
  />
);

export default NonPageComponent;
```

2. Adesso puoi usare questo componente come [ogni altro componente](/docs/building-with-components#non-page-components) importandolo dentro una pagina più grande composta da componenti JSX e markup HTML.

### Recuperare i dati usando l'hook useStaticQuery

Dalla versione Gatsby v2.1.0, puoi usare l'hook `useStaticQuery` per recuperare i tramite una funzione JavaScript invece di un componente. La sintassi evita di usare un componente `<StaticQuery>` come _wrapper_ ad ogni cosa, e risulta più semplice per alcune persone.

L'hook `useStaticQuery` prende una query GraphQL e ritorna i dati richiesti. Può essere salvata in una variabile e usarla più tardi nei tuoi template JSX.

#### Prerequisiti

- Hai bisogno di React e ReactDOM 16.8.0 o maggiore (la cosa è automatica se Gatsby è tenuto aggiornato)
- Letture raccomandate: le [Regole degli Hook di React](https://reactjs.org/docs/hooks-rules.html)

#### Istruzioni

1. importa `useStaticQuery` e `graphql` da `gatsby` per essere in grado di usare l'hook per recuperare i dati.

2. All'inizio di una componente funzione, assegna una variabile al valore ritornato da `useStaticQuery` con la tua query `graphql` passata come un argomento.

3. Nel codice JSX restituito dal tuo componente, puoi referenziare la variabile per usare i dati restituiti.

```jsx:title=src/components/NonPageComponent.js
import React from "react";
import { useStaticQuery, graphql } from "gatsby"; //highlight-line

const NonPageComponent = () => {
  // highlight-start
  const data = useStaticQuery(graphql`
    query NonPageQuery {
      site {
        siteMetadata {
          title
        }
      }
    }
  `);
  // highlight-end
  return (
    <h1>
      Recuperare il titolo da un NonPageComponent:{" "}
      {data.site.siteMetadata.title} //highlight-line
    </h1>
  );
};

export default NonPageComponent;
```

#### Ulteriori risorse

- [Ulteriori informazioni su Query Statiche per recuperare i dati nei componenti](/docs/static-query/)
- [La differenza tra una query statica e una query di pagina](/docs/static-query/#how-staticquery-differs-from-page-query)
- [Ulteriori informazioni sull'hook useStaticQuery](/docs/use-static-query/)
- [Visualizza i tuoi dati con GraphiQL](/docs/introducing-graphiql/)

### Limitare tramite GraphQL

Quando recuperi i dati con GraphQL, puoi limitare il numero dei risultati restituiti usandone un numero specifico. Questo è di aiuto se hai bisogno solo di pochi pezzi di dati o se hai bisogno di [paginare i risultati](/docs/adding-pagination/).

Per limitare i dati, devi avere un sito Gatsby con alcuni nodi nel data layer di GraphQL. Tutti i siti hanno nodi, creati automaticamente, del tipo `allSitePage` e `sitePage`: altri possono essere aggiunti installando i source plugin come `gatsby-source-filesystem` in `gatsby-config.js`.

#### Prerequisiti

- Un [sito Gatsby](/docs/quick-start/)

#### Istruzioni

3. Lancia `gatsby develop` per far partire il server di sviluppo.
4. Apri un tab del tuo browser su: `http://localhost:8000/___graphql`.
5. Aggiungi una query nell'editor usando i seguenti campi su `allSitePage` per iniziare:

```graphql
{
  allSitePage {
    edges {
      node {
        id
        path
      }
    }
  }
}
```

4. Aggiungi l'argomento `limit` al campo `allSitePage` e imposta un valore intero `3`

```graphql
{
  allSitePage(limit: 3) { // highlight-line
    edges {
      node {
        id
        path
      }
    }
  }
}
```

5. Premi il bottone play nella pagina di GraphiQL e i dati nel campo `edges` verranno limitati per il numero specificato.

#### Ulteriori risorse

- Learn about [nodes in Gatsby's GraphQL data API](/docs/node-interface/)
- [Gatsby GraphQL reference for limiting](/docs/graphql-reference/#limit)
- Live example:

<iframe
  title="Limiting returned data"
  src="https://711808k40x.sse.codesandbox.io/___graphql?query=%7B%0A%20%20allSitePage(limit%3A%203)%20%7B%0A%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20id%0A%20%20%20%20%20%20%20%20path%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A&explorerIsOpen=false"
  width="600"
  height="300"
/>

### Ordinare tramite GraphQL

L'ordine dei tuoi risultati può essere indicato usando l'argomento `sort` di GraphQL. Puoi indicare quali campi ordinare e la direzione dell'ordinamento.

Per questa ricetta hai bisogno di un sito Gatsby con una collezione di nodi da ordinare nel data layer di GraphQL. Tutti i siti hanno nodi creati in automatico come `allSitePage`: altri possono essere aggiunti installando i source plugin.

#### Prerequisiti

- Un [sito Gatsby](/docs/quick-start)
- Campi interrogabili con il prefisso `all`, es.`allSitePage`

#### Istruzioni

1. Lancia `gatsby develop` per far partire il server di sviluppo.
2. Apri l'explorer GraphiQL in un tab del tuo browser su: `http://localhost:8000/___graphql`
3. Aggiungi una query nell'editor usando i seguenti campi con `allSitePage` per iniziare:

```graphql
{
  allSitePage {
    edges {
      node {
        id
        path
      }
    }
  }
}
```

4. Aggiungi un argomento `sort` al campo `allSitePage` e passagli un oggetto con gli attributi `fields` e `order`. Il valore per `fields` può essere un campo o un array di campi da ordinare (questo esempio usa il campo `path`), `order` può essere `ASC` o `DESC` per l'ordinamento ascendente o discendente

```graphql
{
  allSitePage(sort: {fields: path, order: ASC}) { // highlight-line
    edges {
      node {
        id
        path
      }
    }
  }
}

```

5. Premi il bottone play nella pagina di GraphiQL e i dati restituiti saranno ordinati in modo ascendente per il campo `path`.

#### Ulteriori risorse

- [Riferimenti per il sorting di Gatsby GraphQL](/docs/graphql-reference/#sort)
- Approfondisci la [API dei nodi del data layer di Gatsby](/docs/node-interface/)
- Esempio:

<iframe
  title="Sorting data"
  src="https://711808k40x.sse.codesandbox.io/___graphql?query=%7B%0A%20%20allSitePage(sort%3A%20%7Bfields%3A%20path%2C%20order%3A%20ASC%7D)%20%7B%0A%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20id%0A%20%20%20%20%20%20%20%20path%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A&explorerIsOpen=false"
  width="600"
  height="300"
/>

### Filtrare tramite GraphQL

I risultati di una query possono essere filtrati su specifici campi usando operatori del tipo `eq` (uguale), `ne` (non uguale), `in` e `regex`.

Per questa ricetta hai bisogno di un sito Gatsby con una collezione di nodi da filtrare nel data layer di GraphQL. Tutti i siti hanno nodi creati in automatico come `allSitePage`: altri possono essere aggiunti installando i source plugin e i transformer plugin come `gatsby-source-filesystem` e `gatsby-transformer-remark` dentro `gatsby-config.js` per creare `allMarkdownRemark`.

#### Prerequisiti

- Un [sito Gatsby](/docs/quick-start)
- Campi interrogabili con il prefisso `all`, es.`allSitePage` o `allMarkdownRemark`

#### Istruzioni

1. Lancia `gatsby develop` per far partire il server di sviluppo.
2. Apri l'explorer GraphiQL in un tab del tuo browser su: `http://localhost:8000/___graphql`
3. Aggiungi una query nell'editor usando un campo con prefisso 'all', come `allMarkdownRemark` (indica che restituisce una lista di nodi)

```graphql
{
  allMarkdownRemark {
    edges {
      node {
        frontmatter {
          title
          categories
        }
      }
    }
  }
}
```

4. Aggiungi un argomento `filter` al campo `allMarkdownRemark` e e passagli un oggetto con i campi che vorresti filtrare. In questo esempio, il contenuto Markdown è filtrato dall'attributo `categories` dei metadati frontmatter. Il valore successivo è l'operatore: in questo caso `eq`, o uguale, con il valore 'creature magiche'.

```graphql
{
  allMarkdownRemark(filter: {frontmatter: {categories: {eq: "creature magiche"}}}) { // highlight-line
    edges {
      node {
        frontmatter {
          title
          categories
        }
      }
    }
  }
}
```

5. Premi il bottone play nella pagina di GraphiQL. I dati che corrispondono ai parametri del filtro dovrebbero essere restituiti, in questo caso solo i file Markdown con la categoria 'creature magiche' nei tag.

#### Ulteriori risorse

- [Referenza sui filtri di Gatsby GraphQL](/docs/graphql-reference/#filter)
- [Una lista completa degli operatori possibili](/docs/graphql-reference/#complete-list-of-possible-operators)
- Approfondisci la [API dei nodi del data layer di Gatsby](/docs/node-interface/)
- Esempio:

<iframe
  title="Filtering data"
  src="https://711808k40x.sse.codesandbox.io/___graphql?query=%7B%0A%20%20allMarkdownRemark(filter%3A%20%7Bfrontmatter%3A%20%7Bcategories%3A%20%7Beq%3A%20%22magical%20creatures%22%7D%7D%7D)%20%7B%0A%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20frontmatter%20%7B%0A%20%20%20%20%20%20%20%20%20%20title%0A%20%20%20%20%20%20%20%20%20%20categories%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A&explorerIsOpen=false"
  width="600"
  height="300"
/>

### Alias nelle Query GraphQL

Puoi rinominare ogni campo in una _query_ GraphQL con un alias.

Se vuoi eseguire due _query_ nello stesso _datasource_, puoi usare un alias per evitare un conflitto di nomi tra due query con lo stesso nome.

#### Istruzioni

1. Lancia `gatsby develop` per far partire il server di sviluppo.
2. Apri l'explorer GraphiQL in un tab del tuo browser su: `http://localhost:8000/___graphql`
3. Aggiungi una _query_ nell'editor usando due campi con lo stesso nome, per esempio `allFile`

```graphql
{
  allFile {
    totalCount
  }
  allFile {
    pageInfo {
      currentPage
    }
  }
}
```

4. Aggiungi il nome che vorresti usare per ogni campo prima del nome del campo del tuo schema GraphQL, separati dai due punti. (Es. `[alias-name]: [original-name]`)

```graphql
{
  fileCount: allFile { // highlight-line
    totalCount
  }
  filePageInfo: allFile { // highlight-line
    pageInfo {
      currentPage
    }
  }
}
```

5. Premi il bottone play nella pagina di GraphiQL. e due oggetti con l'alias usato dovrebbero essere restituiti.

#### Ulteriori risorse

- [Referenza per gli alias di Gatsby GraphQL](/docs/graphql-reference/#aliasing)
- Esempio:

<iframe
  title="Usare gli alias"
  src="https://711808k40x.sse.codesandbox.io/___graphql?query=%7B%0A%20%20fileCount%3A%20allFile%20%7B%20%0A%20%20%20%20totalCount%0A%20%20%7D%0A%20%20filePageInfo%3A%20allFile%20%7B%0A%20%20%20%20pageInfo%20%7B%0A%20%20%20%20%20%20currentPage%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A&explorerIsOpen=false"
  width="600"
  height="300"
/>

### GraphQL Query Fragments

GraphQL fragments are shareable chunks of a query that can be reused.

You might want to use them to share multiple fields between queries or to colocate a component with the data it uses.

#### Istruzioni

1. Declare a `graphql` template string with a Fragment in it. The fragment should be made up of the keyword `fragment`, a name, the GraphQL type it is associated with (in this case of type `Site`, as demonstrated by `on Site`), and the fields that make up the fragment:

```jsx
export const query = graphql`
  // highlight-start
  fragment SiteInformation on Site {
    title
    description
  }
  // highlight-end
`;
```

2. Now, include the fragment in a query for a field of the type specified by the fragment. This includes those fields without having to declare them all independently:

```diff
export const pageQuery = graphql`
  query SiteQuery {
    site {
-     title
-     description
+   ...SiteInformation
    }
  }
`
```

**Note**: Fragments don't need to be imported in Gatsby. Exporting a query with a Fragment makes that Fragment available in _all_ queries in your project.

Fragments can be nested inside other fragments, and multiple fragments can be used in the same query.

#### Ulteriori risorse

- [Simple example repo using fragments](https://github.com/gatsbyjs/gatsby/tree/master/examples/using-fragments)
- [Gatsby GraphQL reference for fragments](/docs/graphql-reference/#fragments)
- [Gatsby image fragments](/docs/gatsby-image/#image-query-fragments)
- [Example repo with co-located data](https://github.com/gatsbyjs/gatsby/tree/master/examples/gatsbygram)

## 7. Working with images

### Import an image into a component with webpack

Images can be imported right into a JavaScript module with webpack. This process automatically minifies and copies the image to your site's `public` folder, providing a dynamic image URL for you to pass to an HTML `<img>` element like a regular file path.

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-import-a-local-image-into-a-gatsby-component-with-webpack"
  lessonTitle="Import a Local Image into a Gatsby Component with webpack"
/>

#### Prerequisiti

- Un [sito Gatsby](/docs/quick-start) with a `.js` file exporting a React component
- an image (`.jpg`, `.png`, `.gif`, `.svg`, etc.) in the `src` folder

#### Istruzioni

1. Import your file from its path in the `src` folder:

```jsx:title=src/pages/index.js
import React from "react";
// Tell webpack this JS file uses this image
import FiestaImg from "../assets/fiesta.jpg"; // highlight-line
```

2. In `index.js`, add an `<img>` tag with the `src` as the name of the import you used from webpack (in this case `FiestaImg`), and add an `alt` attribute [describing the image](https://webaim.org/techniques/alttext/):

```jsx:title=src/pages/index.js
import React from "react";
import FiestaImg from "../assets/fiesta.jpg";

export default () => (
  // The import result is the URL of your image
  <img src={FiestaImg} alt="A dog smiling in a party hat" /> // highlight-line
);
```

3. Run `gatsby develop` to start the development server.
4. View your image in the browser: <http://localhost:8000/>

#### Ulteriori risorse

- [Example repo importing an image with webpack](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-webpack-image)
- [More on all image techniques in Gatsby](/docs/images-and-files/)

### Reference an image from the `static` folder

As an alternative to importing assets with webpack, the `static` folder allows access to content that gets automatically copied into the `public` folder when built.

This is an **escape route** for [specific use cases](/docs/static-folder/#when-to-use-the-static-folder), and other methods like [importing with webpack](#import-an-image-into-a-component-with-webpack) are recommended to leverage optimizations made by Gatsby.

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-use-a-local-image-from-the-static-folder-in-a-gatsby-component"
  lessonTitle="Use a local image from the static folder in a Gatsby component"
/>

#### Prerequisiti

- Un [sito Gatsby](/docs/quick-start) with a `.js` file exporting a React component
- An image (`.jpg`, `.png`, `.gif`, `.svg`, etc.) in the `static` folder

#### Istruzioni

1. Ensure that the image is in your `static` folder at the root of the project. Your project structure might look something like this:

```
├── gatsby-config.js
├── src
│   └── pages
│       └── index.js
├── static
│       └── fiesta.jpg
```

2. In `index.js`, add an `<img>` tag with the `src` as the relative path of the file from the `static` folder, and add an `alt` attribute [describing the image](https://webaim.org/techniques/alttext/):

```jsx:title=src/pages/index.js
import React from "react";

export default () => (
  <img src={`fiesta.jpg`} alt="A dog smiling in a party hat" /> // highlight-line
);
```

3. Run `gatsby develop` to start the development server.
4. View your image in the browser: <http://localhost:8000/>

#### Ulteriori risorse

- [Example repo referencing an image from the static folder](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-static-image)
- [Using the Static Folder](/docs/static-folder/)
- [More on all image techniques in Gatsby](/docs/images-and-files/)

### Optimizing and querying local images with gatsby-image

The `gatsby-image` plugin can relieve much of the pain associated with optimizing images in your site.

Gatsby will generate optimized resources which can be queried with GraphQL and passed into Gatsby's image component. This takes care of the heavy lifting including creating several image sizes and loading them at the right time.

#### Prerequisiti

- The `gatsby-image`, `gatsby-transformer-sharp`, and `gatsby-plugin-sharp` packages installed and added to the plugins array in `gatsby-config`
- [Images sourced](/packages/gatsby-image/#install) in your `gatsby-config` using a plugin like `gatsby-source-filesystem`

#### Istruzioni

1. First, import `Img` from `gatsby-image`, as well as `graphql` and `useStaticQuery` from `gatsby`

```jsx
import { useStaticQuery, graphql } from "gatsby"; // to query for image data
import Img from "gatsby-image"; // to take image data and render it
```

2. Write a query to get image data, and pass the data into the `<Img />` component:

Choose any of the following options or a combination of them.

a. a single image queried by its file [path](/docs/content-and-data/) (Example: `images/corgi.jpg`)

```jsx
const data = useStaticQuery(graphql`
  query {
    file(relativePath: { eq: "corgi.jpg" }) { // highlight-line
      childImageSharp {
        fluid {
          base64
          aspectRatio
          src
          srcSet
          sizes
        }
      }
    }
  }
`);

return (
  <Img fluid={data.file.childImageSharp.fluid} alt="A corgi smiling happily" />
);
```

b. using a [GraphQL fragment](/docs/using-graphql-fragments/), to query for the necessary fields more tersely

```jsx
const data = useStaticQuery(graphql`
  query {
    file(relativePath: { eq: "corgi.jpg" }) {
      childImageSharp {
        fluid {
          ...GatsbyImageSharpFluid // highlight-line
        }
      }
    }
  }
`);

return (
  <Img fluid={data.file.childImageSharp.fluid} alt="A corgi smiling happily" />
);
```

c. several images from a directory (Example: `images/dogs`) [filtered](/docs/graphql-reference/#filter) by the `extension` and `relativeDirectory` fields, and then mapped into `Img` components

```jsx
const data = useStaticQuery(graphql`
  query {
    allFile(
      // highlight-start
      filter: {
        extension: { regex: "/(jpg)|(png)|(jpeg)/" }
        relativeDirectory: { eq: "dogs" }
      }
      // highlight-end
    ) {
      edges {
        node {
          base
          childImageSharp {
            fluid {
              ...GatsbyImageSharpFluid
            }
          }
        }
      }
    }
  }
`);

return (
  <div>
    // highlight-start
    {data.allFile.edges.map(image => (
      <Img
        fluid={image.node.childImageSharp.fluid}
        alt={image.node.base.split(".")[0]} // only use section of the file extension with the filename
      />
    ))}
    // highlight-end
  </div>
);
```

**Note**: This method can make it difficult to match images with `alt` text for accessibility. This example uses images with `alt` text included in the filename, like `dog in a party hat.jpg`.

d. an image of a fixed size using the `fixed` field instead of `fluid`

```jsx
const data = useStaticQuery(graphql`
  query {
    file(relativePath: { eq: "corgi.jpg" }) {
      childImageSharp {
        fixed(width: 250, height: 250) { // highlight-line
          ...GatsbyImageSharpFixed
        }
      }
    }
  }
`);
return (
  <Img fixed={data.file.childImageSharp.fixed} alt="A corgi smiling happily" />
);
```

e. an image of a fixed size with a `maxWidth`

```jsx
const data = useStaticQuery(graphql`
  query {
    file(relativePath: { eq: "corgi.jpg" }) {
      childImageSharp {
        fixed(maxWidth: 250) { // highlight-line
          ...GatsbyImageSharpFixed
        }
      }
    }
  }
`);
return (
  <Img fixed={data.file.childImageSharp.fixed} alt="A corgi smiling happily" /> // highlight-line
);
```

f. an image filling a fluid container with a max width (in pixels) and a higher quality (the default value is 50 i.e. 50%)

```jsx
const data = useStaticQuery(graphql`
  query {
    file(relativePath: { eq: "corgi.jpg" }) {
      childImageSharp {
        fluid(maxWidth: 800, quality: 75) { // highlight-line
          ...GatsbyImageSharpFluid
        }
      }
    }
  }
`);

return (
  <Img fluid={data.file.childImageSharp.fluid} alt="A corgi smiling happily" />
);
```

3. (Optional) Add inline styles to the `<Img />` like you would to other components

```jsx
<Img
  fluid={data.file.childImageSharp.fluid}
  alt="A corgi smiling happily"
  style={{ border: "2px solid rebeccapurple", borderRadius: 5, height: 250 }} // highlight-line
/>
```

4. (Optional) Force an image into a desired aspect ratio by overriding the `aspectRatio` field returned by the GraphQL query before it is passed into the `<Img />` component

```jsx
<Img
  fluid={{
    ...data.file.childImageSharp.fluid,
    aspectRatio: 1.6 // 1280 / 800 = 1.6
  }}
  alt="A corgi smiling happily"
/>
```

5. Run `gatsby develop`, to generate images from files in the filesystem (if not done already) and cache them

#### Ulteriori risorse

- [Example repository illustrating these examples](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipes-gatsby-image)
- [Gatsby Image API](/docs/gatsby-image/)
- [Using Gatsby Image](/docs/using-gatsby-image)
- [More on working with images in Gatsby](/docs/working-with-images/)
- [Free egghead.io videos explaining these steps](https://egghead.io/playlists/using-gatsby-image-with-gatsby-ea85129e)

### Optimizing and querying images in post frontmatter with gatsby-image

For use cases like a featured image in a blog post, you can _still_ use `gatsby-image`. The `Img` component needs processed image data, which can come from a local (or remote) file, including from a URL in the frontmatter of a `.md` or `.mdx` file.

To inline images in markdown (using the `![]()` syntax), consider using a plugin like [`gatsby-remark-images`](/packages/gatsby-remark-images/)

#### Prerequisiti

- The `gatsby-image`, `gatsby-transformer-sharp`, and `gatsby-plugin-sharp` packages installed and added to the plugins array in `gatsby-config`
- [Images sourced](/packages/gatsby-image/#install) in your `gatsby-config` using a plugin like `gatsby-source-filesystem`
- Markdown files sourced in your `gatsby-config` with image URLs in frontmatter
- [Pages created](/docs/creating-and-modifying-pages/) from Markdown using [`createPages`](https://www.gatsbyjs.org/docs/node-apis/#createPages)

#### Istruzioni

1. Verify that the Markdown file has an image URL with a valid path to an image file in your project

```mdx:title=post.mdx
---
title: My First Post
featuredImage: ./corgi.png // highlight-line
---

Post content...
```

2. Verify that a unique identifier (a slug in this example) is passed in context when `createPages` is called in `gatsby-node.js`, which will later be passed into a GraphQL query in the Layout component

```js:title=gatsby-node.js
exports.createPages = async ({ graphql, actions }) => {
  const { createPage } = actions;

  // query for all markdown

  result.data.allMdx.edges.forEach(({ node }) => {
    createPage({
      path: node.fields.slug,
      component: path.resolve(`./src/components/markdown-layout.js`),
      // highlight-start
      context: {
        slug: node.fields.slug
      }
      // highlight-end
    });
  });
};
```

3. Now, import `Img` from `gatsby-image`, and `graphql` from `gatsby` into the template component, write a [pageQuery](/docs/page-query/) to get image data based on the passed in `slug` and pass that data to the `<Img />` component:

```jsx:title=markdown-layout.jsx
import React from "react";
import { graphql } from "gatsby"; // highlight-line
import Img from "gatsby-image"; // highlight-line

export default ({ children, data }) => (
  <main>
    // highlight-start
    <Img
      fluid={data.markdown.frontmatter.image.childImageSharp.fluid}
      alt="A corgi smiling happily"
    />
    // highlight-end
    {children}
  </main>
);

// highlight-start
export const pageQuery = graphql`
  query PostQuery($slug: String) {
    markdown: mdx(fields: { slug: { eq: $slug } }) {
      id
      frontmatter {
        image {
          childImageSharp {
            fluid {
              ...GatsbyImageSharpFluid
            }
          }
        }
      }
    }
  }
`;
// highlight-end
```

4. Run `gatsby develop`, which will generate images for files sourced in the filesystem

#### Ulteriori risorse

- [Example repository using this recipe](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipes-gatsby-image)
- [Featured images with frontmatter](/docs/working-with-images-in-markdown/#featured-images-with-frontmatter-metadata)
- [Gatsby Image API](/docs/gatsby-image/)
- [Using Gatsby Image](/docs/using-gatsby-image)
- [More on working with images in Gatsby](/docs/working-with-images/)

## 8. Transforming data

Transforming data in Gatsby is plugin-driven. Transformer plugins take data fetched using source plugins, and process it into something more usable (e.g. JSON into JavaScript objects, and more).

### Transforming Markdown into HTML

The `gatsby-transformer-remark` plugin can transform Markdown files to HTML.

#### Prerequisiti

- A Gatsby site with `gatsby-config.js` and an `index.js` page
- A Markdown file saved in your Gatsby site `src` directory
- A source plugin installed, such as `gatsby-source-filesystem`
- The `gatsby-transformer-remark` plugin installed

#### Istruzioni

1. Add the transformer plugin in your `gatsby-config.js`:

```js:title=gatsby-config.js
plugins: [
  // not shown: gatsby-source-filesystem for creating nodes to transform
  `gatsby-transformer-remark`
],
```

2. Add a GraphQL query to the `index.js` file of your Gatsby site to fetch `MarkdownRemark` nodes:

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
`;
```

3. Restart the development server and open GraphiQL at <http://localhost:8000/___graphql>. Explore the fields available on the `MarkdownRemark` node.

#### Ulteriori risorse

- [Tutorial on transforming Markdown to HTML](/tutorial/part-six/#transformer-plugins) using `gatsby-transformer-remark`
- Browse available transformer plugins in the [Gatsby plugin library](/plugins/?=transformer)

## 9. Deploying your site

Showtime. Once you are happy with your site, you are ready to go live with it!

### Preparing for deployment

#### Prerequisiti

- Un [sito Gatsby](/docs/quick-start)
- La [CLI di Gatsby](/docs/gatsby-cli/) installata

#### Istruzioni

1. Stop your development server if it is running (`Ctrl + C` on your command line in most cases)

2. For the standard site path at the root directory (`/`), run `gatsby build` using the Gatsby CLI on the command line. The built files will now be in the `public` folder.

```shell
gatsby build
```

3. To include a site path other than `/` (such as `/site-name/`), set a path prefix by adding the following to your `gatsby-config.js` and replacing `yourpathprefix` with your desired path prefix:

```js:title=gatsby-config.js
module.exports = {
  pathPrefix: `/yourpathprefix`
};
```

There are a few reasons to do this -- for instance, hosting a blog built with Gatsby on a domain with another site not built on Gatsby. The main site would direct to `example.com`, and the Gatsby site with a path prefix could live at `example.com/blog`.

4. With a path prefix set in `gatsby-config.js`, run `gatsby build` with the `--prefix-paths` flag to automatically add the prefix to the beginning of all Gatsby site URLs and `<Link>` tags.

```shell
gatsby build --prefix-paths
```

5. Make sure that your site looks the same when running `gatsby build` as with `gatsby develop`. By running `gatsby serve` when you build your site, you can test out (and debug if necessary) the finished product before deploying it live.

```shell
gatsby build && gatsby serve
```

#### Ulteriori risorse

- Walk through building and deploying an example site in [tutorial part one](/tutorial/part-one/#deploying-a-gatsby-site)
- Learn about [performance optimization](/docs/performance/)
- Read about [other deployment related topics](/docs/preparing-for-deployment/)
- Check out the [deployment docs](/docs/deploying-and-hosting/) for specific hosting platforms and how to deploy to them

### Deploying to Netlify

Use [`netlify-cli`](https://www.netlify.com/docs/cli/) to deploy your Gatsby application without leaving the command-line interface.

#### Prerequisiti

- Un [sito Gatsby](/docs/quick-start) with a single component `index.js`
- The [netlify-cli](https://www.npmjs.com/package/netlify-cli) package installed
- La [CLI di Gatsby](/docs/gatsby-cli/) installata

#### Istruzioni

1. Build your gatsby application using `gatsby build`

2. Login into Netlify using `netlify login`

3. Run the command `netlify init`. Select the "Create & configure a new site" option.

4. Choose a custom website name if you want or press enter to receive a random one.

5. Choose your [Team](https://www.netlify.com/docs/teams/).

6. Change the deploy path to `public/`

7. Make sure that everything looks fine before deploying to production using `netlify deploy --prod`

#### Ulteriori risorse

- [Hosting on Netlify](/docs/hosting-on-netlify)
- [gatsby-plugin-netlify](/packages/gatsby-plugin-netlify)

### Deploying to ZEIT Now

Use [Now CLI](https://zeit.co/download) to deploy your Gatsby application without leaving the command-line interface.

#### Prerequisiti

- A [ZEIT Now](https://zeit.co/signup) account
- Un [sito Gatsby](/docs/quick-start) with a single component `index.js`
- [Now CLI](https://zeit.co/download) package installed
- [Gatsby CLI](/docs/gatsby-cli) installed

#### Istruzioni

1. Login into Now CLI using `now login`

2. Change to the directory of your Gatsby.js application in the Terminal if you aren't already there

3. Run `now` to deploy it

#### Ulteriori risorse

- [Deploying to ZEIT Now](/docs/deploying-to-zeit-now/)
