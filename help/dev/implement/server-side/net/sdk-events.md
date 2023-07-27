---
title: Iscriviti agli eventi in [!DNL Adobe Target] SDK .NET
description: Scopri come effettuare la sottoscrizione a vari eventi che si verificano all’interno di .NET SDK utilizzando [!UICONTROL OnDeviceDecisioningHandler] oggetto.
feature: APIs/SDKs
exl-id: 7578033f-3de5-4d13-9739-46ad1269ec5f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '129'
ht-degree: 5%

---

# Eventi SDK (.NET)

## Descrizione

Quando [inizializzazione dell’SDK](initialize-sdk.md), un elemento facoltativo `OnDeviceDecisioningReady` il delegato può essere fornito sulla `TargetClientConfig` , che verrà richiamato quando l&#39;SDK sarà pronto per le chiamate al metodo sul dispositivo. Ci sono anche un paio di altri delegati disponibili per gestire il [!UICONTROL decisioning sul dispositivo] download di artefatti.

## Eventi

Per alcuni eventi è possibile configurare i seguenti delegati:

| Nome | Argomenti | Descrizione |
| --- | --- | --- |
| OnDeviceDecisioningReady | None (Nessuno) | Chiamata eseguita una sola volta la prima volta che il client è pronto per [!UICONTROL decisioning sul dispositivo] |
| ArtefattoDownloadRiuscito | contenuto stringa del file di artefatto | Chiamato ogni volta che un [!UICONTROL decisioning sul dispositivo] artefatto scaricato |
| ArtifactDownloadFailed | Eccezione | Chiamata eseguita ogni volta che si verifica un errore durante il download di un [!UICONTROL decisioning sul dispositivo] artefatto |

## Esempio

### \.NET

```dotnet {line-numbers="true"}
var clientConfig = new TargetClientConfig.Builder("acmeclient", "1234567890@AdobeOrg")
    .SetDecisioningMethod(DecisioningMethod.OnDevice)
    .SetOnDeviceDecisioningReady(DecisioningReady)
    .SetArtifactDownloadSucceeded(artifact => Console.WriteLine("The artifact was successfully downloaded. Contents: " + artifact))
    .SetArtifactDownloadFailed(exception => Console.WriteLine("The artifact failed to download. Exception: " + exception.Message))
    .Build();

var targetClient = TargetClient.Create(clientConfig);

// ...

static void DecisioningReady()
{
    var mboxRequests = new List<MboxRequest> { new (index: 1, name: "a1-serverside-ab") };

    var targetDeliveryRequest = new TargetDeliveryRequest.Builder()
        .SetExecute(new ExecuteRequest(mboxes: mboxRequests))
        .Build();

    var targetResponse = targetClient.GetOffers(targetDeliveryRequest);
}
```
