---
title: Sincronizzazione dei profili in tempo reale per mbox3rdPartyId
description: Scopri come utilizzare mbox3rdPartyId con  [!DNL Adobe Experience Platform Web SDK].
keywords: personalizzazione;target;adobe target;renderDecisions;sendEvent;mbox3rdPartyId;
feature: AEP Web SDK
source-git-commit: 27759841c8d6a4ac4cc72e735f1dc6512c71aea5
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 16%

---

# Cos’è mbox3rdPartyId

`mbox3rdPartyId` in [!DNL Adobe Target] è l’ID visitatore della tua azienda, ad esempio l’D di iscrizione al programma fedeltà.

Quando un visitatore accede al sito di un’azienda, l’azienda in genere crea un ID associato all’account del visitatore, alla carta fedeltà, al numero di iscrizione o ad altri identificatori applicabili per l’azienda. [Ulteriori informazioni](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/3rd-party-id.html#)

## Come utilizzare `mbox3rdPartyId` con [!DNL Platform Web SDK]

### Passaggio 1: configurare `Target Third Party ID Namespace`

Configura `Target Third Party ID Namespace` nel [stream di dati](https://experienceleague.adobe.com/en/docs/experience-platform/datastreams/overview), utilizzando l&#39;ID dello spazio dei nomi che desideri utilizzare come ID di terze parti mbox. [Ulteriori informazioni sugli spazi dei nomi ID](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html)

![Interfaccia utente di Experience Platform con il campo spazio dei nomi dell&#39;ID di terze parti di Target.](/help/dev/implement/client-side/aep-web-sdk/assets/mbox3rdpartyid.png)

### Passaggio 2: invia `mbox3rdpartyId` a [!DNL Target]

Invia `mbox3rdpartyId` a [!DNL Target] nel comando `sendEvent`, utilizzando lo spazio dei nomi ID configurato nel passaggio 1.
[Ulteriori informazioni sull&#39;invio degli ID](../../identity/overview.md#syncing-identities)

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
