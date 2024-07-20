---
title: Sottoscrivi eventi nell'SDK  [!DNL Adobe Target] .NET
description: Scopri come effettuare la sottoscrizione a vari eventi che si verificano all'interno di .NET SDK utilizzando l'oggetto [!UICONTROL OnDeviceDecisioningHandler].
feature: APIs/SDKs
exl-id: 7578033f-3de5-4d13-9739-46ad1269ec5f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '120'
ht-degree: 5%

---

# Eventi SDK (.NET)

## Descrizione

Quando [viene inizializzato l&#39;SDK](initialize-sdk.md), è possibile fornire un delegato `OnDeviceDecisioningReady` facoltativo sull&#39;oggetto `TargetClientConfig`, che verrà richiamato quando l&#39;SDK sarà pronto per le chiamate di metodo su dispositivo. Sono inoltre disponibili altri due delegati per la gestione del download dell&#39;artefatto [!UICONTROL on-device decisioning].

## Eventi

Per alcuni eventi è possibile configurare i seguenti delegati:

| Nome | Argomenti | Descrizione |
| --- | --- | --- |
| OnDeviceDecisioningReady | None (Nessuno) | Chiamata eseguita solo una volta la prima volta che il client è pronto per [!UICONTROL on-device decisioning] |
| ArtefattoDownloadRiuscito | contenuto stringa del file di artefatto | Chiamata eseguita ogni volta che viene scaricato un artefatto [!UICONTROL on-device decisioning] |
| ArtifactDownloadFailed | Eccezione | Chiamata eseguita ogni volta che non è possibile scaricare un artefatto [!UICONTROL on-device decisioning] |

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
