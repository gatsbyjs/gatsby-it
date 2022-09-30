---
title: API di Node in Gatsby 
description: Documentazione della API di Node usate nel processo di compilazione per utenti comuni come creare pagine
jsdoc: ["src/utils/api-node-docs.ts"]
apiCalls: NodeAPI
---

Gatsby fornisce ai plugin e compilatori di sito molte API per controllare i dati del tuo sito nel livello dati di GraphQL.

## Plugin asincroni

Se i tuoi plugin effettuano operazioni asincrone (I/O su disco, accesso a database, chiamare API remote, ecc.), devi ritornare una Promise (usando esplicitamente l'API `Promise` o implicitamente usando la sintassi `async`/`await`) o usando la callback passata al terzo argomento. Gatsby necessita di sapere quando i plugin hanno concluso l'operazione, in quanto alcune API, per funzionare correttamente, necessitano prima che altre API usate precedentemente siano concluse. Vedi [Debuggare cicli asincroni](/docs/debugging-async-lifecycles/) per maggiori informazioni.

```javascript
// Async/await
exports.createPages = async () => {
  // fai qualcosa di asincrono
  const result = await fetchExternalData()
}

// Promise API
exports.createPages = () => {
  return new Promise((resolve, reject) => {
    // fai qualcosa di asincrono
  })
}

// Callback API
exports.createPages = (_, pluginOptions, cb) => {
  // fai qualcosa di asincrono
  cb()
}
```

Se il tuo plugin non esegue alcuna operazione asincorna, puoi ritornare direttamente.

## Uso

Implementa una qualsiasi di queste API esportandole da una file chiamato `gatsby-node.js` nella radice del tuo progetto.
