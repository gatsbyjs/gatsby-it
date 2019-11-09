---
title: Impariamo a conoscere elementi costitutivi di Gatsby
typora-copy-images-to: ./
disableTableOfContents: true
---

Nella [**sezione precendente**](/tutorial/part-zero/), hai preparato il tuo ambiente di sviluppo locale installando il software necessario e creando il tuo primo sito web con Gatsby, usando [**il template starter “hello world”**](https://github.com/gatsbyjs/gatsby-starter-hello-world). Analizziamo adesso nel dettaglio il codice generato da questo template starter.

## Come utilizzare i template starter di Gatsby

Nel [**tutorial parte zero**](/tutorial/part-zero/), hai creato un nuovo sito web basato sul template starter "hello world" utilizzando il seguente comando:

```shell
gatsby new hello-world https://github.com/gatsbyjs/gatsby-starter-hello-world
```

Quando crei un sito Gatsby, puoi usare la seguente struttura di comando per creare un un nuovo sito basato su qualsiasi template starter di Gatsby:

```shell
gatsby new [SITE_DIRECTORY_NAME] [URL_OF_STARTER_GITHUB_REPO]
```

<<<<<<< HEAD
Se ometti una URL alla fine, Gatsby andrà a generare per te, in maniera automatica, un sito basato sul template [**default starter**](https://github.com/gatsbyjs/gatsby-starter-default). Per questa sezione del tutorial, continua a lavorare sul sito "Hello World" che hai già creato nel tutorial zero.
=======
If you omit a URL from the end, Gatsby will automatically generate a site for you based on the [**default starter**](https://github.com/gatsbyjs/gatsby-starter-default). For this section of the tutorial, stick with the “Hello World” site you already created in tutorial part zero. You can learn more about [modifying starters](/docs/modifying-a-starter) in the docs.

> > > > > > > d8b039738bb3fd6173f4578f149c3d5abeb27722

### ✋ Apri il codice

Nel tuo editor, apri il codice generato per il tuo sito "Hello World" e dai un'occhiata alle varie directory e ai file contenuti nella directory ‘hello-world’. Dovrebbe apparire qualcosa del genere:

![Hello World project in VS Code](01-hello-world-vscode.png)

_Nota: Attenzione, l'editor in figura è Visual Studio Code. Se stai utilizzando un altro editor potresti visualizzare qualcosa di leggermente diverso._

Diamo un'occhiata al codice che alimenta la homepage.

> 💡 Se hai interrotto il server di sviluppo dopo aver eseguito il comando `gatsby develop` nella sezione precedente, riavvialo immediatamente - è giunto il momento di apportare alcune modifiche al sito hello-world!

## Familiarizzare con le pagine di Gatsby

Apri la directory `/src` nel tuo code editor. All'interno è presente una singola directory: `/pages`.

Apri il file `src/pages/index.js`. Il codice in questo file crea un componente che contiene un singolo div e un po' di testo - per l'esattezza, “Hello world!”

### ✋ Apportare modifiche alla homepage di “Hello World”

1.  Cambia il testo “Hello World!” con “Hello Gatsby!” e salva il file. Potrai notare, se le finestre sono affiancate, che le modifiche al tuo codice e ai tuoi contenuti avvengono quasi istantaneamente nel browser dopo aver salvato il file.

<video controls="controls" autoplay="true" loop="true">
  <source type="video/mp4" src="./02-demo-hot-reloading.mp4"></source>
  <p>Sorry! Your browser doesn't support this video.</p>
</video>

> 💡 Gatsby utilizza la funzionalità **hot reloading** per velocizzare il processo di sviluppo. Quando il server di sviluppo Gatsby è in esecuzione, i file del sito Gatsby sono “controllati” in background - tutte le volte che salvi un file, le tue modifiche si riflettono immediatamente nel browser. Non è necessario aggiornare la pagina o riavviare il server di sviluppo - le tue modifiche verranno visualizzate all'istante.

2.  Ora puoi rendere le tue modifiche un po' più visibili. Prova a sostituire il codice in `src/pages/index.js` con il codice in basso e salva le modifiche. Vedrai dei cambiamenti nel testo - il colore del testo sarà viola e la dimensione del carattere sarà più grande.

```jsx:title=src/pages/index.js
import React from "react";

export default () => (
  <div style={{ color: `purple`, fontSize: `72px` }}>Hello Gatsby!</div>
);
```

> 💡 Discuteremo più dettagliatamente dello styling in Gatsby nella [**seconda parte**](/tutorial/part-two/) del tutorial.

3.  Rimuovi lo stile della dimensione del carattere, cambia il testo “Hello Gatsby!” in intestazione di livello uno, e aggiungi un paragrafo sotto l'intestazione (header).

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  {/* highlight-start */}
  <div style={{ color: `purple` }}>
    <h1>Hello Gatsby!</h1>
    <p>What a world.</p>
  {/* highlight-end */}
  </div>
)
```

![More changes with hot reloading](03-more-hot-reloading.png)

4.  Aggiungi un'immagine. (In questo caso, un'immagine a caso da Unsplash).

```jsx:title=src/pages/index.js
import React from "react";

export default () => (
  <div style={{ color: `purple` }}>
    <h1>Hello Gatsby!</h1>
    <p>What a world.</p>
    {/* highlight-next-line */}
    <img src="https://source.unsplash.com/random/400x200" alt="" />
  </div>
);
```

![Add image](04-add-image.png)

### Aspetta… HTML nel nostro codice JavaScript?

_Se hai familiarità con React e JSX, non esitare a saltare questo paragrafo._ Se non hai mai utilizzato React in precedenza, potresti chiederti cosa ci faccia un codice HTML in una funzione JavaScript. Oppure per quale motivo stiamo importando `react` nella prima riga ma apparentemente non lo usiamo da nessuna parte. Questa sintassi ibrida “HTML-in-JS” è in realtà un'estensione di JavaScript, proprio per React, chiamata JSX. Puoi tranquillamente seguire questo tutorial senza avere nessuna esperienza con React, ma se sei curioso, eccoti un piccolo assaggio…

Considera il contenuto iniziale del file `src/pages/index.js`:

```jsx:title=src/pages/index.js
import React from "react";

export default () => <div>Hello world!</div>;
```

In puro codice JavaScript, somiglia più a questo:

```javascript:title=src/pages/index.js
import React from "react";

export default () => React.createElement("div", null, "Hello world!");
```

Ora forse sarà più chiaro l'utilizzo di import `'react'`! Ma aspetta. Stai utilizzando JSX, non puro HTML o JavaScript. Come fa il browser a leggerlo? Per essere brevi: non lo legge. I siti Gatsby sono dotati di strumenti già configurati per convertire il tuo codice sorgente in qualcosa che il browser può interpretare.

## Utilizzare i componenti

La homepage che stavi modificando è stata creata definendo un componente di pagina. Che cosa è esattamente un “componente”?

In pratica, un componente è un blocco predefinito per il tuo sito; è un blocco autonomo di codice che descrive una sezione della UI (interfaccia utente).

Gatsby si basa su React. Quando parliamo di usare e definire i **componenti**, stiamo in realtà parlando di **componenti React** — blocchi autonomi di codice (di solito scritti in JSX) che possono accettare degli input e restituire elementi React che descrivono una sezione della UI.

Uno dei grandi cambiamenti richiesti quando inizi a lavorare con i componeti (se sei uno sviluppatore) è che adesso il tuo codice CSS, HTML, e JavaScript è strettamente legato e spesso vive all'interno dello stesso file.

Sebbene possa sembrare un cambiamento molto semplice, ha in realtà profonde implicazioni sul tuo modo di costruire siti web.

Considera ad esempio la creazione di un bottone standard. In passato, avresti creato una classe CSS (come `.primary-button`) con il tuo stile personalizzato e poi avresti usato quello stile a seconda del caso. Ad esempio:

```html
<button class="primary-button">Click me</button>
```

Nel mondo dei componenti, puoi invece creare un componente `PrimaryButton` con lo stile del tuo bottone e usarlo in tutte le parti del sito:

<!-- prettier-ignore -->
```jsx
<PrimaryButton>Click me</PrimaryButton>
```

I componenti diventano i mattoni del tuo sito. Invece di essere limitato ai mattoni forniti dal browser, ad esempio `<button />`, puoi facilmente creare nuovi mattoni che si adattano con eleganza alle esigenze del tuo progetto.

### ✋ Utilizzare i componeti di pagina

Ciascun componente react definito in `src/pages/*.js` diventerà automaticamente una pagina. Vediamo come funziona.

Hai già un file `src/pages/index.js` generato con lo starter “Hello World”. Creiamo una pagina di informazioni (about).

1.  Crea un nuovo file `src/pages/about.js`, copia il seguente codice nel file e salva.

```jsx:title=src/pages/about.js
import React from "react";

export default () => (
  <div style={{ color: `teal` }}>
    <h1>About Gatsby</h1>
    <p>Such wow. Very React.</p>
  </div>
);
```

2.  Vai su http://localhost:8000/about/.

![New about page](05-about-page.png)

Semplicemente inserendo un componente React nel file `src/pages/about.js`, hai ottenuto una pagina accessibile su `/about`.

### ✋ Usare i sotto-componenti

Supponiamo che la homepage e la pagina di informazioni (about) siano diventate abbastanza complesse e che tu stia riscrivendo molte cose. Puoi utilizzare i sotto-componenti per dividere la UI in sezioni riutilizzabili. Entrambe le tue pagine hanno un header `<h1>` - crea un componente che descriva un `Header`.

1.  Crea una nuova directory in `src/components` e un file chiamato `header.js` all'interno della stessa directory.
2.  Aggiungi il seguente codice al nuovo file `src/components/header.js`.

```jsx:title=src/components/header.js
import React from "react";

export default () => <h1>This is a header.</h1>;
```

3.  Modifica il file `about.js` al fine di importare il coponente `Header`. Sostituisci il markup `h1` con `<Header />`:

```jsx:title=src/pages/about.js
import React from "react";
import Header from "../components/header"; // highlight-line

export default () => (
  <div style={{ color: `teal` }}>
    <Header /> {/* highlight-line */}
    <p>Such wow. Very React.</p>
  </div>
);
```

![Adding Header component](06-header-component.png)

Nel browser, il testo dell'intestazione “About Gatsby” dovrebbe ora essere stato sostituito con “This is a header.” Ma non vogliamo che la pagina “About” dica “This is a header.” Vogliamo che dica, “About Gatsby”.

4.  Torna a `src/components/header.js` e apporta le seguenti modifiche:

```jsx:title=src/components/header.js
import React from "react"

