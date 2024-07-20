---
title: Inizializza l'SDK  [!DNL Adobe Target] Node.js per registrare le richieste
description: Scopri come registrare le richieste nell’SDK di  [!DNL Adobe Target] Node.js.
feature: APIs/SDKs
exl-id: 5db3e301-47b3-4330-b185-c0c03f72e790
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '85'
ht-degree: 2%

---

# Logger (Node.js)

## Descrizione

Quando [viene inizializzato l&#39;SDK](initialize-sdk.md), l&#39;oggetto `options.logger` è un oggetto facoltativo. Tuttavia, per eseguire il debug in modo efficace quando si verifica un problema, è necessario fornire un oggetto `logger` durante l&#39;inizializzazione dell&#39;SDK.

Per l&#39;oggetto `logger` è previsto un metodo `debug()` e `error()`. Se viene fornito un logger appropriato, ad esempio `console`, verranno registrate [!DNL Target] richieste e risposte.

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
