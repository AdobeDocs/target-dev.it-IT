---
title: Notifiche API di consegna di Adobe Target
description: Come si attivano le notifiche utilizzando [!UICONTROL Adobe Target Delivery API]?
keywords: api di consegna
exl-id: 711388fd-2c1f-4ca4-939f-c56dc4bdc04a
feature: APIs/SDKs
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 0%

---

# Notifiche

Le notifiche devono essere attivate quando una mbox o una visualizzazione preacquisite viene visitata o sottoposta a rendering all’utente finale.

Affinché le notifiche vengano inviate per la mbox o la visualizzazione corretta, assicurati di tenere traccia di `eventToken` corrispondente per ogni mbox o visualizzazione. Le notifiche con `eventToken` corretto per le mbox o le visualizzazioni corrispondenti devono essere attivate affinché il reporting venga rispecchiato correttamente.

## Notifiche per mbox preacquisite

Una o più notifiche possono essere inviate tramite una singola chiamata di consegna. Determina se la metrica da tracciare è un `click` o un `display` per ogni mbox in modo che la `type` della notifica possa essere riflessa correttamente. Inoltre, passa un `id` per ogni notifica in modo che si possa determinare se una notifica è stata inviata correttamente tramite [!UICONTROL  Adobe Target Delivery API]. `timestamp` è inoltre importante da inoltrare a [!DNL Target] per indicare quando si è verificato `click` o `display` per una data mbox a scopo di reporting.

```
curl -X POST \
'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=10abf6304b2714215b1fd39a870f01afc#1555632114' \
-H 'Content-Type: application/json' \
-H 'cache-control: no-cache' \
-d '{
    "id": {
      "tntId": "abcdefghijkl00023.1_1"
    },
    "context": {
      "channel": "web",
      "browser" : {
        "host" : "demo"
      },
      "address" : {
        "url" : "http://demo.dev.tt-demo.com/demo/store/index.html"
      },
      "screen" : {
        "width" : 1200,
        "height": 1400
      }
    },
      "notifications": [
      {
      "id" : "SummerOfferNotification",
        "timestamp" : 1555705311051,
        "type" : "display",
        "mbox" : {
          "name" :"SummerOffer"   
        },
        "tokens" : [
          "GcvBXDhdJFNR9E9r1tgjfmqipfsIHvVzTQxHolz2IpSCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q"
        ]
      },
    {
      "id" : "SummerShoesOfferNotification",
        "timestamp" : 1555705311051,
        "type" : "display",
        "mbox" : {
          "name" :"SummerShoesOffer"   
        },
        "tokens" : [
          "GcvBXDhdJFNR9E9r1tgjfmqipfsIHvVzTQxHolz2IpSCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q"
        ]
      },
    {
      "id" : "SummerDressOfferNotification",
        "timestamp" : 1555705311051,
        "type" : "display",
        "mbox" : {
          "name" :"SummerDressOffer"   
        },
        "tokens" : [
          "GcvBXDhdJFNR9E9r1tgjfmqipfsIHvVzTQxHolz2IpSCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q"
        ]
    } 
    ]
  }'
```

La chiamata di esempio precedente darà luogo a una risposta che indica che la richiesta `notifications` è stata elaborata correttamente.

```
{
  "status": 200,
  "requestId": "36014eed-4772-4c48-a9e2-e532762b6a85",
  "client": "demo",
  "id": {
      "tntId": "abcdefghijkl00023.28_20"
  },
  "edgeHost": "mboxedge28.tt.omtrdc.net",
  "notifications": [
      {
          "id": "SummerOfferNotification"
      },
      {
          "id": "SummerDressOfferNotification"
      },
      {
          "id": "SummerShoesOfferNotification"
      }
  ]
}
```

Se tutti i `notifications` inviati a [!DNL Target] sono elaborati correttamente, verranno visualizzati nell&#39;array `notifications` nella risposta. Tuttavia, se manca un `notifications` `id`, quel particolare `notification` non è passato attraverso. In questo scenario, è possibile implementare una logica di nuovo tentativo fino al recupero di una risposta `notification` completata. Assicurati che nella logica di esecuzione di un nuovo tentativo sia specificato un timeout in modo che la chiamata API non si blocchi e non provochi ritardi nelle prestazioni.

## Notifiche per le visualizzazioni preacquisite

Una o più notifiche possono essere inviate tramite una singola chiamata di consegna. Determina se la metrica da tracciare è un `click` o `display` per ogni mbox in modo che il tipo di notifica possa essere riflesso correttamente. Inoltre, passare un `id` per ogni notifica in modo che si possa determinare se una notifica è stata inviata correttamente tramite [!UICONTROL Adobe Target Delivery API]. La marca temporale è importante anche da inoltrare a [!DNL Target] per indicare quando si è verificato `click` o `display` per una determinata visualizzazione a scopo di reporting.

```
curl -X POST \
  'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=d359570e04f14e1faeeba02d6ab9914e' \
  -H 'Content-Type: application/json' \
  -H 'cache-control: no-cache' \
  -d '{
  "id": {
    "tntId": "84e8d0e211054f18af365d65f45e902b.28_131"
  },
  "context": {
    "channel": "web",
    "browser": {
      "host": "target.enablementadobe.com"
    },
    "address": {
      "url": "https://target.enablementadobe.com/react/demo/#/"
    }
  },
  "notifications": [{
      "id": "228",
      "type": "display",
      "timestamp": 1556226121884,
      "tokens": ["N3C13I0M2PH8iaKtONJlFJNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="],
      "view": {
        "name": "checkout-express",
      }
    },
    {
      "id": "5",
      "type": "display",
      "timestamp": 1556226121884,
      "tokens": ["N3C13I0M2PH8iaKtONJlFJNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="],
      "view": {
        "name": "home",
      }
    },
    {
      "id": "6",
      "type": "display",
      "timestamp": 1556226121884,
      "tokens": ["N3C13I0M2PH8iaKtONJlFJNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="],
      "view": {
        "name": "products",
      }
    }
  ]
}'
```

La chiamata di esempio precedente darà luogo a una risposta che indica che la richiesta `notifications` è stata elaborata correttamente.

```
{
    "status": 200,
    "requestId": "85cc7394-c19a-4398-9b8b-bbee1e4c4579",
    "client": "demo",
    "id": {
        "tntId": "84e8d0e211054f18af365d65f45e902b.28_131"
    },
    "edgeHost": "mboxedge28.tt.omtrdc.net",
    "notifications": [
        {
            "id": "5"
        },
        {
            "id": "6"
        },
        {
            "id": "228"
        }
    ]
}
```

Se tutti i `notifications` inviati a [!DNL Target] sono elaborati correttamente, verranno visualizzati nell&#39;array `notifications` nella risposta. Tuttavia, se manca un `notifications` `id`, quella particolare notifica non è stata eseguita. In questo scenario, è possibile implementare una logica di nuovo tentativo fino a quando non viene recuperata una risposta di notifica corretta. Assicurati che nella logica di esecuzione di un nuovo tentativo sia specificato un timeout in modo che la chiamata API non si blocchi e non provochi ritardi nelle prestazioni.
