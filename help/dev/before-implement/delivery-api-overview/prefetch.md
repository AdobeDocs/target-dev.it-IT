---
title: Preacquisizione API di consegna Adobe Target
description: Come si utilizza la preacquisizione in [!UICONTROL API di consegna di Adobe Target]?
keywords: api di consegna
exl-id: eab88e3a-442c-440b-a83d-f4512fc73e75
feature: APIs/SDKs
source-git-commit: 803723d95d50cc39101d1646232446fbb0254385
workflow-type: tm+mt
source-wordcount: '556'
ht-degree: 0%

---

# Preacquisizione

La preacquisizione consente ai client come applicazioni e server mobili di recuperare contenuti per più mbox o visualizzazioni in una richiesta, memorizzarli nella cache locale e successivamente inviare una notifica [!DNL Target] quando l’utente visita queste mbox o visualizzazioni.

Quando si utilizza la preacquisizione, è importante avere familiarità con i seguenti termini:

| Nome campo | Descrizione |
| --- | --- |
| `prefetch` | Elenco di mbox e visualizzazioni che devono essere recuperate ma non contrassegnate come visitate. Il [!DNL Target] Edge restituisce un `eventToke`n per ogni mbox o vista esistente nella matrice di preacquisizione. |
| `notifications` | Elenco di mbox e visualizzazioni precedentemente preacquisite che devono essere contrassegnate come visitate. |
| `eventToken` | Token crittografato con hash restituito quando il contenuto viene prerecuperato. Questo token deve essere rimandato a [!DNL Target] nel `notifications` array. |

## Preacquisizione di Mbox

I client come le app e i server per dispositivi mobili possono preacquisire più mbox per un determinato utente all’interno di una sessione e memorizzarle nella cache, al fine di evitare chiamate multiple a [!UICONTROL API di consegna di Adobe Target].

```
curl -X POST \
'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=7abf6304b2714215b1fd39a870f01afc#1555632114' \
-H 'Content-Type: application/json' \
-H 'cache-control: no-cache' \
-d '
{
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
    "prefetch": {
    "mboxes" : [
      {
        "name" : "SummerOffer",
        "index" : 1
      },
      {
        "name" : "SummerShoesOffer",
        "index" : 2
      },
      {
        "name" : "SummerDressOffer",
        "index" : 3
      }      
    ]
  }
}'
```

All&#39;interno del `prefetch` , aggiungi uno o più `mboxes` desideri eseguire la preacquisizione di contemporaneamente per un utente all’interno di una sessione. Una volta preacquisiti per questi `mboxes` riceverai la seguente risposta:

```
{
    "status": 200,
    "requestId": "5efee0d8-3779-4b12-a74e-e04848faf191",
    "client": "demo",
    "id": {
        "tntId": "abcdefghijkl00023.1_1"
    },
    "edgeHost": "mboxedge28.tt.omtrdc.net",
    "prefetch": {
        "mboxes": [
            {
                "index": 1,
                "name": "SummerOffer",
                "options": [
                    {
                        "content": "<p><b>Enjoy this 15% discount on your next purchase</b></p>",
                        "type": "html",
                        "eventToken": "GcvBXDhdJFNR9E9r1tgjfmqipfsIHvVzTQxHolz2IpSCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==",
                    }
                ]
            },
            {
                "index": 2,
                "name": "SummerShoesOffer",
                "options": [
                    {
                        "content": "<p><b>Enjoy this 15% discount on your next shoe purchase</b></p>"
                        "type": "html",
                        "eventToken": "GcvBXDhdJFNR9E9r1tgjfmqipfsIHvVzTQxHolz2IpSCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==",
                    }
                ]
            },
            {
                "index": 3,
                "name": "SummerDressOffer",
                "options": [
                    {
                        "content": "<p><b>Enjoy this 15% discount on your next dress purchase</b></p>"
                        "type": "html",
                        "eventToken": "GcvBXDhdJFNR9E9r1tgjfmqipfsIHvVzTQxHolz2IpSCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==",
                    }
                ]
            }
        ]
    }
}
```

