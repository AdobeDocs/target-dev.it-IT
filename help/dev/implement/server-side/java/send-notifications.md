---
title: Invia notifiche di visualizzazione o clic a  [!DNL Adobe Target]  tramite l'SDK Java
description: Scopri come utilizzare sendNotifications() per inviare notifiche di visualizzazione o clic a  [!DNL Adobe Target]  per la misurazione e il reporting.
feature: APIs/SDKs
exl-id: 9231b480-f50f-40d1-ab06-0b9f2a2d79e3
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 2%

---

# Invio di notifiche (Java)

## Descrizione

`sendNotifications()` viene utilizzato per inviare notifiche di visualizzazione o clic a [!DNL Adobe Target] per la misurazione e il reporting.

>[!NOTE]
>
>Quando un oggetto `execute` con parametri obbligatori si trova all&#39;interno della richiesta stessa, l&#39;impression verrà incrementata automaticamente per le attività idonee.

I metodi SDK che incrementano automaticamente un’impression sono:

* `getOffers()`
* `getAttributes()`

Quando un oggetto `prefetch` viene passato all&#39;interno della richiesta, l&#39;impression non viene incrementata automaticamente per le attività con mbox all&#39;interno dell&#39;oggetto `prefetch`. `sendNotifications()` deve essere utilizzato per le esperienze preacquisite per incrementare impressioni e conversioni.

## Metodo

### creare

```javascript {line-numbers="true"}
ResponseStatus TargetClient.sendNotifications(TargetDeliveryRequest request)
```

## Esempio

Innanzitutto, compiliamo la richiesta [!DNL Target Delivery API] per preacquisire il contenuto per le mbox `home` e `product1`.

### Preacquisizione

```javascript {line-numbers="true"}
List<MboxRequest> mboxRequests = new ArrayList<>();
mboxRequests.add((MboxRequest) new MboxRequest().name("home").index(1));
mboxRequests.add((MboxRequest) new MboxRequest().name("product1").index(2));
PrefetchRequest prefetchMboxesRequest = new PrefetchRequest().setMboxes(mboxRequests)

// Next, we fetch the offers via Target Java SDK getOffers() API
TargetDeliveryResponse targetResponse = targetJavaClient.getOffers(targetDeliveryRequest);
```

In caso di esito positivo, la risposta conterrà un oggetto di risposta [!UICONTROL Target Delivery API] contenente contenuto prerecuperato per le mbox richieste. Un oggetto `targetResponse.response` di esempio potrebbe avere il seguente aspetto:

### Risposta

```javascript {line-numbers="true"}
{
  "status": 200,
  "requestId": "e8ac2dbf5f7d4a9f9280f6071f24a01e",
  "id": {
    "tntId": "08210e2d751a44779b8313e2d2692b96.21_27"
  },
  "client": "adobetargetmobile",
  "edgeHost": "mboxedge21.tt.omtrdc.net",
  "prefetch": {
    "mboxes": [
      {
        "index": 0,
        "name": "home",
        "options": [
          {
            "type": "html",
            "content": "HOME OFFER",
            "eventToken": "t0FRvoWosOqHmYL5G18QCZNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==",
            "responseTokens": {
              "profile.memberlevel": "0",
              "geo.city": "dublin",
              "activity.id": "302740",
              "experience.name": "Experience B",
              "geo.country": "ireland"
            }
          }
        ],
        "state": "J+W1Fq18hxliDDJonTPfV0S+mzxapAO3d14M43EsM9f12A6QaqL+E3XKkRFlmq9U"
      },
      {
        "index": 1,
        "name": "product1",
        "options": [
          {
            "type": "html",
            "content": "TEST OFFER 1",
            "eventToken": "t0FRvoWosOqHmYL5G18QCZNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==",
            "responseTokens": {
              "profile.memberlevel": "0",
              "geo.city": "dublin",
              "activity.id": "302740",
              "experience.name": "Experience B",
              "geo.country": "ireland"
            }
          }
        ],
        "state": "J+W1Fq18hxliDDJonTPfV0S+mzxapAO3d14M43EsM9f12A6QaqL+E3XKkRFlmq9U"
      }
    ]
  }
}
```

Prendere nota dei campi mbox `name` e `state`, nonché del campo `eventToken`, in ciascuna delle opzioni di contenuto [!DNL Target]. Questi devono essere forniti nella richiesta `sendNotifications()`, non appena viene visualizzata ogni opzione di contenuto. Supponiamo che la mbox `product1` sia stata visualizzata su un dispositivo non browser. La richiesta di notifica avrà un aspetto simile al seguente:

### Richiesta

```javascript {line-numbers="true"}
TargetDeliveryRequest mboxNotificationRequest = TargetDeliveryRequest.builder().notifications(new ArrayList() {{
    add(new Notification()
            .id("1")
            .timestamp(System.currentTimeMillis())
            .type(MetricType.DISPLAY)
            .mbox(new NotificationMbox()
                    .name("product1")
                    .state("J+W1Fq18hxliDDJonTPfV0S+mzxapAO3d14M43EsM9f12A6QaqL+E3XKkRFlmq9U")
            )
            .tokens(Arrays.asList(new String[]{"t0FRvoWosOqHmYL5G18QCZNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="}))
    );
}}).build();
```

Nella risposta di preacquisizione sono stati inclusi sia lo stato mbox che il token evento corrispondente all&#39;offerta [!DNL Target] distribuita. Dopo aver generato la richiesta di notifiche, è possibile inviarla a [!DNL Target] tramite il metodo API `sendNotifications()`:

### Risposta

```javascript {line-numbers="true"}
ResponseStatus notificationResponse = targetJavaClient.sendNotifications(mboxNotificationRequest);
```
