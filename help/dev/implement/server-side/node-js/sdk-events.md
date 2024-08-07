---
title: Iscriviti agli eventi nell'SDK di  [!DNL Adobe Target] Node.js
description: Scopri come effettuare la sottoscrizione a vari eventi che si verificano nell’SDK di Node.js utilizzando l’oggetto [!UICONTROL OnDeviceDecisioningHandler].
feature: APIs/SDKs
exl-id: 40c53840-a560-4819-ae04-f527c36b22fe
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '163'
ht-degree: 2%

---

# Eventi SDK (Node.js)

## Descrizione

Quando [viene inizializzato l&#39;SDK](initialize-sdk.md), l&#39;oggetto `options.events` è un oggetto facoltativo con chiavi di nome evento e valori della funzione di callback. Può essere utilizzato per abbonarsi a vari eventi che si verificano all&#39;interno dell&#39;SDK. Ad esempio, l&#39;evento `clientReady` può essere utilizzato con una funzione di callback che verrà richiamata quando l&#39;SDK sarà pronto per le chiamate ai metodi.

Quando viene chiamata la funzione di callback, viene trasmesso un oggetto evento. Ogni evento ha un `type` corrispondente al nome dell&#39;evento. Alcuni eventi includono proprietà aggiuntive con informazioni pertinenti.

## Eventi

| Nome evento (tipo) | Descrizione | Proprietà evento aggiuntive |
| --- | --- | --- |
| clientReady | Emesso quando l&#39;artefatto è stato scaricato e l&#39;SDK è pronto per `getOffers` chiamate. Consigliato quando si utilizza il metodo di decisione sul dispositivo. |
| artifactDownloadSucceeded | Viene emesso ogni volta che viene scaricato un nuovo artefatto. | artifactPayload, artifactLocation |
| artifactDownloadFailed | Emesso ogni volta che un artefatto non viene scaricato. | artifactLocation, errore |

## Esempio

### Node.js

```js {line-numbers="true"}
const targetClient = TargetClient.create({
    client: "acmeclient",
    organizationId: "1234567890@AdobeOrg",
    decisioningMethod: "on-device",
    events: {
        clientReady: onTargetClientReady,
        artifactDownloadSucceeded: onArtifactDownloadSucceeded,
        artifactDownloadFailed: onArtifactDownloadFailed
    }
});

function onTargetClientReady() {
    // make getOffers requests
    targetClient.getOffers({...})            
}

function onArtifactDownloadSucceeded(event) {
    console.log(`The artifact was successfully downloaded from '${event.artifactLocation}'`);
    // optionally do something with event.artifactPayload, like persist it
}

function onArtifactDownloadFailed(event) {
    console.log(`The artifact failed to download from '${event.artifactLocation}' with the following error message: ${event.error.message}`);
}
```
