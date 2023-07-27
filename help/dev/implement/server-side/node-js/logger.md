---
title: Inizializzare [!DNL Adobe Target] SDK di Node.js per registrare le richieste
description: Scopri come registrare le richieste in [!DNL Adobe Target] SDK di Node.js.
feature: APIs/SDKs
exl-id: 5db3e301-47b3-4330-b185-c0c03f72e790
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '85'
ht-degree: 2%

---

# Logger (Node.js)

## Descrizione

Quando [inizializzazione dell’SDK](initialize-sdk.md), il `options.logger` object è un oggetto facoltativo. Tuttavia, per eseguire il debug in modo efficace quando si verifica un problema, è necessario `logger` è necessario fornire l&#39;oggetto durante l&#39;inizializzazione dell&#39;SDK.

Il `logger` l&#39;oggetto deve avere `debug()` e un `error()` metodo. Quando viene fornito un logger appropriato, ad esempio `console`, [!DNL Target] Le richieste e le risposte verranno registrate.

## Esempio

### Node.js

```js {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");
const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg",
  logger: console
};

const targetClient = TargetClient.create(CONFIG);

const request = {
    execute: {
        mboxes: [{
            name: "a1-serverside-ab",
            index: 1
        }]
    }
};

const response = await targetClient.getOffers({ request, targetCookie });
```

Dovresti vedere le richieste e le risposte stampate nella console.
