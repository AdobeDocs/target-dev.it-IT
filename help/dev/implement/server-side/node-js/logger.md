---
title: Inizializza il SDK Node.js [!DNL Adobe Target] per registrare le richieste
description: Scopri come registrare le richieste nel SDK Node.js di [!DNL Adobe Target].
feature: APIs/SDKs
exl-id: 5db3e301-47b3-4330-b185-c0c03f72e790
TQID: 'https://experienceleague.adobe.com/tC6xT-eAHOO17h1BK-PwWTBmwg3Dy0Wj8KYrV3W-VR4'
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
    internal-label: Target
feature_v2:
  - id: a19e8738-9679-599a-b83b-5f2f15f8e4d6
    internal-label: APIs/SDKs
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
source-git-commit: 5d119ccf18b09b3ba864a69642458597f65c754f
workflow-type: tm+mt
source-wordcount: '85'
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
