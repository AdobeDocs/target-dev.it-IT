---
title: Sincronizzazione dei profili in tempo reale per mbox3rdPartyId
description: Scopri come utilizzare mbox3rdPartyId con [!DNL Adobe Experience Platform Web SDK].
keywords: personalizzazione;target;adobe target;renderDecisions;sendEvent;mbox3rdPartyId;
feature: AEP Web SDK
exl-id: 1c5067ef-38b3-4bf1-bd39-ea0f2cbd1074
TQID: 'https://experienceleague.adobe.com/Ej2sYVnBD9orRTlsMQG85JJV7dvn-9gnABDa0b8uBlM'
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
    internal-label: Target
feature_v2:
  - id: adee20bd-51f4-461d-b9db-d215f8756eeb
    internal-label: Audiences
  - id: b050e0cd-2ddd-42cd-a71b-5d9e1fdf75e0
    internal-label: APIs and SDKs
subfeature_v2:
  - id: a1f3c920-a3a8-4506-8067-53189547b5e6
    internal-label: AEP Web SDK
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
topic_v2:
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
    internal-label: Personalization
source-git-commit: 5d119ccf18b09b3ba864a69642458597f65c754f
workflow-type: tm+mt
source-wordcount: '181'
ht-degree: 32%
---
# Usa mbox3rdPartyId

`mbox3rdPartyId` in [!DNL Adobe Target] è l’ID visitatore della tua azienda, ad esempio l’D di iscrizione al programma fedeltà.

Quando si accede al sito di una società, l’azienda generalmente crea un ID associato all’account del visitatore, alla carta fedeltà, al numero di iscrizione o ad altri identificatori validi per tale società. [Ulteriori informazioni](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/3rd-party-id.html)

## Come utilizzare `mbox3rdPartyId` con [!DNL Platform Web SDK]

### Passaggio 1: configurare `Target Third Party ID Namespace`

Configura `Target Third Party ID Namespace` nel [stream di dati](https://experienceleague.adobe.com/en/docs/experience-platform/datastreams/overview), utilizzando l&#39;ID dello spazio dei nomi che desideri utilizzare come ID di terze parti mbox. [Ulteriori informazioni sugli spazi dei nomi ID](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html)

![Interfaccia utente di Experience Platform con il campo spazio dei nomi dell&#39;ID di terze parti di Target.](/help/dev/implement/client-side/aep-web-sdk/assets/mbox3rdpartyid.png)

### Passaggio 2: invia `mbox3rdpartyId` a [!DNL Target]

Invia `mbox3rdpartyId` a [!DNL Target] nel comando `sendEvent`, utilizzando lo spazio dei nomi ID configurato nel passaggio 1.
[Ulteriori informazioni sull&#39;invio degli ID](/help/dev/implement/client-side/aep-web-sdk/using-mbox-3rdpartyid.md)

```javascript
alloy("sendEvent", {
  xdm: {
    "identityMap": {
      "ID_NAMESPACE": [ // Replace `ID_NAMESPACE` with the namespace you have configured in Step 1.
        {
          "id": "1234",
          "authenticatedState": "authenticated"
        }
      ]
    }
  }
});
```
