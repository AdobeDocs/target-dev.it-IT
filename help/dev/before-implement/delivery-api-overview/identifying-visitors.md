---
title: API di consegna Adobe Target che identifica i visitatori
description: Come posso identificare l'utente in  [!DNL Adobe Target]?
keywords: API Delivery
exl-id: 5b8c28aa-caad-44a9-880a-3c5f844e47b2
feature: APIs/SDKs
TQID: https://experienceleague.adobe.com/ciTxaPn8odyuyHzrnqhPWzdmpcU2bknOATGCt-ZtAZw
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: adee20bd-51f4-461d-b9db-d215f8756eeb
  - id: f7c7de77-382f-4f48-8b36-61a170f06d3d
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 797
ht-degree: 9%

---

# Identificazione dei visitatori

Un visitatore può essere identificato in più modi in [!DNL Adobe Target].

Target utilizza tre identificatori:

| Nome campo | Descrizione |
| --- | --- |
| `tntId` | `tntId` è l&#39;identificatore primario in [!DNL Target] per un utente. È possibile specificare questo ID oppure [!DNL Target] lo genererà automaticamente se la richiesta non ne contiene uno. |
| `thirdPartyId` | `thirdPartyId` è l&#39;identificatore della tua azienda per l&#39;utente che puoi inviare con ogni chiamata. Quando un utente accede al sito di un’azienda, l’azienda in genere crea un ID associato all’account del visitatore, alla carta fedeltà, al numero di iscrizione o ad altri identificatori applicabili per l’azienda. |
| `marketingCloudVisitorId` | `marketingCloudVisitorId` viene utilizzato per unire e condividere dati tra diverse soluzioni Adobe. `marketingCloudVisitorId` è richiesto per le integrazioni con Adobe Analytics e Adobe Audience Manager. |
| `customerIds` | Oltre all&#39;ID visitatore di Experience Cloud, è possibile utilizzare altri [ID cliente](https://experienceleague.adobe.com/docs/id-service/using/reference/authenticated-state.html?lang=it) e uno stato di autenticazione per ogni visitatore. |

## ID [!DNL Target]

L&#39;ID [!DNL Target] o `tntId` può essere visto come un ID dispositivo. `tntId` viene generato automaticamente da [!DNL Target] se non viene fornito nella richiesta. In seguito, le richieste successive devono includere `tntId` per consentire la distribuzione del contenuto corretto a un dispositivo utilizzato dall&#39;utente.