export default props => <h1>{props.headerText}</h1> {/* highlight-line */}
```

5.  Torna su `src/pages/about.js` e modifica come segue:

```jsx:title=src/pages/about.js
import React from "react";
import Header from "../components/header";

export default () => (
  <div style={{ color: `teal` }}>
    <Header headerText="About Gatsby" /> {/* highlight-line */}
    <p>Such wow. Very React.</p>
  </div>
);
```

![Passing data to header](07-pass-data-header.png)

Ora dovresti di nuovo vedere l'intestazione “About Gatsby”!

### Cosa sono i “props”?

Abbiamo precedentemente definito i componenti React come pezzi di codice riutilizzabili che descrivono una UI. Per rendere dinamici questi pezzi riutilizzabili devi essere in grado di equipaggiarli con dati differenti. Puoi farlo con degli input chiamati “props". I props sono proprietà (sufficientemente specifiche) fornite ai componenti React.

In `about.js` hai passato un prop `headerText` con il volore di `"About Gatsby"` al sotto-componente importato `Header`:

```jsx:title=src/pages/about.js
<Header headerText="About Gatsby" />
```

In `header.js`, il componente header aspetta di ricevere il prop `headerText` (perché il codice che hai scritto lo prevede). Quindi puoi accedervi in questo modo:

```jsx:title=src/components/header.js
<h1>{props.headerText}</h1>
```

> 💡 In JSX è possibile incapsulare ogni espressione JavaScript racchiudendola con `{}`. Questo è il modo in cui puoi accedere alla proprietà `headerText` (o “prop!”) attraverso l'oggetto “props”.

Se avessi passato un altro prop al tuo componente `<Header />`, come ad esempio...

```jsx:title=src/pages/about.js
<Header headerText="About Gatsby" arbitraryPhrase="is arbitrary" />
```

...avresti potuto accedere anche al prop `arbitraryPhrase`: `{props.arbitraryPhrase}`.

6.  Per evidenziare come tutto questo renda i tuoi componenti riutilizzabili, aggiungi un altro componente `<Header />` alla pagina di informazioni, inserisci il seguente codice nel file `src/pages/about.js` e salva.

```jsx:title=src/pages/about.js
import React from "react";
import Header from "../components/header";

