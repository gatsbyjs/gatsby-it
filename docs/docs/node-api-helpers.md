---
title: Node API Helpers
description: Documentazione sugli API helper per creare nodi nel layer dati del GraphQL di Gatsby
jsdoc: ["src/utils/api-node-helpers-docs.js"]
apiCalls: NodeAPIHelpers
contentsHeading: Shared helpers
showTopLevelSignatures: true
---

Il primo argomento passato ad ogni [API dei nodi di Gatsby](/docs/node-apis/) è un oggetto contenente un insieme di helper. Gli helper condivisi da tutte le API di nodo di Gatsby sono documentate nella sezione [Helper condivisi](#apis).

```javascript
// in gatsby-node.js
exports.createPages = gatsbyNodeHelpers => {
  const { actions, reporter } = gatsbyNodeHelpers
  // usa helper
}
```

Una convenzione comune è destrutturare gli helper direttamente nella lista degli argomenti:

```javascript
// in gatsby-node.js
exports.createPages = ({ actions, reporter }) => {
  // usa helpers
}
```

## Nota

Alcune API forniscono helper aggiuntivi. Per esempio `createPages` fornisce la funzione `graphql`. Controlla la documentazione delle specifiche API in [Gatsby Node APIs](/docs/node-apis/) per i dettagli.
