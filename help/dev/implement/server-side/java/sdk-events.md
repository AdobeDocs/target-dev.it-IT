---
title: Iscriviti agli eventi in  [!DNL Adobe Target] Java SDK
description: Scopri come effettuare la sottoscrizione a vari eventi che si verificano all'interno di Java SDK utilizzando l'oggetto [!UICONTROL OnDeviceDecisioningHandler].
feature: APIs/SDKs
exl-id: f2d56762-6bf7-4c6b-9c14-fb20e5cfd60d
TQID: https://experienceleague.adobe.com/x3aig-jM-GXzmLNcUNclZUK9Y49tuSF9-sdkxzJFtiM
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 134
ht-degree: 5%

---

# Eventi SDK (Java)

## Descrizione

Quando [viene inizializzato SDK](initialize-sdk.md), è possibile fornire un oggetto `OnDeviceDecisioningHandler` facoltativo sull&#39;oggetto `ClientConfig`. Può essere utilizzato per abbonarsi a vari eventi che si verificano all’interno di SDK. Ad esempio, l&#39;evento `onDeviceDecisioningReady` può essere utilizzato con una funzione di callback che verrà richiamata quando SDK sarà pronto per le chiamate ai metodi.

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
