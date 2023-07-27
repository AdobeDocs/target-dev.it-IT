---
title: Utilizzare [!UICONTROL getOffers()] in [!DNL Adobe Target] quando si utilizza l’SDK di Node.js
description: Scopri come utilizzare [!UICONTROL getOffers()] per eseguire una decisione e recuperare un’esperienza da [!DNL Adobe Target].
feature: APIs/SDKs
exl-id: 3c4125ea-68d4-405e-9b9a-5fa832743153
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 21%

---

# [!UICONTROL Ottieni offerte] (Node.js)

## Descrizione

`[!UICONTROL getOffers()]` viene utilizzato per eseguire una decisione e recuperare un’esperienza da [!DNL Adobe Target].


## Metodo

### getOffers

```js {line-numbers="true"}
TargetClient.getOffers(options: Object): Promise
```

## Parametri

Il `options` L&#39;oggetto ha la seguente struttura:

| Nome | Tipo | Obbligatorio | impostazione predefinita | Descrizione |
| --- |--- | --- | --- | --- |
| Richiesta | Oggetto | Sì | None (Nessuno) | Conforme al [[!DNL Target] API di consegna](/help/dev/implement/delivery-api/overview.md) richiesta |
| visitorCookie | Stringa | No | None (Nessuno) | Cookie ECID (VisitorId) |
| targetCookie | Stringa | No | None (Nessuno) | [!DNL Target] cookie |
| targetLocationHint | Stringa | No | None (Nessuno) | [!DNL Target] hint di posizione |
| consumerId | Stringa | No | None (Nessuno) | consumerIds per [!UICONTROL Analytics for Target] (A4T) unione |
| ID cliente | Array | No | None (Nessuno) | ID cliente in formato compatibile con VisitorId |
| sessionId | Stringa | No | None (Nessuno) | Utilizzato per il collegamento di più [!DNL Target] richieste |
| visitatore | Oggetto | No | nuovo VisitorId | Specifica un&#39;istanza VisitorId esterna |

## Promessa

`Promise` ha la seguente struttura:

| Nome | Tipo | Descrizione |
| --- | --- | --- |
| richiesta | Oggetto | [[!UICONTROL API di consegna di Target]](/help/dev/implement/delivery-api/overview.md) richiesta |
| risposta | Oggetto | [[!UICONTROL API di consegna di Target]](/help/dev/implement/delivery-api/overview.md) risposta |
| visitorState | Oggetto | Oggetto che deve essere passato all’API visitatore `getInstance()` |
| targetCookie | Oggetto | [!DNL Target] cookie |
| targetLocationHintCookie | Oggetto | [!DNL Target] cookie hint posizione |
| analyticsDetails | Array | Payload di Analytics, in caso di utilizzo di Analytics lato client |
| responseTokens | Array | Un elenco di [Token di risposta](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html?). |
| traccia | Array | Dati di trace aggregati per tutte le mbox/visualizzazioni di richiesta |
| status | Oggetto | Oggetto contenente lo stato della risposta. |
| decisioningMethod | Stringa | Determina il metodo decisionale da utilizzare ([su dispositivo](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/overview.md), lato server, ibrido) |

`targetCookie` e `targetLocationHintCookie` gli oggetti utilizzati per restituire i dati al browser hanno la seguente struttura:

| Nome | Tipo | Descrizione |
| --- | --- | --- |
| name | Stringa | Nome cookie |
| value | Any | Valore del cookie, il verrà convertito in stringa |
| maxAge | Numero | Il `maxAge` L&#39;opzione è utile per impostare le scadenze relative al tempo corrente in secondi |

Il `status` l’oggetto utilizzato per indicare lo stato della risposta target ha la seguente struttura:

| Nome | Tipo | Descrizione |
| --- | --- | --- |
| status | Numero | Codice di stato HTTP |
| message | Stringa | Un messaggio sulla risposta. Ad esempio, può indicare se la risposta è stata decisa [su dispositivo](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/overview.md) o lato server |
| remoteMboxes | Array | Quando il metodo di decisione è `on-device`, viene fornito un array di nomi mbox che non possono essere decisi completamente sul dispositivo. In altre parole, un [[!UICONTROL API di consegna di Target]](/help/dev/implement/delivery-api/overview.md) richiesta necessaria. |

## Esempio

### Node.js

```js {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");
const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};

const targetClient = TargetClient.create(CONFIG);

const request = {
    context: {channel: "web"},
    execute: {
        mboxes: [{
            name: "a1-serverside-ab",
            index: 1
        }]
}};

const response = await targetClient.getOffers({ request });
```
