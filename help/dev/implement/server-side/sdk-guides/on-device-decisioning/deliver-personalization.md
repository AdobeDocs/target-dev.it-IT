---
title: Distribuire personalizzazioni tramite SDK di Adobe Target
description: Scopri come distribuire la personalizzazione utilizzando [!UICONTROL decisioning sul dispositivo].
feature: APIs/SDKs
exl-id: bac64c78-0d3a-40d7-ae2b-afa0f1b8dc4f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '559'
ht-degree: 0%

---

# Distribuire la personalizzazione

## Riepilogo dei passaggi

1. Abilita [!UICONTROL decisioning sul dispositivo] per la tua organizzazione
1. Creare un [!UICONTROL Targeting esperienza] Attività (XT)
1. Definire un’esperienza personalizzata per pubblico
1. Verificare l’esperienza personalizzata per pubblico
1. Configurare la generazione di rapporti
1. Aggiungere metriche per il tracciamento dei KPI
1. Implementare offerte personalizzate nella tua applicazione
1. Implementare il codice per tenere traccia degli eventi di conversione
1. Attiva [!UICONTROL Targeting esperienza] Attività di personalizzazione (XT)

Supponiamo di essere un&#39;azienda itinerante. Desideri offrire un’offerta personalizzata del 25% di sconto su alcuni pacchetti di viaggio. Affinché l’offerta possa risuonare con i tuoi utenti, decidi di mostrare un punto di riferimento della città di destinazione. Desideri inoltre garantire che la consegna delle offerte personalizzate venga eseguita con latenza vicina a zero, in modo che non influisca negativamente sulle esperienze utente e non distorca i risultati.

## 1. Abilita [!UICONTROL decisioning sul dispositivo] per la tua organizzazione

1. L’abilitazione del decisioning sul dispositivo garantisce che un’attività A/B venga eseguita con latenza vicina allo zero. Per abilitare questa funzione, vai a **[!UICONTROL Amministrazione]** > **[!UICONTROL Implementazione]** > **[!UICONTROL Dettagli account]** in [!DNL Adobe Target], e abilita **[!UICONTROL Decisioning sul dispositivo]** attivare/disattivare.

   ![immagine alt](assets/asset-odd-toggle.png)

   >[!NOTE]
   >
   >È necessario disporre dell&#39;amministratore o dell&#39;approvatore [ruolo utente](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/user-management.html) per abilitare o disabilitare [!UICONTROL Decisioning sul dispositivo] attivare/disattivare.

   Dopo aver abilitato **[!UICONTROL Decisioning sul dispositivo]** attivare/disattivare, [!DNL Adobe Target] inizia a generare *artefatti regola* per il tuo cliente.

## 2. Creare un [!UICONTROL Targeting esperienza] Attività (XT)

1. In entrata [!DNL Adobe Target], passare alla **[!UICONTROL Attività]** , quindi seleziona **[!UICONTROL Crea attività]** > **[!UICONTROL Targeting esperienza]**.

   ![immagine alt](assets/asset-xt.png)

1. In **[!UICONTROL Crea attività di targeting esperienza]** , lascia il valore predefinito **[!UICONTROL Web]** opzione selezionata (1), seleziona **[!UICONTROL Modulo]** in compositore esperienza (2), seleziona un’area di lavoro e una proprietà (3), quindi fai clic su **[!UICONTROL Successivo]** 4).

   ![immagine alt](assets/asset-xt-next.png)

## 3. Definire un’esperienza personalizzata per pubblico

1. In **[!UICONTROL Esperienze]** passaggio di creazione dell’attività, fai clic su **[!UICONTROL Cambia pubblico]** per creare un pubblico di quei visitatori che desiderano viaggiare a San Francisco, California.

   ![immagine alt](assets/asset-change-audience.png)

1. In **[!UICONTROL Crea pubblico]** , definisci una regola personalizzata in cui `destinationCity = San Francisco`. Questo definisce il gruppo di utenti che desiderano viaggiare a San Francisco.

   ![immagine alt](assets/asset-audience-sf.png)

1. Ancora in **[!UICONTROL Esperienze]** passaggio, inserisci il nome della posizione (1) all’interno dell’applicazione in cui desideri eseguire il rendering di un’offerta speciale relativa al Golden Gate Bridge, ma solo per quelli diretti a San Francisco. Nell’esempio mostrato qui, homepage è la posizione selezionata per l’offerta HTML (2), definita nella sezione **[!UICONTROL Contenuto]** area.

   ![immagine alt](assets/asset-content-sf.png)

