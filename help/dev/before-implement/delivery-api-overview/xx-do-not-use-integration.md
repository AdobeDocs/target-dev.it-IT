---
title: Integrazione con Experience Cloud
description: Integrazione con Experience Cloud
keywords: api di consegna
source-git-commit: f16903556954d2b1854acd429f60fbf6fc2920de
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 5%

---

<!-- The content on this page was originally pulled from the legacy Delivery API doc site, and was subsequently found to be an outdated version of information now found on [Implement > Server-side > Integration > A4T Reporting](../../implement/server-side/sdk-guides/integration-with-experience-cloud/a4t-reporting.md) and [Implement > Server-side > Integration > AAM Segments](../../implement/server-side/sdk-guides/integration-with-experience-cloud/aam-segments.md). Therefore sunsetting this page, but not deleting it from the repo immediately, pending a more thorough examination to avoid inadvertently deleting relevant content. -->


# Integrazione con Experience Cloud

## Adobe Analytics for Target (A4T)

Quando una chiamata API di consegna di Target viene attivata dal server, Adobe Target restituisce l’esperienza per tale utente e, oltre a ciò, Adobe Target restituisce il payload di Adobe Analytics al chiamante o lo inoltra automaticamente ad Adobe Analytics. Per inviare le informazioni sull’attività di Target ad Adobe Analytics sul lato server, è necessario soddisfare alcuni prerequisiti:

1. L’attività è impostata nell’interfaccia utente di Adobe Target con Adobe Analytics come origine per la generazione di rapporti e gli account abilitati per A4T
1. L’ID visitatore di Adobe Marketing Cloud è generato dall’utente API ed è disponibile quando viene attivata la chiamata API di consegna di Target

### Adobe Target inoltra automaticamente il payload di Analytics

Adobe Target può inoltrare automaticamente il payload di Analytics ad Adobe Analytics tramite il lato server se vengono forniti i seguenti identificatori:

1. `supplementalDataId` : ID utilizzato per eseguire l’unione tra Adobe Analytics e Adobe Target
1. `trackingServer` : il server Analytics di Adobe Per consentire ad Adobe Target e Adobe Analytics di unire correttamente i dati, `supplementalDataId` devono essere passati ad Adobe Target e Adobe Analytics.

```
curl -X POST \
  'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=d359234570e04f14e1faeeba02d6ab9914e' \
  -H 'Content-Type: application/json' \
  -H 'cache-control: no-cache' \
  -d '{
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
      "id": {
        "marketingCloudVisitorId": "2304820394812039"
      },
      "property" : {
        "token": "08b62abd-c3e7-dfb2-da93-96b3aa724d81"
      },
      "experienceCloud": {
        "analytics": {
          "supplementalDataId" : "23423498732598234",
          "trackingServer": "ags041.sc.omtrdc.net",
          "logging": "server_side"
        }
      },
        "execute": {
        "mboxes" : [
          {
            "name" : "homepage",
            "index" : 1
          }
        ]
      }
    }'
```

### Recuperare il payload di Analytics da Adobe Target

I consumatori dell’API di consegna di Adobe Target possono recuperare il payload di Adobe Analytics per una mbox corrispondente in modo che il consumatore possa inviare il payload ad Adobe Analytics tramite [API di inserimento dati](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md). Quando viene attivata una chiamata Adobe Target lato server, passa `client_side` al `logging` nella richiesta. Questo a sua volta restituirà un payload se la mbox è presente in un’attività che utilizza Analytics come origine per la generazione di rapporti.

```
curl -X POST \
  'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=d359234570e04f14e1faeeba02d6ab9914e' \
  -H 'Content-Type: application/json' \
  -H 'cache-control: no-cache' \
  -d '{
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
      "property" : {
        "token": "08b62abd-c3e7-dfb2-da93-96b3aa724d81"
      },
      "experienceCloud": {
        "analytics": {
          "logging": "client_side"
        }
      },
        "execute": {
        "mboxes" : [
          {
            "name" : "homepage",
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

Dopo aver specificato `logging` = `client_side`, riceverai il payload in `mbox` come mostrato di seguito.

```
{
    "status": 200,
    "requestId": "4b8855a5-8354-4ac4-8ae7-c551f7c0bb8a",
    "client": "demo",
    "id": {
        "tntId": "d359234570e04f14e1faeeba02d6ab9914e.28_7"
    },
    "edgeHost": "mboxedge28.tt.omtrdc.net",
    "execute": {
        "mboxes": [
            {
                "index": 1,
                "name": "homepage",
                "options": [
                    {
                        "content": "<p><b>Enjoy this 15% discount on your next purchase</b></p>",
                        "type": "html",

                    }
                ],
                "analytics": {
                    "payload": {
                        "pe": "tnt",
                        "tnta": "285408:0:0|2"
                    }
                }
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

Se la risposta di Target contiene elementi nel `analytics` -> `payload` inoltra la proprietà così com&#39;è ad Adobe Analytics. Analytics sa come elaborare questo payload. Questa operazione può essere eseguita in una richiesta GET utilizzando il seguente formato:

```
https://{datacollectionhost.sc.omtrdc.net}/b/ss/{rsid}/0/CODEVERSION?pe=tnt&tnta={payload}&mid={mid}&vid={vid}&aid={aid}
```

### Parametri e variabili della stringa di query

| Nome campo | Obbligatorio | Descrizione |
| --- | --- | --- |
| `rsid` | Sì | ID suite di rapporti |
| `pe` | Sì | Evento pagina. Sempre impostato su `tnt` |
| `tnta` | Sì | Payload di Analytics restituito dal server Target in `analytics` -> `payload` -> `tnta` |
| `mid` | ID visitatore di Marketing Cloud |

### Valori intestazione richiesti

| Nome intestazione | Valore intestazione |
| --- | --- |
| Host | Server di raccolta dati di Analytics (esempio: adobeags421.sc.omtrdc.net) |

### Esempio di chiamata HTTP Get di inserimento dati A4T

```
https://demo.sc.omtrdc.net/b/ss/myCustomRsid/0/MOBILE-1.0?pe=tnt&tnta=285408:0:0|2&mid=2304820394812039
```

## Adobe Audience Manager

I segmenti Adobe Audience Manager (AAM) possono essere utilizzati anche tramite le API di distribuzione di Adobe Target. Per sfruttare i segmenti AAM, è necessario fornire i seguenti campi:

| Nome campo | Obbligatorio | Descrizione |
| --- | --- | --- |
| `locationHint` | Sì | DCS Location Hint viene utilizzato per determinare quale endpoint DCS AAM colpire per recuperare il profilo. Deve essere >= 1. |
| `marketingCloudVisitorId` | Sì | ID visitatore di Marketing Cloud |
| `blob` | Sì | Il BLOB dell’AAM viene utilizzato per inviare dati aggiuntivi all’AAM. Non deve essere vuoto e le dimensioni devono essere &lt;= 1024. |

```
curl -X POST \
  'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=d359234570e04f14e1faeeba02d6ab9914e' \
  -H 'Content-Type: application/json' \
  -H 'cache-control: no-cache' \
  -d '{
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
      "id": {
        "marketingCloudVisitorId": "2304820394812039"
      },
      "property" : {
        "token": "08b62abd-c3e7-dfb2-da93-96b3aa724d81"
      },
      "experienceCloud": {
        "audienceManager": {
          "locationHint": 9,
          "blob": "32fdghkjh34kj5h43"
        }
      },
        "execute": {
        "mboxes" : [
          {
            "name" : "homepage",
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
