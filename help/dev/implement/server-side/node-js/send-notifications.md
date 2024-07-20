---
title: Invia notifiche di visualizzazione o clic a  [!DNL Adobe Target]  tramite l'SDK di Node.js
description: Scopri come utilizzare sendNotifications() per inviare notifiche di visualizzazione o clic a  [!DNL Adobe Target]  per la misurazione e il reporting.
feature: APIs/SDKs
exl-id: 84bb6a28-423c-457f-8772-8e3f70e06a6c
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 4%

---

# Inviare notifiche (Node.js)

## Descrizione

`[!UICONTROL sendNotifications()]` viene utilizzato per inviare notifiche di visualizzazione o clic a [!DNL Adobe Target] per la misurazione e il reporting.

>[!NOTE]
>
>Quando un oggetto `execute` con parametri obbligatori si trova all&#39;interno della richiesta stessa, l&#39;impression verrà incrementata automaticamente per le attività idonee.

I metodi SDK che incrementano automaticamente un’impression sono:

* `getOffers()`
* `getAttributes()`

Quando un oggetto `prefetch` viene passato all&#39;interno della richiesta, l&#39;impression non viene incrementata automaticamente per le attività con mbox all&#39;interno dell&#39;oggetto `prefetch`. `sendNotifications()` deve essere utilizzato per le esperienze preacquisite per incrementare impressioni e conversioni.

## Metodo

### creare

```js {line-numbers="true"}
TargetClient.sendNotifications(options: Object): Promise
```

### Parametri

`options` ha la seguente struttura:

| Nome | Tipo | Obbligatorio | Predefinito |
| --- | --- | --- | --- |
| richiesta | Oggetto | Sì | None (Nessuno) |

## Esempio

Innanzitutto, compiliamo la richiesta API di consegna Ddi Target per preacquisire il contenuto per le mbox `home` e `product1`.

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

In caso di esito positivo, la risposta conterrà un oggetto di risposta [!UICONTROL Target Delivery API] contenente contenuto prerecuperato per le mbox richieste. Un oggetto `targetResponse.response` di esempio potrebbe essere visualizzato come segue:

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

Prendere nota dei campi mbox `name` e `state`, nonché del campo `eventToken`, in ciascuna delle opzioni di contenuto [!DNL Target]. Questi devono essere forniti nella richiesta `sendNotifications()`, non appena viene visualizzata ogni opzione di contenuto. Supponiamo che la mbox `product1` sia stata visualizzata su un dispositivo non browser. La richiesta di notifica verrà visualizzata come segue:

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

Nella risposta di preacquisizione sono stati inclusi sia lo stato mbox che il token evento corrispondente all&#39;offerta [!DNL Target] distribuita. Dopo aver generato la richiesta di notifiche, è possibile inviarla a [!DNL Target] tramite il metodo API `sendNotifications()`:

### Node.js

```js {line-numbers="true"}
const notificationResponse = await targetClient.sendNotifications({ request: mboxNotificationRequest });
```
