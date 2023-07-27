---
title: Come utilizzare le richieste asincrone in [!DNL Adobe Target] SDK di Node.js
description: Scopri come [!DNL Target] L’SDK di Node.js supporta le richieste asincrone, che possono ridurre a zero il tempo di destinazione effettivo.
feature: APIs/SDKs
exl-id: aa06f3ca-7d2a-4334-8092-730a8705dfb0
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '116'
ht-degree: 16%

---

# Ottieni attributi (Node.js)

## Descrizione

`[!UICONTROL getAttributes()]` viene utilizzato per recuperare esperienze di sperimentazione e personalizzate da [!DNL Target] ed estrarre i valori degli attributi.

## Metodo

### getAttributes

```js {line-numbers="true"}
TargetClient.getAttributes(mboxNames: Array, options: Object): Promise
```

## Parametri

| Nome | Tipo | Obbligatorio | impostazione predefinita |
| --- | --- | --- |--- |
| mboxNames | Array | Sì | None (Nessuno) |
| options | Oggetto | No | None (Nessuno) |

## Promessa

Il `Promise` restituito da `TargetClient.getAttributes()` risolve un oggetto con i seguenti metodi:

| Metodo | Tipo restituito | Descrizione |
| --- | --- | --- |
| getValue(mboxName, key) | Any | Restituisce il valore per il nome mbox e la chiave attributo specificati |
| asObject(mboxName) | Oggetto | Restituisce un oggetto json semplice con coppie chiave-valore |
| getResponse() | [Risposta getOffers](https://github.com/jasonwaters/target-nodejs-sdk#targetclientgetoffers) | Restituisce l’oggetto di risposta normalmente restituito da `getOffers` |

## Esempio

### Node.js

```js {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");
const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};

const targetClient = TargetClient.create(CONFIG);
const offerAttributes = await targetClient.getAttributes(["demo-engineering-flags"]);


//returns just the value of searchProviderId from the mbox offer
const searchProviderId = offerAttributes.getValue("demo-engineering-flags", "searchProviderId");

//returns a simple JSON object representing the mbox offer
const engineeringFlags = offerAttributes.asObject("demo-engineering-flags");

//  the value of engineeringFlags looks like this
//  {
//      "cdnHostname": "cdn.cloud.corp.net",
//      "searchProviderId": 143,
//      "hasLegacyAccess": false
//  }

const assetUrl = `http://${engineeringFlags.cdnHostname}/path/to/asset`;
```
