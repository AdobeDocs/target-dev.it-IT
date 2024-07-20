---
title: Usa [!UICONTROL getOffers()] in [!DNL Adobe Target] con SDK di Node.js
description: Scopri come utilizzare [!UICONTROL getOffers()] per eseguire una decisione e recuperare un'esperienza da [!DNL Adobe Target].
feature: APIs/SDKs
exl-id: 3c4125ea-68d4-405e-9b9a-5fa832743153
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '313'
ht-degree: 21%

---

# [!UICONTROL Get Offers] (Node.js)

## Descrizione

`[!UICONTROL getOffers()]` viene utilizzato per eseguire una decisione e recuperare un&#39;esperienza da [!DNL Adobe Target].


## Metodo

### getOffers

```js {line-numbers="true"}
TargetClient.getOffers(options: Object): Promise
```

## Parametri

L&#39;oggetto `options` ha la seguente struttura:

| Nome | Tipo | Obbligatorio | Predefinito | Descrizione |
| --- |--- | --- | --- | --- |
| Richiesta | Oggetto | Sì | None (Nessuno) | Conforme alla richiesta [[!DNL Target] API di consegna](/help/dev/implement/delivery-api/overview.md) |
| visitorCookie | Stringa | No | None (Nessuno) | Cookie ECID (VisitorId) |
| targetCookie | Stringa | No | None (Nessuno) | Cookie [!DNL Target] |
| targetLocationHint | Stringa | No | None (Nessuno) | [!DNL Target] hint di posizione |
| consumerId | Stringa | No | None (Nessuno) | consumerIds per l&#39;unione di [!UICONTROL Analytics for Target] (A4T) |
| ID cliente | Array | No | None (Nessuno) | ID cliente in formato compatibile con VisitorId |
| sessionId | Stringa | No | None (Nessuno) | Utilizzato per collegare più richieste [!DNL Target] |
| visitatore | Oggetto | No | nuovo VisitorId | Specifica un&#39;istanza VisitorId esterna |

## Promessa

`Promise` restituito ha la seguente struttura:

| Nome | Tipo | Descrizione |
| --- | --- | --- |
| richiesta | Oggetto | [[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md) richiesta |
| risposta | Oggetto | [[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md) risposta |
| visitorState | Oggetto | Oggetto da passare all&#39;API visitatore `getInstance()` |
| targetCookie | Oggetto | Cookie [!DNL Target] |
| targetLocationHintCookie | Oggetto | Cookie dell&#39;hint di posizione [!DNL Target] |
| analyticsDetails | Array | Payload di Analytics, in caso di utilizzo di Analytics lato client |
| responseTokens | Array | Elenco di [token di risposta](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html?). |
| traccia | Array | Dati di trace aggregati per tutte le mbox/visualizzazioni di richiesta |
| status | Oggetto | Oggetto contenente lo stato della risposta. |
| decisioningMethod | Stringa | Determina il metodo decisionale da utilizzare ([sul dispositivo](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/overview.md), lato server, ibrido) |

`targetCookie` e `targetLocationHintCookie` oggetti utilizzati per restituire dati al browser hanno la seguente struttura:

| Nome | Tipo | Descrizione |
| --- | --- | --- |
| name | Stringa | Nome cookie |
| value | Any | Valore del cookie, il verrà convertito in stringa |
| maxAge | Numero | L&#39;opzione `maxAge` è una comodità per impostare le scadenze relative all&#39;ora corrente in secondi |

L&#39;oggetto `status` utilizzato per indicare lo stato della risposta di destinazione ha la seguente struttura:

| Nome | Tipo | Descrizione |
| --- | --- | --- |
| status | Numero | Codice di stato HTTP |
| message | Stringa | Un messaggio sulla risposta. Ad esempio, potrebbe indicare se la risposta è stata decisa [sul dispositivo](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/overview.md) o lato server |
| remoteMboxes | Array | Quando il metodo di decisione è `on-device`, viene fornito un array di nomi mbox che non è stato possibile decidere completamente sul dispositivo. In altre parole, è necessaria una richiesta [[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md). |

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
