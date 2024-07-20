---
title: Iscriviti agli eventi nell'SDK Java  [!DNL Adobe Target]
description: Scopri come effettuare la sottoscrizione a vari eventi che si verificano nell’SDK Java utilizzando l’oggetto [!UICONTROL OnDeviceDecisioningHandler].
feature: APIs/SDKs
exl-id: f2d56762-6bf7-4c6b-9c14-fb20e5cfd60d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 5%

---

# Eventi SDK (Java)

## Descrizione

Quando [si inizializza l&#39;SDK](initialize-sdk.md), è possibile fornire un oggetto `OnDeviceDecisioningHandler` facoltativo sull&#39;oggetto `ClientConfig`. Può essere utilizzato per abbonarsi a vari eventi che si verificano all&#39;interno dell&#39;SDK. Ad esempio, l&#39;evento `onDeviceDecisioningReady` può essere utilizzato con una funzione di callback che verrà richiamata quando l&#39;SDK sarà pronto per le chiamate ai metodi.

## Eventi

L&#39;oggetto `OnDeviceDecisioningHandler` contiene i seguenti callback, chiamati per determinati eventi:

| Nome | Argomenti | Descrizione |
| --- | --- | --- |
| onDeviceDecisioningReady | None (Nessuno) | Chiamata eseguita solo una volta la prima volta che il client è pronto per [!UICONTROL on-device decisioning] |
| artifactDownloadSucceeded | byte[] contenuto del file di artefatto | Chiamata eseguita ogni volta che viene scaricato un artefatto [!UICONTROL on-device decisioning] |
| artifactDownloadFailed | Eccezione | Chiamata eseguita ogni volta che non è possibile scaricare un artefatto [!UICONTROL on-device decisioning] |

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