export default () => (
  <div style={{ color: `teal` }}>
    <Header headerText="About Gatsby" />
    <Header headerText="It's pretty cool" /> {/* highlight-line */}
    <p>Such wow. Very React.</p>
  </div>
);
```

![Doppia intestazione per mostrare i componenti riutilizzabili](08-duplicate-header.png)

E il gioco è fatto; ecco una seconda intestazione — senza riscrivere il codice — utilizzando i props per passare dati diversi.

### Utilizzare i componenti di layout

I componenti del layout comprendono le sezioni di un sito che intendi condividere su più pagine. Per esempio, i siti Gatsby hanno di solito un componente di layout con una intestazione ed un piè pagina condivisi. Altri elementi in comune da aggiungere includono una barra laterale e/o un menu di navigazione.

Esploreremo i componenti del layout nella [**terza parte**](/tutorial/part-three/).

## Collegamenti tra le pagine

Spesso vorrai collegare le tue pagine — diamo un'occiata al routing in un sito Gatsby.

### ✋ Utilizzare il componente `<Link />`

1.  Apri il componete della pagina di index (`src/pages/index.js`), importa il componente `<Link />` da Gatsby, aggiungi ill componente `<Link />` nella sezione header, e configuralo con una proprietà `to` con il valore di `"/contact/"` per il percorso:

```jsx:title=src/pages/index.js
import React from "react";
import { Link } from "gatsby"; // highlight-line
import Header from "../components/header";

