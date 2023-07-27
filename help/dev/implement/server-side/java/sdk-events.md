---
title: Iscriviti agli eventi in [!DNL Adobe Target] SDK Java
description: Scopri come abbonarti a vari eventi che si verificano all’interno dell’SDK Java utilizzando [!UICONTROL OnDeviceDecisioningHandler] oggetto.
feature: APIs/SDKs
exl-id: f2d56762-6bf7-4c6b-9c14-fb20e5cfd60d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 4%

---

# Eventi SDK (Java)

## Descrizione

Quando [inizializzazione dell’SDK](initialize-sdk.md), un elemento facoltativo `OnDeviceDecisioningHandler` L&#39;oggetto può essere fornito sul `ClientConfig` oggetto. Può essere utilizzato per abbonarsi a vari eventi che si verificano all&#39;interno dell&#39;SDK. Ad esempio, `onDeviceDecisioningReady` L&#39;evento può essere utilizzato con una funzione di callback che verrà richiamata quando l&#39;SDK sarà pronto per le chiamate ai metodi.

## Eventi

Il `OnDeviceDecisioningHandler` L&#39;oggetto contiene i seguenti callback, chiamati per determinati eventi:

| Nome | Argomenti | Descrizione |
| --- | --- | --- |
| onDeviceDecisioningReady | None (Nessuno) | Chiamata eseguita una sola volta la prima volta che il client è pronto per [!UICONTROL decisioning sul dispositivo] |
| artifactDownloadSucceeded | byte[] contenuto del file artefatto | Chiamato ogni volta che un [!UICONTROL decisioning sul dispositivo] artefatto scaricato |
| artifactDownloadFailed | Eccezione | Chiamata eseguita ogni volta che si verifica un errore durante il download di un [!UICONTROL decisioning sul dispositivo] artefatto |

## Esempio

### Eventi SDK

```javascript {line-numbers="true"}
ClientConfig clientConfig = ClientConfig.builder()
        .client("acmeclient")
        .organizationId("1234567890@AdobeOrg")
        .defaultDecisioningMethod(DecisioningMethod.ON_DEVICE)
        .onDeviceDecisioningHandler(new OnDeviceDecisioningHandler() {
            @Override
            public void onDeviceDecisioningReady() {
                // make getOffers requests
                makeTargetRequests();
            }

            @Override
            public void artifactDownloadSucceeded(byte[] artifactData) {
                System.out.println("The artifact was successfully downloaded.");
            }

            @Override
            public void artifactDownloadFailed(TargetClientException e) {
                System.out.println("The artifact failed to download.");
            }
        }).build();

TargetClient targetJavaClient = TargetClient.create(clientConfig);


void makeTargetRequests() {
    List<MboxRequest> mboxRequests = new ArrayList<>();
    mboxRequests.add((MboxRequest) new MboxRequest().name("a1-serverside-ab").index(1));

    TargetDeliveryRequest targetDeliveryRequest = TargetDeliveryRequest.builder()
            .context(new Context().channel(ChannelType.WEB))
            .execute(new ExecuteRequest().setMboxes(mboxRequests))
            .build();

    TargetDeliveryResponse targetResponse = targetJavaClient.getOffers(targetDeliveryRequest);
}
```
