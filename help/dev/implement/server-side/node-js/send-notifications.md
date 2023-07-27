---
title: Invia notifiche di visualizzazione o clic a [!DNL Adobe Target] utilizzo dell’SDK di Node.js
description: Scopri come utilizzare sendNotifications() per inviare notifiche di visualizzazione o di clic a [!DNL Adobe Target] per la misurazione e il reporting.
feature: APIs/SDKs
exl-id: 84bb6a28-423c-457f-8772-8e3f70e06a6c
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 4%

---

# Inviare notifiche (Node.js)

## Descrizione

`[!UICONTROL sendNotifications()]` viene utilizzato per inviare notifiche di visualizzazione o clic a [!DNL Adobe Target] per la misurazione e il reporting.

>[!NOTE]
>
>Quando un `execute` L&#39;oggetto con i parametri richiesti si trova all&#39;interno della richiesta stessa. L&#39;impression verrà incrementata automaticamente per le attività qualificate.

I metodi SDK che incrementano automaticamente un’impression sono:

* `getOffers()`
* `getAttributes()`

Quando un `prefetch` viene passato all&#39;interno della richiesta, l&#39;impression non viene incrementata automaticamente per le attività con mbox all&#39;interno di `prefetch` oggetto. `sendNotifications()` deve essere utilizzato per le esperienze preacquisite per incrementare impressioni e conversioni.

## Metodo

### creare

```js {line-numbers="true"}
TargetClient.sendNotifications(options: Object): Promise
```

### Parametri

`options` ha la seguente struttura:

| Nome | Tipo | Obbligatorio | impostazione predefinita |
| --- | --- | --- | --- |
| richiesta | Oggetto | Sì | None (Nessuno) |

## Esempio

Innanzitutto, creiamo il Target DRichiesta API di consegna per preacquisire il contenuto per `home` e `product1` mbox.

### Node.js

```js {line-numbers="true"}
const prefetchMboxesRequest = {
  prefetch: {
    mboxes: [
      { name: "home" },
      { name: "product1" }
    ]
  }
};
// Next, we fetch the offers via Target Node.js SDK getOffers() API
const targetResponse = await targetClient.getOffers({ request: prefetchMboxesRequest });
```

Una risposta corretta conterrà [!UICONTROL API di consegna di Target] oggetto response, che contiene contenuto preacquisito per le mbox richieste. Un esempio `targetResponse.response` L&#39;oggetto può essere visualizzato come segue:

### Node.js

```js {line-numbers="true"}
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

Osserva la mbox `name` e `state` campi, nonché `eventToken` in ogni campo [!DNL Target] opzioni di contenuto. Questi devono essere forniti nella sezione `sendNotifications()` non appena viene visualizzata ogni opzione di contenuto. Supponiamo che `product1` mbox è stato visualizzato su un dispositivo non browser. La richiesta di notifica verrà visualizzata come segue:

### Node.js

```js {line-numbers="true"}
const mboxNotificationRequest = {
  notifications: [{
    id: "1",
    timestamp: Date.now(),
    type: "display",
    mbox: {
      name: "product1",
      state: "J+W1Fq18hxliDDJonTPfV0S+mzxapAO3d14M43EsM9f12A6QaqL+E3XKkRFlmq9U"
    },
    tokens: [ "t0FRvoWosOqHmYL5G18QCZNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==" ]
  }]
};
```

Tieni presente che abbiamo incluso sia lo stato mbox che il token evento corrispondente al [!DNL Target] offerta consegnata nella risposta di preacquisizione. Dopo aver generato la richiesta di notifiche, possiamo inviarla a [!DNL Target] tramite `sendNotifications()` Metodo API:

### Node.js

```js {line-numbers="true"}
const notificationResponse = await targetClient.sendNotifications({ request: mboxNotificationRequest });
```
