---
titolo: Introduzione alla personalizzazione in Gatsby
typora-copy-images-to: ./
disableTableOfContents: true
---

<!-- Idea: Create a glossary to refer to. A lot of these terms get jumbled -->

<!--
  - Global styles
  - Component css
  - CSS-in-JS
  - CSS Modules

-->

Benvenuto nella seconda parte del tutorial di Gatsby!

## Cosa c'è in questo tutorial?

In questa parte, esplorerai le opzioni per perzonalizzare i siti web Gatsby e approfondire l'utilizzo di componenti React per costruire siti.

## Usare stili globali

Ogni sito ha degli stili globali. Questo include cose come la tipografia del sito e colori di sfondo. Questi stili impostano l'aspetto generale del sito — un po' come il colore e la texture di un muro impostano l'aspetto generale della stanza.

### Creare stili globali con file CSS standard

Una delle maniere più semplici per aggiungere stili globali ad un sito è usare un foglio di stile `.css` globale.

#### ✋ Creare un nuovo sito Gatsby

Inizia con il creare un nuovo sito Gatsby. Sarebbe meglio (soprattutto se non hai esperienza con la linea di comando) chiudere le finestre di terminale che hai usato per la [prima parte](/tutorial/part-one/) e iniziare una nuova sessione del terminale per la seconda parte.

Apri una nuova finestra del terminale, crea un nuovo sito gatsby "hello world", e fai partire il server di sviluppo:

```shell
gatsby new tutorial-part-two https://github.com/gatsbyjs/gatsby-starter-hello-world
cd tutorial-part-two
```

Adesso hai un nuovo sito Gatsby (bastao sul progetto iniziale Gatsby "hello world") con la seguente struttura:

```text
├── package.json
├── src
│   └── pages
│       └── index.js
```

#### ✋ Aggiungere stili ad un file CSS

1. Crea un file `.css` nel tuo progetto:

```shell
cd src
mkdir styles
cd styles
touch global.css
```

> Nota: Sentiti libero di creare queste directory e file usando il tuo editor di testo, se preferisci.

Dovresti adesso avere una struttura come questa:

```text
├── package.json
├── src
│   └── pages
│       └── index.js
│   └── styles
│       └── global.css
```

2. Definisci alcuni stili nel file `global.css`:

```css:title=src/styles/global.css
html {
  background-color: lavenderblush;
}
```

> Nota: Il posizionamento del file di esempio CSS in una cartella `/src/styles/` è arbitrario.

#### ✋ Includere il foglio di stile in `gatsby-browser.js`

1. Crea il `gatsby-browser.js`

```shell
cd ../..
touch gatsby-browser.js
```

La struttura del tuo progetto dovrebbe adesso somigliare a questa:

```text
├── package.json
├── src
│   └── pages
│       └── index.js
│   └── styles
│       └── global.css
├── gatsby-browser.js
```

> 💡 Cos'è `gatsby-browser.js`? Non preoccuparti troppo di questo e per il momento, sappi soltanto che `gatsby-browser.js` è uno dei tanti file speciali che Gatsby cerca e utilizza (se esistono). Qui, la denominazione del file **è** importante. Se vuoi approfondire di più adesso, dai un'occhiata alla [documentazione](/docs/browser-apis/).

2. Importa il tuo foglio di stile appena creato nel file `gatsby-browser.js`:

```javascript:title=gatsby-browser.js
import "./src/styles/global.css"

// or:
// require('./src/styles/global.css')
```

> Nota: Qui funzionano entrambe le sintassi CommonJS (`require`) e ES Module (`import`). Se non sei sicuro su quale scegliere, noi usiamo `import` la maggior parte del tempo. Però, quando lavoriamo con file che vengono eseguiti solo in un ambiente Node.js (come `gatsby-node.js`), si dovrà utilizzare `require`.

3. Avvia il server di sviluppo:

```shell
gatsby develop
```

Se dai un'occhiata al tuo progetto nel browser, dovresti vedere uno sfondo lavanda applicato al progetto iniziale "hello world":

![Hello World! lavanda](global-css.png)

