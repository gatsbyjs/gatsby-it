---
title: Azioni
description: Documentazione sulle azioni e su come possono aiutare a manipolare lo stato all'interno di Gatsby
jsdoc:
  - "src/redux/actions/public.js"
  - "src/redux/actions/restricted.ts"
contentsHeading: Funzioni
---

Gatsby usa [Redux](http://redux.js.org) internamente per gestire lo stato. Quando implementi una API Gatsby, ti viene servita una serie di azioni (equivalenti alle azioni legate a [bindActionCreators](https://redux.js.org/api/bindactioncreators/) in Redux), le quali possono essere usate per manipolare lo stato sul tuo sito.

L'oggetto `actions` contiene le funzioni e queste possono essere estratte individualmente usando la destrutturazione degli oggetti ES6.

```javascript
// Per la funzione createNodeField
exports.onCreateNode = ({ node, getNode, actions }) => {
  const { createNodeField } = actions
}
```
