---
title: Preacquisizione API di consegna Adobe Target
description: Come si utilizza la preacquisizione in [!UICONTROL Adobe Target Delivery API]?
keywords: api di consegna
exl-id: eab88e3a-442c-440b-a83d-f4512fc73e75
feature: APIs/SDKs
source-git-commit: 4ff2746b8b485fe3d845337f06b5b0c1c8d411ad
workflow-type: tm+mt
source-wordcount: '522'
ht-degree: 0%

---

# Preacquisizione

La preacquisizione consente ai client come le app e i server mobili di recuperare contenuti per più mbox o visualizzazioni in una richiesta, memorizzarli nella cache locale e successivamente inviare una notifica a [!DNL Target] quando il visitatore visita tali mbox o visualizzazioni.

Quando si utilizza la preacquisizione, è importante avere familiarità con i seguenti termini:

| Nome campo | Descrizione |
| --- | --- |
| `prefetch` | Elenco di mbox e visualizzazioni che devono essere recuperate ma non contrassegnate come visitate. L&#39;Edge [!DNL Target] restituisce un `eventToken` per ogni mbox o vista esistente nell&#39;array di preacquisizione. |
| `notifications` | Elenco di mbox e visualizzazioni precedentemente preacquisite che devono essere contrassegnate come visitate. |
| `eventToken` | Token crittografato con hash restituito quando il contenuto viene prerecuperato. Questo token deve essere rimandato a [!DNL Target] nell&#39;array `notifications`. |

## Preacquisizione di Mbox

I client, ad esempio le app per dispositivi mobili e i server, possono preacquisire più mbox per un determinato visitatore all&#39;interno di una sessione e memorizzarle nella cache per evitare più chiamate a [!UICONTROL Adobe Target Delivery API].

```shell shell-session
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

All&#39;interno del campo `prefetch`, aggiungi uno o più `mboxes` da preacquisire almeno una volta per un visitatore all&#39;interno di una sessione. Dopo la preacquisizione di questi `mboxes`, si riceve la seguente risposta:

```JSON {line-numbers="true"}
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

All&#39;interno della risposta, viene visualizzato il campo `content` contenente l&#39;esperienza da mostrare al visitatore per un particolare `mbox`. Questa funzione è molto utile quando è memorizzata nella cache del server, in modo che, quando un visitatore interagisce con l&#39;app web o mobile all&#39;interno di una sessione e visita un `mbox` in una pagina specifica dell&#39;applicazione, l&#39;esperienza possa essere distribuita dalla cache anziché effettuare un&#39;altra chiamata a [!UICONTROL Adobe Target Delivery API]. Tuttavia, quando un&#39;esperienza viene consegnata al visitatore da `mbox`, un `notification` viene inviato tramite una chiamata API di consegna per consentire la registrazione delle impression. Questo perché la risposta di `prefetch` chiamate è memorizzata nella cache, il che significa che il visitatore non ha visto le esperienze al momento della chiamata di `prefetch`. Per ulteriori informazioni sul processo `notification`, vedere [Notifiche](notifications.md).

## Preacquisire mbox con `clickTrack` metriche quando si utilizza [!UICONTROL Analytics for Target] (A4T)

[[!UICONTROL Adobe Analytics for Target]](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html?lang=it){target=_blank} (A4T) è un&#39;integrazione tra soluzioni che consente di creare attività basate su [!DNL Analytics] metriche di conversione e segmenti di pubblico.

Il seguente frammento di codice è una risposta di un recupero preventivo di una mbox contenente `clickTrack` metriche per notificare a [!DNL Analytics] che è stato fatto clic su un&#39;offerta:

```JSON {line-numbers="true"}
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
>La preacquisizione di una mbox contiene il payload [!DNL Analytics] solo per le attività qualificate. La preacquisizione delle metriche di successo per le attività non ancora qualificate genera incongruenze nei rapporti.

## Preacquisire le viste

Le visualizzazioni supportano le applicazioni a pagina singola (SPA) e le applicazioni mobili in modo più semplice. Le visualizzazioni possono essere viste come un gruppo logico di elementi visivi che insieme formano un’esperienza SPA o mobile. Ora, tramite l&#39;API di consegna, è possibile preacquisire le attività [[!UICONTROL A/B Test]](https://experienceleague.adobe.com/docs/target/using/activities/abtest/test-ab.html?lang=it){target=_blank} e [[!UICONTROL Experience Targeting]](https://experienceleague.adobe.com/docs/target/using/activities/experience-targeting/experience-target.html?lang=it){target=_blank} (X)T create dal Compositore esperienza visivo con modifiche su [Visualizzazioni per SPA](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md).

```shell  {line-numbers="true"}
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

La chiamata di esempio precedente recupera tutte le visualizzazioni create tramite il Compositore esperienza visivo SPA per [!UICONTROL A/B Test] e le attività XT da visualizzare per il Web `channel`. Si noti che la chiamata preacquisisce tutte le visualizzazioni dalle attività [!UICONTROL A/B Test] o XT per le quali un visitatore con `tntId`:`84e8d0e211054f18af365d65f45e902b.28_131` che sta visitando il `url`:`https://target.enablementadobe.com/react/demo/#/` è idoneo.

```JSON  {line-numbers="true"}
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

Nei campi `content` della risposta, annota i metadati come `type`, `selector`, `cssSelector` e `content`, utilizzati per eseguire il rendering dell&#39;esperienza per il visitatore quando un utente visita la pagina. Si noti che il contenuto `prefetched` può essere memorizzato nella cache e sottoposto a rendering per l&#39;utente quando necessario.