1. Aggiungere un altro pubblico di destinazione facendo clic su **[!UICONTROL Aggiungi targeting esperienza]**. Questa volta, rivolgiti a un pubblico che desidera viaggiare a New York definendo una regola per il pubblico in cui `destinationCity = New York`. Definisci la posizione all’interno dell’applicazione in cui desideri eseguire il rendering di un’offerta speciale relativa all’Empire State Building. Nell’esempio riportato di seguito, `homepage` è la posizione selezionata per l’offerta HTML (2), definita nella **[!UICONTROL Contenuto]** area.

   ![immagine alt](assets/asset-content-ny.png)

## 4. Verificare l’esperienza personalizzata per pubblico

In **[!UICONTROL Targeting]** fase, verifica di aver configurato l’esperienza personalizzata desiderata per pubblico.

![immagine alt](assets/asset-verify-sf-ny.png)

## 5. Impostare la generazione rapporti

In **[!UICONTROL Obiettivi e impostazioni]** passo, scegli **[!UICONTROL Adobe Target]** come **[!UICONTROL Origine per la generazione di rapporti]** per visualizzare i risultati dell’attività in [!DNL Adobe Target] UI o scegli **[!UICONTROL Adobe Analytics]** per visualizzarli nell’interfaccia utente di Adobe Analytics.

![immagine alt](assets/asset-reporting-sf-ny.png)

## 6. Aggiungere metriche per il tracciamento dei KPI

Scegli un **[!UICONTROL Metrica per obiettivo]** per misurare il successo dell’attività. In questo esempio, una conversione corretta dipende dal fatto che l’utente faccia clic sull’offerta di destinazione personalizzata.

## 7. Implementa le offerte personalizzate nella tua applicazione

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");

const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
  request: {      
    execute: {
      pageLoad: {
        parameters: {
          destinationCity: "San Francisco"
        }
      }
    }       
  }
})
.then(console.log)
.catch(console.error);
```

>[!TAB Java]

```java {line-numbers="true"}
ClientConfig config = ClientConfig.builder()
  .client("acmeclient")
  .organizationId("1234567890@AdobeOrg")
  .build();
TargetClient targetClient = TargetClient.create(config);

Context context = new Context().channel(ChannelType.WEB);

ExecuteRequest executeRequest = new ExecuteRequest();

RequestDetails pageLoad = new RequestDetails();
pageLoad.setParameters(
    new HashMap<String, String>() {
      {
        put("destinationCity", "San Francisco");
      }
    });

executeRequest.setPageLoad(pageLoad);

TargetDeliveryRequest request = TargetDeliveryRequest.builder()
  .context(context)
  .execute(executeRequest)
  .build();

TargetDeliveryResponse offers = targetClient.getOffers(request);
```

>[!ENDTABS]

## 8. Implementa il codice per tenere traccia degli eventi di conversione

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
//... Code removed for brevity

//When a conversion happens
TargetClient.sendNotifications({
    targetCookie,
    "request" : {
      "notifications" : [
        {
          type: "click",
          timestamp : Date.now(),
          id: "conversion",
          mbox : {
            name : "destinationOffer"
          }
        }
      ]
    }
})
```

>[!TAB Java]

```java {line-numbers="true"
ClientConfig config = ClientConfig.builder()
  .client("acmeclient")
  .organizationId("1234567890@AdobeOrg")
  .build();
TargetClient targetClient = TargetClient.create(config);

Context context = new Context().channel(ChannelType.WEB);

ExecuteRequest executeRequest = new ExecuteRequest();

RequestDetails pageLoad = new RequestDetails();
pageLoad.setParameters(
    new HashMap<String, String>() {
      {
        put("destinationCity", "San Francisco");
      }
    });

executeRequest.setPageLoad(pageLoad);
NotificationDeliveryService notificationDeliveryService = new NotificationDeliveryService();

Notification notification = new Notification();
notification.setId("conversion");
notification.setImpressionId(UUID.randomUUID().toString());
notification.setType(MetricType.CLICK);
notification.setTimestamp(System.currentTimeMillis());
notification.setTokens(
    Collections.singletonList(
        "IbG2Jz2xmHaqX7Ml/YRxRGqipfsIHvVzTQxHolz2IpSCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="));

TargetDeliveryRequest targetDeliveryRequest =
    TargetDeliveryRequest.builder()
        .context(context)
        .execute(executeRequest)
        .notifications(Collections.singletonList(notification))
        .build();

TargetDeliveryResponse offers = targetClient.getOffers(request);
notificationDeliveryService.sendNotification(request);
```

>[!ENDTABS]

## 9. Attiva l’attività Targeting esperienze (XT)

![immagine alt](assets/asset-xt-activate.png)
