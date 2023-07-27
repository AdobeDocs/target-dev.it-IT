---
title: Invia notifiche di visualizzazione o clic a [!DNL Adobe Target] utilizzo di .NET SDK
description: Scopri come utilizzare sendNotifications() per inviare notifiche di visualizzazione o di clic a [!DNL Adobe Target] per la misurazione e il reporting.
feature: APIs/SDKs
exl-id: 724e787c-af53-4152-8b20-136f7b5452e1
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 1%

---

# Invia notifiche (.NET)

## Descrizione

`SendNotifications()` viene utilizzato per inviare notifiche di visualizzazione o clic a [!DNL Adobe Target] per la misurazione e il reporting.

>[!NOTE]
>
>Quando un `Execute` L&#39;oggetto con i parametri richiesti si trova all&#39;interno della richiesta stessa. L&#39;impression verrà incrementata automaticamente per le attività qualificate.

I metodi SDK che incrementano automaticamente un’impression sono:

* `GetOffers()`
* `GetAttributes()`

Quando un `Prefetch` viene passato all&#39;interno della richiesta, l&#39;impression non viene incrementata automaticamente per le attività con mbox all&#39;interno di `Prefetch` oggetto. `SendNotifications()` deve essere utilizzato per le esperienze preacquisite per incrementare impressioni e conversioni.

## Metodo

### Creare

```dotnet {line-numbers="true"}
TargetDeliveryResponse TargetClient.SendNotifications(TargetDeliveryRequest request)
```

## Esempio

Innanzitutto, creiamo il [!UICONTROL API di consegna di Target] richiesta di preacquisizione del contenuto per `home` e `product1` mbox.

### \.NET

```dotnet {line-numbers="true"}
var mboxRequests = new List<MboxRequest>
    {
        new (index: 1, name: "home"),
        new (index: 2, name: "product1"),
    };

var targetDeliveryRequest = new TargetDeliveryRequest.Builder()
    .SetPrefetch(new PrefetchRequest(mboxes: mboxRequests))
    .Build();

// Next, we fetch the offers via Target .NET SDK GetOffers() API
var targetResponse = targetClient.GetOffers(targetDeliveryRequest);
```

Una risposta corretta conterrà [!DNL Target Delivery API] oggetto response, che contiene contenuto preacquisito per le mbox richieste. Un esempio `targetResponse.Response` L&#39;oggetto può essere visualizzato come segue:

### \.NET

```dotnet {line-numbers="true"}
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

Osserva `mbox` nome e `state` campi, nonché `eventToken` in ogni campo [!DNL Target] opzioni di contenuto. Questi devono essere forniti nella sezione `SendNotifications()` non appena viene visualizzata ogni opzione di contenuto. Supponiamo che `product1` mbox è stato visualizzato su un dispositivo non browser. La richiesta di notifica verrà visualizzata come segue:

### \.NET

```dotnet {line-numbers="true"}
var mboxNotifications = new List<Notification>
{
    new (id: "1", type: MetricType.Display, timestamp: DateTimeOffset.UtcNow.ToUnixTimeMilliseconds(),
        mbox: new NotificationMbox("product1", "J+W1Fq18hxliDDJonTPfV0S+mzxapAO3d14M43EsM9f12A6QaqL+E3XKkRFlmq9U"),
        tokens: new List<string> { "t0FRvoWosOqHmYL5G18QCZNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==" })
}; 

var mboxNotificationRequest = new TargetDeliveryRequest.Builder()
    .SetNotifications(mboxNotifications)
    .Build();
```

Nota che abbiamo incluso sia lo stato mbox che il token evento corrispondente al [!DNL Target] offerta consegnata nella risposta di preacquisizione. Dopo aver generato la richiesta di notifiche, possiamo inviarla a [!DNL Target] tramite `SendNotifications()` Metodo API:

### \.NET

```dotnet {line-numbers="true"}
var notificationResponse = targetClient.SendNotifications(mboxNotificationRequest);
```
