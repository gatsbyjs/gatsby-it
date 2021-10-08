# Guida di stile

In questo file puoi trovare le regole specifiche da seguire per la traduzione in italiano di Gatsby.

## Regole

### Testo nei blocchi di codice

Non tradurre il testo nei blocchi di codice, fatta eccezione per i commenti. Se vuoi, puoi tradurre il testo nelle stringhe, ma fai attenzione a non tradurre le stringhe che si riferiscono al codice!

Esempio:

```js
// Example
import React from "react"
export default () => (
  <div style={{ color: `purple`, fontSize: `72px` }}>Hello Gatsby!</div>
)
```

✅ Si può:

```js
// Esempio
import React from "react"
export default () => (
  <div style={{ color: `purple`, fontSize: `72px` }}>Hello Gatsby!</div>
)
```

✅ Anche questo va bene:

```js
// Esempio
import React from "react"
export default () => (
  <div style={{ color: `purple`, fontSize: `72px` }}>Ciao, Gatsby!</div>
)
```

❌ Non si può:

```js
// Esempio
import React from "react"
export default () => (
  // 'purple' è una parola chiave di CSS
  <div style={{ color: `viola`, fontSize: `72px` }}>Ciao, Gatsby!</div>
)
```

❌ Non fare assolutamente:

```js
importa Reagire da "reagire"
esporta predefinito () => (
   <div stile={{ colore: `viola`, fontSize:` 72px` }}>Ciao, Gatsby!</div>
)
```

### Collegamenti esterni

Se un collegamento esterno punta a un articolo di una fonte come [MDN] o [Wikipedia], ed esiste una versione in lingua di quell'articolo che sia di buona qualità, considera la possibilità di far puntare il collegamento a quell'articolo.

[mdn]: https://developer.mozilla.org/it/
[wikipedia]: https://it.wikipedia.org/wiki/Pagina_principale

Esempio:

```md
React is an open source [JavaScript](https://en.wikipedia.org/wiki/JavaScript) library.
```

✅ OK:

```md
React è una libreria [JavaScript](https://it.wikipedia.org/wiki/JavaScript) open source.
```

Per i link che non hanno un equivalente in lingua (Stack Overflow, video di YouTube, ecc.), usa il collegamento in inglese.

## Glossario

In questa sezione puoi trovare una guida alla traduzione dei termini tecnici più comuni.

| Terminologia | Traduzione |
| ------------ | ---------- |
| Theme        | Tema       |
| Gatsby CLI   | Gatsby CLI |
| ...          | ??         |

Termini e frasi che non vanno tradotti in italiano:

- Query
- Plugin

Aiutaci ad ampliare questa lista con una PR!
