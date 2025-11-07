---
title: Iscriviti agli eventi nel SDK  [!DNL Adobe Target] Node.js
description: Scopri come effettuare la sottoscrizione a vari eventi che si verificano nel SDK di Node.js utilizzando l’oggetto [!UICONTROL OnDeviceDecisioningHandler].
feature: APIs/SDKs
exl-id: 40c53840-a560-4819-ae04-f527c36b22fe
source-git-commit: 67cc93cf697f8d5bca6fedb3ae974e4012347a0b
workflow-type: tm+mt
source-wordcount: '163'
ht-degree: 2%

---

# Eventi SDK (Node.js)

## Descrizione

Quando [viene inizializzato SDK](initialize-sdk.md), l&#39;oggetto `options.events` è un oggetto facoltativo con chiavi di nome evento e valori della funzione di callback. Può essere utilizzato per abbonarsi a vari eventi che si verificano all’interno di SDK. Ad esempio, è possibile utilizzare l&#39;evento `clientReady` con una funzione di callback che verrà richiamata quando SDK sarà pronto per le chiamate ai metodi.

Quando viene chiamata la funzione di callback, viene trasmesso un oggetto evento. Ogni evento ha un `type` corrispondente al nome dell&#39;evento. Alcuni eventi includono proprietà aggiuntive con informazioni pertinenti.

## Eventi

| Nome evento (tipo) | Descrizione | Proprietà evento aggiuntive |
| --- | --- | --- |
| clientReady | Emesso quando l&#39;artefatto è stato scaricato e SDK è pronto per `getOffers` chiamate. Consigliato quando si utilizza il metodo di decisione sul dispositivo. |  |
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
