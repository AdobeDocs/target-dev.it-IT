---
title: Sottoscrivi eventi in [!DNL Adobe Target] .NET SDK
description: Scopri come sottoscrivere vari eventi che si verificano in .NET SDK utilizzando l'oggetto [!UICONTROL OnDeviceDecisioningHandler].
feature: APIs/SDKs
exl-id: 7578033f-3de5-4d13-9739-46ad1269ec5f
TQID: 'https://experienceleague.adobe.com/oeGknU-pW1-XjVrxn8JNEPoFBF8Gntt-vaVnqjdyTC8'
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
source-wordcount: '134'
ht-degree: 5%
---
# Eventi SDK (.NET)

## Descrizione

Quando [viene inizializzato SDK](initialize-sdk.md), è possibile fornire un delegato `OnDeviceDecisioningReady` facoltativo sull&#39;oggetto `TargetClientConfig`, che verrà richiamato quando SDK sarà pronto per le chiamate di metodo su dispositivo. Sono inoltre disponibili altri due delegati per la gestione del download dell&#39;artefatto [!UICONTROL decisioning sul dispositivo].

## Eventi

Per alcuni eventi è possibile configurare i seguenti delegati:

| Nome | Argomenti | Descrizione |
| --- | --- | --- |
| OnDeviceDecisioningReady | None (Nessuno) | Chiamata eseguita solo una volta la prima volta che il client è pronto per [!UICONTROL le decisioni sul dispositivo] |
| ArtefattoDownloadRiuscito | contenuto stringa del file di artefatto | Chiamata eseguita ogni volta che viene scaricato un artefatto di [!UICONTROL decisioning sul dispositivo] |
| ArtifactDownloadFailed | Eccezione | Chiamata eseguita ogni volta che non è stato possibile scaricare un artefatto [!UICONTROL decisioning sul dispositivo] |

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