```http {line-numbers="true"}
curl -X POST \
'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=10abf6304b2714215b1fd39a870f01afc#1555632114' \
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

La chiamata di esempio precedente dimostra che non è necessario passare `tntId`. In questo scenario, [!DNL Target] genera un `tntId` e lo fornisce nella risposta, come illustrato di seguito:

```URI {line-numbers="true"}
{
  "status": 200,
  "requestId": "5b586f83-890c-46ae-93a2-610b1caa43ef",
  "client": "demo",
  "id": {
      "tntId": "10abf6304b2714215b1fd39a870f01afc.28_20"
  },
  "edgeHost": "mboxedge28.tt.omtrdc.net",
  ...
}
```

`tntId` generato è `10abf6304b2714215b1fd39a870f01afc.28_20`. Nota: `tntId` deve essere utilizzato quando si chiama l&#39;[!UICONTROL API di consegna di Adobe Target] per lo stesso utente in più sessioni.

## ID visitatore di Marketing Cloud

`marketingCloudVisitorId` è un ID universale e costante che identifica i visitatori in tutte le soluzioni di Experience Cloud. Quando l&#39;organizzazione implementa il servizio ID, questo ID consente di identificare lo stesso visitatore del sito e i relativi dati in diverse soluzioni Experience Cloud come Adobe Target, Adobe Analytics o Adobe Audience Manager. Tieni presente che `marketingCloudVisitorId` è richiesto per l&#39;utilizzo e l&#39;integrazione con Analytics e Audience Manager.

```
curl -X POST \
  'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=10abf6304b2714215b1fd39a870f01afc#1555632114' \
  -H 'Content-Type: application/json' \
  -H 'cache-control: no-cache' \
  -d '{
  "id": {
    "marketingCloudVisitorId": "10527837386392355901041112038610706884"
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

La chiamata di esempio precedente illustra come un `marketingCloudVisitorId` recuperato dal servizio Experience Cloud ID viene passato ad Adobe Target. In questo scenario, [!DNL Target] genera un `tntId` poiché non è stato passato alla chiamata originale che verrà mappato al `marketingCloudVisitorId` fornito come mostrato nella risposta seguente.

## ID di terze parti

Se l&#39;organizzazione utilizza un ID per identificare il visitatore, è possibile utilizzare `thirdPartyID` per distribuire il contenuto. Tuttavia, devi fornire `thirdPartyID` per ogni chiamata [!UICONTROL API di consegna Adobe Target] effettuata.

```
curl -X POST \
  'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=10abf6304b2714215b1fd39a870f01afc#1555632114' \
  -H 'Content-Type: application/json' \
  -H 'cache-control: no-cache' \
  -d '{
  "id": {
    "thirdPartyId": "B234A029348"
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

La chiamata di esempio precedente mostra un `thirdPartyId`, che è un ID persistente utilizzato dalla tua azienda per identificare un utente finale indipendentemente dal fatto che interagisca con la tua azienda dai canali web, mobili o IoT. In altre parole, `thirdPartyId` farà riferimento ai dati del profilo utente che possono essere utilizzati in tutti i canali. In questo scenario, [!DNL Target] genera un `tntId`, poiché non è stato passato alla chiamata originale, che verrà mappato al `thirdPartyId` fornito come mostrato nella risposta seguente.

```
{
    "status": 200,
    "requestId": "55de9886-bd14-4dee-819c-7d1633b79b90",
    "client": "demo",
    "id": {
        "tntId": "10abf6304b2714215b1fd39a870f01afc.28_20",
        "thirdPartyId": "B234A029348"
    },
    "edgeHost": "mboxedge28.tt.omtrdc.net",
    ...
}
```

## ID cliente

È possibile aggiungere [ID cliente](https://experienceleague.adobe.com/docs/id-service/using/reference/authenticated-state.html?lang=it) e associarli a un ID visitatore di Experience Cloud. Ogni volta che invii `customerIds` devi fornire anche `marketingCloudVisitorId`. Inoltre, è possibile fornire uno stato di autenticazione insieme a ogni `customerId` per ogni visitatore. È possibile prendere in considerazione il seguente stato di autenticazione:

| Stato di autenticazione | Stato dell&#39;utente |
| --- | --- |
| `unknown` | Utente sconosciuto o mai autenticato. Questo stato può essere utilizzato per scenari come quello di un visitatore che è atterrato sul sito facendo clic su un annuncio pubblicitario. |
| `authenticated` | Al momento l&#39;utente è autenticato con una sessione attiva sul sito Web o sull&#39;app. |
| `logged_out` | L&#39;utente si è autenticato ma si è disconnesso in modo attivo. L&#39;utente desiderava disconnettersi dallo stato autenticato. L&#39;utente non desidera più essere trattato come utente autenticato. |

Tieni presente che solo quando l&#39;ID cliente si trova nello stato `authenticated`, Target farà riferimento ai dati del profilo utente memorizzati e collegati all&#39;ID cliente. Se l&#39;ID cliente si trova nello stato `unknown` o `logged_out`, l&#39;ID cliente verrà ignorato e tutti i dati del profilo utente associati ad esso non verranno utilizzati per il targeting del pubblico.

```
curl -X POST \
  'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=d359234570e044f14e1faeeba02d6ab23439914e' \
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
        "marketingCloudVisitorId" : "2304820394812039",
        "customerIds": [{
          "id": "134325423",
          "integrationCode" : "crm_data",
          "authenticatedState" : "authenticated"
        }]
      },
      "property" : {
        "token": "08b62abd-c3e7-dfb2-da93-96b3aa724d81"
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

La chiamata di esempio precedente illustra come inviare `customerId` con `authenticatedState`. Quando si invia un `customerId`, sono necessari `integrationCode`, `id` e `authenticatedState` e `marketingCloudVisitorId`. `integrationCode` è l&#39;alias del [file degli attributi del cliente](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/working-with-customer-attributes.html?lang=it) fornito tramite CRS.

## Profilo unito

È possibile combinare `tntId`, `thirdPartyID` e `marketingCloudVisitorId` nella stessa richiesta. In questo scenario, Adobe Target manterrà la mappatura di tutti questi ID e la fisserà a un visitatore. Scopri come unire e sincronizzare in tempo reale [i profili](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/3rd-party-id.html?lang=it) utilizzando i diversi identificatori.

```
curl -X POST \
  'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=d359234570e044f14e1faeeba02d6ab23439914e' \
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
        "marketingCloudVisitorId" : "2304820394812039",
        "tntId": "d359234570e044f14e1faeeba02d6ab23439914e.28_78",
        "thirdPartyId":"23423432"
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

La chiamata di esempio precedente illustra come combinare `tntId`, `thirdPartyID` e `marketingCloudVisitorId` nella stessa richiesta. Nella risposta vengono restituiti anche tutti e tre gli ID.
