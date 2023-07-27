---
title: API di consegna Adobe Target Consegna singola o in batch
description: Come si utilizza [!UICONTROL API di consegna di Adobe Target] Chiamate di consegna singole o batch?
keywords: api di consegna
exl-id: 525cd1f2-616a-486c-8f49-8117615500bb
feature: APIs/SDKs
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '460'
ht-degree: 0%

---

# Consegna singola o in batch

Il [!UICONTROL API di consegna di Adobe Target] supporta una chiamata di consegna singola o batch. È possibile effettuare una richiesta al server per contenuto per una o più mbox.

Quando si decide di effettuare una singola chiamata rispetto a una chiamata in batch, è necessario valutare i costi delle prestazioni. Se conosci tutti i contenuti che devono essere visualizzati per un utente, la best practice consiste nel recuperare contenuti per tutte le mbox con una singola chiamata di consegna in batch, al fine di evitare di effettuare più chiamate di consegna singole.

## Chiamata di consegna singola

Puoi recuperare un’esperienza da mostrare all’utente per una mbox tramite [!UICONTROL API di consegna di Adobe Target]. Tieni presente che se effettui una singola chiamata di consegna, dovrai avviare un’altra chiamata al server per recuperare contenuto aggiuntivo per una mbox per un utente. Questo può diventare molto costoso nel tempo, quindi assicurati di valutare il tuo approccio quando utilizzi la singola chiamata API di consegna.

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
    "execute": {
    "mboxes" : [
      {
        "name" : "SummerOffer",
        "index" : 1
      }
    ]
  }
}'
```

Nell’esempio di chiamata di consegna singola precedente, l’esperienza viene recuperata e visualizzata all’utente con `tntId`: `abcdefghijkl00023.1_1` per un `mbox`:`SummerOffer` sul canale web. Questa singola chiamata di consegna genera la seguente risposta:

```
{
  "status": 200,
  "requestId": "25e0cc42-3d7b-456a-8b49-af60c1fb23d9",
  "client": "demo",
  "id": {
      "tntId": "abcdefghijkl00023.1_1"
  },
  "edgeHost": "mboxedge28.tt.omtrdc.net",
  "execute": {
      "mboxes": [
          {
              "index": 1,
              "name": "SummerOffer",
              "options": [
                  {
                      "content": "<p><b>Enjoy this 15% discount on your next purchase</b></p>",
                      "type": "html",
                  }
              ]
          }
      ]
    }
}
```

Nella risposta, tieni presente `content` Questo campo contiene il HTML che descrive l’esperienza da mostrare all’utente per il web che corrisponde alla mbox SummerOffer.

### Esegui caricamento pagina

Se esistono esperienze che devono essere visualizzate quando si verifica un caricamento di pagina nel canale web, come ad esempio test AB dei font che si trovano nel piè di pagina o nell’intestazione, puoi specificare `pageLoad` nel `execute` per recuperare tutte le modifiche da applicare.

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
  "execute": {
    "pageLoad": {}
  }
}'
```

La chiamata di esempio precedente recupera tutte le esperienze da mostrare a un utente quando la pagina `https://target.enablementadobe.com/react/demo/#/` caricamenti.

```
{
      "status": 200,
      "requestId": "355ebc47-edb6-481f-aeae-ae55d71afaca",
      "client": "demo",
      "id": {
          "tntId": "84e8d0e211054f18af365d65f45e902b.28_131"
      },
      "edgeHost": "mboxedge28.tt.omtrdc.net",
      "execute": {
          "pageLoad": {
              "options": [
                  {
                      "content": [
                          {
                              "type": "setHtml",
                              "selector": "#app > DIV:nth-of-type(1) > DIV:nth-of-type(1) > NAV.nav:eq(0) > DIV.container:eq(0) > DIV.nav-right:eq(0) > A.nav-item:eq(0)",
                              "cssSelector": "#app > DIV:nth-of-type(1) > DIV:nth-of-type(1) > NAV:nth-of-type(1) > DIV:nth-of-type(1) > DIV:nth-of-type(2) > A:nth-of-type(1)",
                              "content": "Modified Home"
                          }
                      ],
                      "type": "actions"
                  }
              ],
              "metrics": [
                  {
                      "type": "click",
                      "selector": "#app > DIV:nth-of-type(1) > DIV:nth-of-type(2) > SECTION.section:eq(0) > DIV.container:eq(0) > FORM.col-md-4:eq(0) > DIV.form-group:eq(0) > BUTTON.btn:eq(0)",
                      "eventToken": "QPaLjCeI9qKCBUylkRQKBg=="
                  }
              ]
          }
      }
  }
```

In `content` , è possibile recuperare la modifica che deve essere applicata al caricamento di una pagina. Nell’esempio precedente, tieni presente che un collegamento sull’intestazione deve essere denominato *Home Modificato*.

## Chiamata di consegna in batch

Invece di effettuare più chiamate di consegna con una singola mbox in ogni chiamata, effettuare una chiamata di consegna con un batch di mbox può ridurre le chiamate al server non necessarie. L’invocazione di una chiamata al server deve essere minimizzata il più possibile per essere altamente performante.

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
    "execute": {
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

Nell’esempio di chiamata di consegna in batch precedente, le esperienze vengono recuperate e visualizzate per l’utente con `tntId`: `abcdefghijkl00023.1_1` per più `mbox`:`SummerOffer`, `SummerShoesOffer`, e `SummerDressOffer`. Poiché sappiamo che è necessario mostrare un’esperienza per più mbox per questo utente, possiamo raggruppare queste richieste in batch ed effettuare una chiamata al server invece di tre singole chiamate di consegna.

```
{
  "status": 200,
  "requestId": "fe15286f-effb-434f-85d8-c3db804075ce",
  "client": "demo",
  "id": {
      "tntId": "abcdefghijkl00023.28_120"
  },
  "edgeHost": "mboxedge28.tt.omtrdc.net",
  "execute": {
      "mboxes": [
          {
              "index": 1,
              "name": "SummerOffer",
              "options": [
                  {
                      "content": "<p><b>Enjoy this 15% discount on your next purchase</b></p>",
                      "type": "html",

                  }
              ]
          },
          {
              "index": 2,
              "name": "SummerShoesOffer",
              "options": [
                  {
                      "content": "<p><b>Enjoy this 15% discount on your next shoe purchase</b></p>",
                      "type": "html",
                  }
              ]
          },
          {
              "index": 3,
              "name": "SummerDressOffer",
              "options": [
                  {
                      "content": "<p><b>Enjoy this 15% discount on your next dress purchase</b></p>",
                      "type": "html",
                  }
              ]
          }
      ]
  }
}
```

Nella risposta precedente, puoi vedere che all’interno di `content` di ogni mbox, è possibile recuperare la rappresentazione HTML dell’esperienza da mostrare all’utente per ogni mbox.
