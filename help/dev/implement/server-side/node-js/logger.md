---
title: Inizializza il SDK  [!DNL Adobe Target] Node.js per registrare le richieste
description: Scopri come registrare le richieste nel SDK  [!DNL Adobe Target] Node.js.
feature: APIs/SDKs
exl-id: 5db3e301-47b3-4330-b185-c0c03f72e790
TQID: https://experienceleague.adobe.com/tC6xT-eAHOO17h1BK-PwWTBmwg3Dy0Wj8KYrV3W-VR4
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 83
ht-degree: 2%

---

# Logger (Node.js)

## Descrizione

Quando [viene inizializzato SDK](initialize-sdk.md), l&#39;oggetto `options.logger` è un oggetto facoltativo. Tuttavia, per eseguire il debug in modo efficace quando si verifica un problema, è necessario fornire un oggetto `logger` durante l&#39;inizializzazione di SDK.

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
