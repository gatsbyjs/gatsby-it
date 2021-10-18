---
title: "Ricette: Distribuire il tuo sito"
tableOfContentsDepth: 1
---

È ora di andare in scena. Una volta che sei contento del tuo sito, sei pronto per metterlo in linea!

## Prepararsi per la distribuzione

### Prerequisiti

- Un [sito Gatsby](/docs/quick-start)
- La linea di comando [Gatsby CLI](/docs/gatsby-cli) installata

### Indicazioni

1. Ferma il tuo server di sviluppo se è in esecuzione (`Ctrl + C` nella tua riga di comando nella maggior parte dei casi)

2. Per il percorso standard del sito esegui `gatsby build` nella cartella radice (`/`) usando la Gatsby CLI nella riga di comando. I file compilati saranno ora nella cartella `public`.

```shell
gatsby build
```

3. Per includere un indirizzo del sito oltre a `/` (come `/nome-sito/`), imposta un prefisso di percorso aggiungendo ciò che segue al tuo `gatsby-config.js` e rimpiazzando `prefissodeltuopercorso` con il prefisso desiderato:

```js:title=gatsby-config.js
module.exports = {
  pathPrefix: `/prefissodeltuopercorso`,
}
```

Ci sono alcune ragioni per farlo --  per esempio, hostare un blog compilato con Gatsby su un dominio con un altro sito non compilato con Gatsby. Il sito principale sarà diretto su `esempio.com`, e il sito Gatsby con un prefisso di percorso potrebbe vivere su `esempio.com/blog`.

4. Con un prefisso di percorso impostato in `gatsby-config.js`, esegui `gatsby build` col flag `--prefix-paths` per aggiungere automaticamente il prefisso all'inizio di tutti gli URL del sito Gatsby e dei tag `<Link>`.

```shell
gatsby build --prefix-paths
```

5. Assicurati che il tuo sito si visualizzi nello stesso modo sia quando si esegue `gatsby build` che quando si esegue `gatsby develop`. Eseguendo `gatsby serve` quando si compila il sito, si può testare (e debuggare se necessario) il prodotto finito prima di distribuire il sito.

```shell
gatsby build && gatsby serve
```

### Risorse aggiuntive

