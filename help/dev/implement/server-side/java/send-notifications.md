---
title: Invia notifiche di visualizzazione o clic a [!DNL Adobe Target] utilizzo dell’SDK Java
description: Scopri come utilizzare sendNotifications() per inviare notifiche di visualizzazione o di clic a [!DNL Adobe Target] per la misurazione e il reporting.
feature: APIs/SDKs
exl-id: 9231b480-f50f-40d1-ab06-0b9f2a2d79e3
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '233'
ht-degree: 2%

---

# Invio di notifiche (Java)

## Descrizione

`sendNotifications()` viene utilizzato per inviare notifiche di visualizzazione o clic a [!DNL Adobe Target] per la misurazione e il reporting.

>[!NOTE]
>
>Quando un `execute` L&#39;oggetto con i parametri richiesti si trova all&#39;interno della richiesta stessa. L&#39;impression verrà incrementata automaticamente per le attività qualificate.

I metodi SDK che incrementano automaticamente un’impression sono:

* `getOffers()`
* `getAttributes()`

Quando un `prefetch` viene passato all&#39;interno della richiesta, l&#39;impression non viene incrementata automaticamente per le attività con mbox all&#39;interno di `prefetch` oggetto. `sendNotifications()` deve essere utilizzato per le esperienze preacquisite per incrementare impressioni e conversioni.

## Metodo

### creare

```javascript {line-numbers="true"}
ResponseStatus TargetClient.sendNotifications(TargetDeliveryRequest request)
```

## Esempio

Innanzitutto, creiamo il [!DNL Target Delivery API] richiesta di preacquisizione del contenuto per `home` e `product1` mbox.

### Preacquisizione

```javascript {line-numbers="true"}
List<MboxRequest> mboxRequests = new ArrayList<>();
mboxRequests.add((MboxRequest) new MboxRequest().name("home").index(1));
mboxRequests.add((MboxRequest) new MboxRequest().name("product1").index(2));
PrefetchRequest prefetchMboxesRequest = new PrefetchRequest().setMboxes(mboxRequests)

// Next, we fetch the offers via Target Java SDK getOffers() API
TargetDeliveryResponse targetResponse = targetJavaClient.getOffers(targetDeliveryRequest);
```

Una risposta corretta conterrà [!UICONTROL API di consegna di Target] oggetto response, che contiene contenuto preacquisito per le mbox richieste. Un esempio `targetResponse.response` L&#39;oggetto può avere il seguente aspetto:

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

Osserva la mbox `name` e `state` campi, nonché `eventToken` in ogni campo [!DNL Target] opzioni di contenuto. Questi devono essere forniti nella sezione `sendNotifications()` non appena viene visualizzata ogni opzione di contenuto. Supponiamo che `product1` mbox è stato visualizzato su un dispositivo non browser. La richiesta di notifica avrà un aspetto simile al seguente:

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

Tieni presente che abbiamo incluso sia lo stato mbox che il token evento corrispondente al [!DNL Target] offerta consegnata nella risposta di preacquisizione. Dopo aver generato la richiesta di notifiche, possiamo inviarla a [!DNL Target] tramite `sendNotifications()` Metodo API:

### Risposta

```javascript {line-numbers="true"}
ResponseStatus notificationResponse = targetJavaClient.sendNotifications(mboxNotificationRequest);
```