All’interno della risposta, visualizzerai `content` campo contenente l’esperienza da mostrare all’utente per una particolare `mbox`. Questa funzione è molto utile quando è memorizzata nella cache del server, in modo che quando un utente interagisce con l’app web o mobile all’interno di una sessione e visita un’ `mbox` in una pagina specifica dell&#39;applicazione, l&#39;esperienza può essere distribuita dalla cache anziché crearne un&#39;altra [!UICONTROL API di consegna di Adobe Target] chiamare. Tuttavia, quando un’esperienza viene consegnata all’utente da `mbox`, a `notification` sarà inviato tramite una chiamata API di consegna per consentire la registrazione delle impression. Questo perché la risposta di `prefetch` Le chiamate di sono memorizzate nella cache, il che significa che l&#39;utente non ha visualizzato le esperienze al momento della `prefetch` chiamata eseguita. Per saperne di più sulle `notification` processo, consulta [Notifiche](notifications.md).

## Preacquisire mbox con le metriche clickTrack quando si utilizza [!UICONTROL Analytics for Target] (A4T)

[[!UICONTROL Adobe Analytics for Target]](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html){target=_blank} (A4T) è un’integrazione tra soluzioni che consente di creare attività basate su [!DNL Analytics] metriche di conversione e segmenti di pubblico.

Il seguente frammento di codice è una risposta di una preacquisizione di una mbox contenente `clickTrack` metriche da notificare [!DNL Analytics] che è stato fatto clic su un’offerta:

```
{
  "prefetch": {
    "mboxes": [
      {
        "index": 0,
        "name": "<mboxName>",
        "options": [
           ...
        ],
        "metrics": [
          {
            "type": "click",
            "eventToken": "<eventToken>",
             "analytics": {
               "payload": {
                 "pe": "tnt",
                 "tnta": "..."
               }
             }
          },
          }
        ],
        "analytics": {
          "payload": {
            "pe": "tnt",
            "tnta": "347565:1:0|2,347565:1:0|1"
          }
        }
      }
    ]
  }
}
```

>[!NOTE]
>
>La preacquisizione di una mbox contiene [!DNL Analytics] payload solo per attività qualificate. La preacquisizione delle metriche di successo per le attività non ancora qualificate genera incongruenze nei rapporti.

## Preacquisire le viste