export default () => (
  <div style={{ color: `purple` }}>
    <Link to="/contact/">Contact</Link> {/* highlight-line */}
    <Header headerText="Hello Gatsby!" />
    <p>What a world.</p>
    <img src="https://source.unsplash.com/random/400x200" alt="" />
  </div>
);
```

Quando clicchi sul link "Contact" della homepage, dovresti vedere...

![Gatsby dev 404 page](09-dev-404.png)

...la pagina di sviluppo 404 di Gatsby. Perché? Perché stai provando a collegarti a una pagina che non è stata ancora creata.

2.  Dovrai ora creare un componente di pagina per la tua nuova pagina "Contact" in `src/pages/contact.js` e collegarla alla homepage:

```jsx:title=src/pages/contact.js
import React from "react";
import { Link } from "gatsby";
import Header from "../components/header";

export default () => (
  <div style={{ color: `teal` }}>
    <Link to="/">Home</Link>
    <Header headerText="Contact" />
    <p>Send us a message!</p>
  </div>
);
```

<<<<<<< HEAD
Dopo aver salvato il file, dovresti vedere la pagina dei contatti e dovresti essere in grado di collegarti attraverso la homepage.
=======
After you save the file, you should see the contact page and be able to follow the link to the homepage.

> > > > > > > d8b039738bb3fd6173f4578f149c3d5abeb27722

<video controls="controls" loop="true">
  <source type="video/mp4" src="./10-linking-between-pages.mp4"></source>
  <p>Sorry! You browser doesn't support this video.</p>
</video>

Il componente Gatsby `<Link />` serve per creare collegamenti tra le pagine del tuo sito. Per i link a pagine che non sono gestite dal tuo sito Gatsby, puoi usare il tipico tag HTML `<a>`.

## Distribuire un sito Gatsby

Gatsby.js è un _moderno generatore di siti_, e questo significa che non ci sono server da installare o complicati database da distribuire. Invece, il comando Gatsby `build` crea una directory di file statici HTML e JavaScript che è possibili distribuire con un servizio di hosting per siti statici.

Prova a usare [Surge](http://surge.sh/) per distribuire il tuo primo sito web Gatsby. Surge è uno dei tanti "host di siti statici" che permette di distribuire i siti web creati con Gatsby.

Se non hai precedentemente installato &amp; configurato Surge, apri una nuova finestra del terminale e installa gli strumenti della riga di comando:

```shell
npm install --global surge

# Then create a (free) account with them
surge login
```

Fatto questo, crea il tuo sito eseguendo il seguente comando nel terminale, nella root (cartella principale) del sito (suggerimento: assicurati di eseguire il comando nella root del sito, in questo caso nella cartella hello-world, aprendo una nuova scheda nella stessa finestra che hai usato per eseguire `gatsby develop`):

```shell
gatsby build
```

La compilazione dovrebbe richiedere 15-30 secondi. Una volta completata, è interessante dare un'occhiata ai file che il comando `gatsby build` ha appena preparato per la distribuzione.

Dai un'occchiata alla lista di file generata digitando nel terminale, a livello della root del tuo sito, il seguente comando, e osserva la directory `public`:

```shell
ls public
```

Puoi quindi distribuire il tuo sito, pubblicando i file generati su surge.sh

```shell
surge public/
```

Una volta che il processo sarà concluso, dovresti vedere nel terminale qualcosa del genere:

![Screenshot of publishing Gatsby site with Surge](surge-deployment.png)

Apri l'indirizzo web indicato nella riga inferiore (in questo caso `lowly-pain.surge.sh`) e potrai vedere il tuo sito appena pubblicato! Ottimo lavoro!

## ➡️ Qual è il prossimo passo?

In questa sezione:

- Hai imparato cosa sono gli starter di Gatsby e come usarli per creare nuovi progetti
- Hai scoperto JSX
- Hai studiato i componenti
- Hai imparato cosa sono i componenti e sotto-componenti di pagina in Gatsby
- Hai imparato cosa sono i “props” react e come riutilizzarli nei componenti React

Passa ora a: [**come aggiungere stili al tuo sito**](/tutorial/part-two/)!