- Guida passo passo sulla compilazione e distribuzione di un sito di esempio in [tutorial parte uno](/tutorial/part-one/#deploying-a-gatsby-site)
- Impara sull'[ottimizzazione delle performance](/docs/performance/)
- Leggi su [altri argomenti relativi alla distribuzione](/docs/preparing-for-deployment/)
- Controlla la [documentazione sulla distribuzione](/docs/deploying-and-hosting/) per piattaforme di hosting specifiche e come distribuire su esse

## Distribuire su Netlify

Usa [`netlify-cli`](https://www.netlify.com/docs/cli/) per distribuire la tua applicazione Gatsby senza lasciare la riga di comando.

### Prerequisiti

- Un [sito Gatsby](/docs/quick-start) con una singola componente `index.js`
- Il pacchetto [netlify-cli](https://www.npmjs.com/package/netlify-cli) installato
- La [Gatsby CLI](/docs/gatsby-cli) installata

### Indicazioni

1. Compila la tua applicazione Gatsby con `gatsby build`

2. Effettua il login in Netlify con `netlify login`

3. Esegui il comando `netlify init`. Seleziona l'opzione "Create & configure a new site" (Crea e configura un nuovo sito).

4. Se vuoi personalizza il nome del sito oppure premi invio per riceverne uno casuale.

5. Scegli il tuo [Team](https://www.netlify.com/docs/teams/).

6. Cambia il percorso di distibuzione in `public/`

7. Assicurati che tutto sia visualizzato e impostato correttamente prima di distribuire in produzione usando `netlify deploy -d . --prod`

### Risorse aggiuntive

- [Hostare su Netlify](/docs/hosting-on-netlify)
- [gatsby-plugin-netlify](/packages/gatsby-plugin-netlify)

## Distribuire su Vercel

Usa la [Vercel CLI](https://vercel.com/download) per distribuire la tua applicazione Gatsby senza lasciare la riga di comando.

### Prerequisiti

- Un account [Vercel](https://vercel.com/signup)
- Un [sito Gatsby](/docs/quick-start) con una singola componente `index.js`
- Il pacchetto [Vercel CLI](https://vercel.com/download) installato
- [Gatsby CLI](/docs/gatsby-cli) installato

### Indicazioni

1. Effettua il login nella Vercel CLI con `vercel login`

2. Spostati nel percorso della tua applicazione Gatsby.js nel terminale se non sei già là

3. Esegui `vercel` per distribuirla

### Risorse aggiuntive

- [Distribuire su Vercel](/docs/deploying-to-vercel/)

## Distribuire su Cloudflare Workers

Usa [`wrangler`](https://developers.cloudflare.com/workers/tooling/wrangler/) per distribuire la tua applicazione Gatsby globalmente senza lasciare la riga di comando.

### Prerequisiti

- Un account su [Cloudflare](https://dash.cloudflare.com/sign-up)
- Un [Workers Unlimited plan](https://developers.cloudflare.com/workers/about/pricing/) per \$5/mese per abilitare il KV store, il quale è necessario per servire i file di Gatsby.
- Un [sito Gatsby](/docs/quick-start) impostato con la Gatsby CLI
- [wrangler](https://developers.cloudflare.com/workers/tooling/wrangler/install/) installato globalmente (`npm install -g @cloudflare/wrangler`)

### Indicazioni

1. Compila la tua applicazione Gatsby con `gatsby build`
2. Esegui `wrangler config` dove ti sarà richiesto il tuo [token per le API Cloudflare](https://developers.cloudflare.com/workers/quickstart/#api-token)
3. Esegui `wrangler init --site`
4. Configura `wrangler.toml`. Prima aggiungi il campo [ID account](https://developers.cloudflare.com/workers/quickstart/#account-id-and-zone-id) e poi uno tra
   1. Un dominio workers.dev gratuito impostando `workers_dev = true`
   2. Un dominio personalizzato su Cloudflare impostando `workers_dev = false`, `zone_id = "abdc..` and `route = dominiopersonalizzato.com/*`
5. In `wrangler.toml` imposta `bucket = "./public"`
6. Esegui `wrangler publish` e il tuo sito sarà pubblicato in qualche secondo!

### Risorse aggiuntive

- [Hostare su Cloudflare](/docs/deploying-to-cloudflare-workers)

## Impostare Google Analytics

Usa `gatsby-plugin-google-analytics` per tracciare l'attività del sito e fornire statistiche su come gli utenti accedono al tuo sito.

### Prerequisiti

- Un [sito Gatsby](/docs/quick-start) con un file `gatsby-config.js` e una pagina `index.js`
- La [Gatsby CLI](/docs/gatsby-cli) installata
- Un dominio di un fornitore a tua scelta, ad es. [AWS](https://aws.amazon.com/getting-started/tutorials/get-a-domain/)

### Verifica il dominio in search.google.com

1. Naviga alla [Google search console](https://search.google.com/search-console/not-verified) per verificare il dominio cliccando su **Cerca proprietà** > **Aggiungi proprietà**. Scrivi il tuo dominio e premi Continua.
2. Aggiungi un record **TXT** alla tua configurazione DNS. Segui le indicazioni del tuo fornitore o fai riferimento alla [documentazione Google](https://support.google.com/a/answer/183895?hl=en).

### Collega il dominio a Google Analytics

1. Effettua il login su [Google Analytics](https://analytics.google.com/analytics/).
2. Clicca **Amministratore**.
3. Seleziona **Crea Propietà** nella colonna Proprietà.
4. Scegli **Web**.
5. Riempi i dettagli e clicca **Crea**.

### Ottenere il tuo `ID Monitoraggio` di Google Analytics

1. Entra nel tuo account Google Analytics.
2. Clicca **Amministratore**.
3. Seleziona un account dal menù nella colonna ACCOUNT.
4. Seleziona una proprietà dal menù nella colonna PROPRIETÀ.
5. Sotto Proprietà, clicca **Informazioni di monitoraggio** > **Codice monitoraggio**. Il tuo ID Monitoraggio è mostrato nella parte superiore della pagina.

### Usare l'ID nel plugin

1. Esegui `npm install gatsby-plugin-google-analytics` nel tuo terminale.
2. Aggiungi ciò che segue al tuo file `gatsby-config.js`.

```javascript:title="gatsby-config.js"
module.exports = {
  plugins: [
    {
      resolve: `gatsby-plugin-google-analytics`,
      options: {
        // sostituisci "UA-XXXXXXXXX-X" con il tuo ID monitoraggio
        trackingId: "UA-XXXXXXXXX-X",
      },
    },
  ],
}`
```

3. Compila e distribuisci il tuo sito per iniziare a vedere del traffico nella tua [dashboard di Google Analytics](https://analytics.google.com/analytics/web/).

### Risorse aggiuntive

- [Aggiungere l'analisi](/docs/adding-analytics/)