> Suggerimento: Questa parte del tutorial si è focalizzata sul modo più veloce e semplice per iniziare a personalizzare un sito Gatsby — importando file CSS direttamente, usando `gatsby-browser.js`. In molti casi, il modo migliore per aggiungere stili globali è con una componente di layout condivisa. [Dai un'occhiata alla documentazione](/docs/global-css/) per approfondire sull'approccio.

## Usare CSS orientato ai componenti

Fino ad ora, abbiamo parlato dell'approccio più tradizionale di usare fogli di stile standard CSS. Adesso, parleremo di diversi metodi per modularizzare i CSS in modo da approcciarci allo stile in modo orientato ai componenti.

### CSS Modules

Esploriamo i **CSS Modules**. Citando dalla
[homepage CSS Module](https://github.com/css-modules/css-modules):

> Un **CSS Module** è un file CSS in cui tutti i nomi delle classi e delle animazioni
> sono definiti localmente per impostazione predefinita.

I CSS Modules sono molto popolari perché permettono di scrivere normalmente CSS ma con molta più sicurezza. Lo strumento genera automaticamente nomi unici di classi e animazioni, in modo da non doverti preoccupare di collisioni di nomi.

Gatsby funziona subito con i CSS Modules. Questo approccio è di gran lunga consigliato per chi sta inziando a costruire con Gatsby (e React in generale).

#### ✋ Costruire una nuova pagina usando i CSS Modules

In questa sezione, creerai un nuovo componente di pagina e personalizzerai quella pagina usando un CSS Module.

Per prima cosa, crea un nuovo componente `Container`.

1. Crea una nuova directory in `src/components` e poi, in questa nuova directory, crea un file chiamato `container.js` e copiaci il codice seguente:

```javascript:title=src/components/container.js
import React from "react"
import containerStyles from "./container.module.css"

export default ({ children }) => (
  <div className={containerStyles.container}>{children}</div>
)
```

Noterai che hai importato un file CSS module chiamato `container.module.css`. Adesso creiamo quel file.

2. Nella stessa directory (`src/components`), crea un file `container.module.css` e copia/incolla il codice seguente:

```css:title=src/components/container.module.css
.container {
  margin: 3rem auto;
  max-width: 600px;
}
```

Noterai che il nome del file termina con `.module.css` invece del solito `.css`. Questo è il modo con cui comunichi a Gatsby che questo file CSS dovrebbe essere elaborato come un CSS module piuttosto che un semplice CSS.

3. Crea un nuovo componente di pagina creando un file in
   `src/pages/about-css-modules.js`:

```javascript:title=src/pages/about-css-modules.js
import React from "react"

import Container from "../components/container"

export default () => (
  <Container>
    <h1>About CSS Modules</h1>
    <p>CSS Modules are cool</p>
  </Container>
)
```

Adesso, se visiti `http://localhost:8000/about-css-modules/`, la tua pagina dovrebbe somigliare a questa:

![Pagina con stili CSS Modules](css-modules-basic.png)

#### ✋ Personalizza un componente usando i CSS Modules

In questa sezione, creerai una lista di persone con nomi, avatar, e brevi biografie in latino. Creerai un componente `<User />` e personalizzerai quel componente usando un CSS Module.

1. Crea il file per il CSS in `src/pages/about-css-modules.module.css`.

2. Incolla il codice seguente nel nuovo file:

```css:title=src/pages/about-css-modules.module.css
.user {
  display: flex;
  align-items: center;
  margin: 0 auto 12px auto;
}

.user:last-child {
  margin-bottom: 0;
}

.avatar {
  flex: 0 0 96px;
  width: 96px;
  height: 96px;
  margin: 0;
}

.description {
  flex: 1;
  margin-left: 18px;
  padding: 12px;
}

.username {
  margin: 0 0 12px 0;
  padding: 0;
}

.excerpt {
  margin: 0;
}
```

3. Importa il nuovo file `src/pages/about-css-modules.module.css` nella pagina `about-css-modules.js` che hai creato in precedenza, modificando le prime righe del file in questo modo:

```javascript:title=src/pages/about-css-modules.js
import React from "react"
// highlight-next-line
import styles from "./about-css-modules.module.css"
import Container from "../components/container"

// highlight-next-line
console.log(styles)
```

Il codice della `console.log(styles)` farà il log dell'import risultante così da permetterti di vedere il risultato del tuo file `./about-css-modules.module.css` elaborato. Se apri la console dello sviluppatore (usando ad esempio gli strumenti da sviluppatore di Firefox o Chrome) nel tuo browser, vedrai:

![Importa il risultato del CSS Module nella console](css-modules-console.png)

Se lo compari al tuo file CSS, vedrai che ogni classe è adesso una chiave nell'oggetto importato puntante ad una lunga stringa, ad esempio `avatar` punta a `src-pages----about-css-modules-module---avatar---2lRF7`. Questi sono i nomi delle classi che i CSS Modules generano. L'unicità è garantita all'interno del sito. E poiché hai bisogno di importarli per usare le classi, non c'è mai dubbio su dove alcuni CSS vengano usati.

4. Crea un nuovo componente `<User />` in linea nella pagina componente `about-css-modules.js`. Modifica `about-css-modules.js` in modo da farlo somigliare al seguente codice:

```jsx:title=src/pages/about-css-modules.js
import React from "react"
import styles from "./about-css-modules.module.css"
import Container from "../components/container"

console.log(styles)

// highlight-start
const User = props => (
  <div className={styles.user}>
    <img src={props.avatar} className={styles.avatar} alt="" />
    <div className={styles.description}>
      <h2 className={styles.username}>{props.username}</h2>
      <p className={styles.excerpt}>{props.excerpt}</p>
    </div>
  </div>
)
// highlight-end

export default () => (
  <Container>
    <h1>About CSS Modules</h1>
    <p>CSS Modules are cool</p>
    {/* highlight-start */}
    <User
      username="Jane Doe"
      avatar="https://s3.amazonaws.com/uifaces/faces/twitter/adellecharles/128.jpg"
      excerpt="I'm Jane Doe. Lorem ipsum dolor sit amet, consectetur adipisicing elit."
    />
    <User
      username="Bob Smith"
      avatar="https://s3.amazonaws.com/uifaces/faces/twitter/vladarbatov/128.jpg"
      excerpt="I'm Bob Smith, a vertically aligned type of guy. Lorem ipsum dolor sit amet, consectetur adipisicing elit."
    />
    {/* highlight-end */}
  </Container>
)
```

> Suggerimento: Generalmente, se utilizzi un componente di un sito in diversi posti, esso dovrebbe trovarsi nel suo proprio file di modulo nella directory `components`. Ma, se è utilizzato solo in un file, crealo in linea.

La pagina finale dovrebbe adesso somigliare a questa:

![Pagina di lista di utenti con CSS Modules](css-modules-userlist.png)

### CSS-in-JS

CSS-in-JS è un approccio di personalizzazione orientato a componenti. Generalmente, è un pattern dove il [CSS è composto inline usando JavaScript](https://reactjs.org/docs/faq-styling.html#what-is-css-in-js).

#### Usare CSS-in-JS con Gatsby

Ci sono diverse librerie CSS-in-JS e molte presentano già i plugin Gatsby. Non approfondiremo un esempio CSS-in-JS in questo tutorial iniziale, ma ti incoraggiamo a [esplorare](/docs/styling/) cosa l'ecosistema ha da offrire. Ci sono dei mini-tutorial per due librerie, in particolare [Emotion](/docs/emotion/) e [Styled Components](/docs/styled-components/).

#### Letture consigliate su CSS-in-JS

Se sei interessato in altre letture, dai un'occhiata a [questa presentazione del 2014 di Christopher "vjeux" Chedeau che ha scatenato questo movimento](https://speakerdeck.com/vjeux/react-css-in-js) così come a [l'articolo più recente di Mark Dalgleish "A Unified Styling Language"](https://medium.com/seek-blog/a-unified-styling-language-d0c208de2660).

### Altre opzioni di CSS

Gatsby supporta quasi ogni possibile opzione di personalizzazione (Se non è ancora presente un plugin per la tua opzione di css preferita, [per favore contribuisci ad inserirla!](/contributing/how-to-contribute/))

- [Typography.js](/packages/gatsby-plugin-typography/)
- [Sass](/packages/gatsby-plugin-sass/)
- [JSS](/packages/gatsby-plugin-jss/)
- [Stylus](/packages/gatsby-plugin-stylus/)
- [PostCSS](/packages/gatsby-plugin-postcss/)

e altre!

## Cosa viene dopo?

Adesso continua con la [terza parte del tutorial](/tutorial/part-three/), dove imparerai riguardo i plugin Gatsby e i componenti di layout.