Le visualizzazioni supportano le applicazioni a pagina singola (SPA) e le applicazioni mobili in modo più semplice. Le visualizzazioni possono essere viste come un gruppo logico di elementi visivi che insieme formano un’esperienza SPA o mobile. Ora, tramite l’API di consegna, il Compositore esperienza visivo ha creato attività AB e XT con modifiche su [Opinioni per l&#39;SPA](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md) ora può essere preacquisito.

```
curl -X POST \
  'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=a3e7368c62d944c0855d424cd7a03ab0' \
  -H 'Content-Type: application/json' \
  -H 'cache-control: no-cache' \
  -d '{
  "id": {
    "tntId": "84e8d0e211054f18af365d65f45e902b.28_131"
  },
  "context": {
    "channel": "web",
    "window": {
      "width": 1819,
      "height": 842
    },
    "browser": {
      "host": "target.enablementadobe.com"
    },
    "address": {
      "url": "https://target.enablementadobe.com/react/demo/#/"
    }
  },
  "prefetch": {
    "views": [{}]
  }
}'
```

La chiamata di esempio precedente preacquisirà tutte le visualizzazioni create tramite il Compositore esperienza visivo dell’SPA per le attività AB e XT da visualizzare per il web `channel`. Nella chiamata di vogliamo preacquisire tutte le visualizzazioni dalle attività AB o XT con cui un visitatore `tntId`:`84e8d0e211054f18af365d65f45e902b.28_131` che sta visitando `url`:`https://target.enablementadobe.com/react/demo/#/` si qualifica per.

```
{
    "status": 200,
    "requestId": "14ce028e-d2d2-4504-b3da-32740fa8dd61",
    "client": "demo",
    "id": {
        "tntId": "84e8d0e211054f18af365d65f45e902b.28_131"
    },
    "edgeHost": "mboxedge28.tt.omtrdc.net",
    "prefetch": {
        "views": [
            {
                "id": 228,
                "name": "checkout-express",
                "key": "checkout-express",
                "state": "Vqfb6kYGAmzWOLf9W6E+Q/0LyS+SYe2h5tuTXzRNnkjKkZaZZr2ijp41/6AwK6fdFgADhFNC7l5efUCs9shgTw==",
                "options": [
                    {
                        "content": [
                            {
                                "type": "setHtml",
                                "selector": "#app > DIV:nth-of-type(1) > DIV:nth-of-type(2) > SECTION.section:eq(0) > DIV.container:eq(0) > FORM.col-md-4:eq(0) > DIV:nth-of-type(1) > DIV.mb-3:eq(2)",
                                "cssSelector": "#app > DIV:nth-of-type(1) > DIV:nth-of-type(2) > SECTION:nth-of-type(1) > DIV:nth-of-type(1) > FORM:nth-of-type(2) > DIV:nth-of-type(1) > DIV:nth-of-type(3)",
                                "content": "<span style=\"color:#000080;\"><strong>*We charge an additional fee of $12.34 for faster delivery. If you choose express delivery get 15% off on your next order.</strong></span>"
                            }
                        ],
                        "type": "actions",
                        "eventToken": "N3C13I0M2PH8iaKtONJlFJNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="
                    }
                ]
            },
            {
                "id": 5,
                "name": "home",
                "key": "home",
                "state": "Vqfb6kYGAmzWOLf9W6E+Q/0LyS+SYe2h5tuTXzRNnkjKkZaZZr2ijp41/6AwK6fdFgADhFNC7l5efUCs9shgTw==",
                "options": [
                    {
                        "content": [
                            {
                                "type": "setHtml",
                                "selector": "#app > DIV:nth-of-type(1) > DIV:nth-of-type(2) > SECTION.section:eq(0) > DIV.container:eq(1) > DIV.heading:eq(0) > H1.title:eq(0)",
                                "cssSelector": "#app > DIV:nth-of-type(1) > DIV:nth-of-type(2) > SECTION:nth-of-type(1) > DIV:nth-of-type(2) > DIV:nth-of-type(1) > H1:nth-of-type(1)",
                                "content": "<span style=\"color:#800000;\"><strong>Trending Items</strong></span>"
                            }
                        ],
                        "type": "actions",
                        "eventToken": "N3C13I0M2PH8iaKtONJlFJNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="
                    }
                ]
            },
            {
                "id": 6,
                "name": "products",
                "key": "products",
                "state": "Vqfb6kYGAmzWOLf9W6E+Q/0LyS+SYe2h5tuTXzRNnkjKkZaZZr2ijp41/6AwK6fdFgADhFNC7l5efUCs9shgTw==",
                "options": [
                    {
                        "content": [
                            {
                                "type": "setStyle",
                                "selector": "#app > DIV:nth-of-type(1) > DIV:nth-of-type(2) > SECTION.section:eq(0) > DIV.container:eq(0) > DIV.heading:eq(0) > BUTTON.btn:eq(0)",
                                "cssSelector": "#app > DIV:nth-of-type(1) > DIV:nth-of-type(2) > SECTION:nth-of-type(1) > DIV:nth-of-type(1) > DIV:nth-of-type(1) > BUTTON:nth-of-type(1)",
                                "content": {
                                    "background-color": "rgba(191,0,0,1)",
                                    "priority": "important"
                                }
                            }
                        ],
                        "type": "actions",
                        "eventToken": "N3C13I0M2PH8iaKtONJlFJNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="
                    }
                ]
            }
        ]
    }
}
```

In `content` campi della risposta, metadati di note come `type`, `selector`, `cssSelector`, e `content`, utilizzati per eseguire il rendering dell’esperienza per l’utente finale quando un utente visita la pagina. Tieni presente che `prefetched` il contenuto può essere memorizzato in cache e sottoposto a rendering per l’utente, quando necessario.
