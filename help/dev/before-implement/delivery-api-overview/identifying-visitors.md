---
title: API di consegna Adobe Target che identifica i visitatori
description: Come posso identificare l’utente in [!DNL Adobe Target]?
keywords: api di consegna
exl-id: 5b8c28aa-caad-44a9-880a-3c5f844e47b2
feature: APIs/SDKs
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 8%

---

# Identificazione dei visitatori

Un visitatore può essere identificato in più modi [!DNL Adobe Target].

Target utilizza tre identificatori:

| Nome campo | Descrizione |
| --- | --- |
| `tntId` | Il `tntId` è l’identificatore primario in [!DNL Target] per un utente. Puoi fornire questo ID o [!DNL Target] La genererà automaticamente se la richiesta non ne contiene uno. |
| `thirdPartyId` | Il `thirdPartyId` è l’identificatore aziendale dell’utente che puoi inviare con ogni chiamata. Quando un utente accede al sito di un’azienda, l’azienda in genere crea un ID associato all’account del visitatore, alla carta fedeltà, al numero di iscrizione o ad altri identificatori applicabili per l’azienda. |
| `marketingCloudVisitorId` | Il `marketingCloudVisitorId` viene utilizzato per unire e condividere i dati tra diverse soluzioni Adobe. Il `marketingCloudVisitorId` è richiesto per le integrazioni con Adobe Analytics e Adobe Audience Manager. |
| `customerIds` | Insieme all’ID visitatore Experience Cloud, sono disponibili [ID cliente](https://experienceleague.adobe.com/docs/id-service/using/reference/authenticated-state.html) e puoi utilizzare uno stato di autenticazione per ogni visitatore. |

## [!DNL Target] ID

Il [!DNL Target] ID o `tntId` può essere visto come un ID dispositivo. Questo `tntId` viene generato automaticamente da [!DNL Target] se non viene fornito nella richiesta. In seguito, le richieste successive devono includere quanto segue `tntId` affinché il contenuto corretto venga distribuito a un dispositivo utilizzato dall’utente.

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

La chiamata di esempio precedente dimostra che un `tntId` non ha bisogno di essere trasmesso. In questo scenario, [!DNL Target]  genera un `tntId` e forniscilo nella risposta, come illustrato di seguito:

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

Il valore generato `tntId` è `10abf6304b2714215b1fd39a870f01afc.28_20`. Nota questo `tntId` deve essere utilizzato quando si chiama [!UICONTROL API di consegna di Adobe Target] per lo stesso utente in più sessioni.

## ID visitatore di Marketing Cloud

Il `marketingCloudVisitorId` è un ID universale e costante che identifica i visitatori in tutte le soluzioni dell’Experience Cloud. Quando la tua organizzazione implementa il servizio ID, questo ID ti consente di identificare lo stesso visitatore del sito e i relativi dati in diverse soluzioni di Experience Cloud come Adobe Target, Adobe Analytics o Adobe Audience Manager. Tieni presente che `marketingCloudVisitorId` è richiesto per l’utilizzo e l’integrazione con Analytics e Audienci Manager.

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

La chiamata di esempio precedente illustra come una `marketingCloudVisitorId` recuperato dal servizio ID Experience Cloud, viene passato ad Adobe Target. In questo scenario, [!DNL Target] genera un `tntId` poiché non è stato passato alla chiamata originale, che verrà mappata sul `marketingCloudVisitorId` come mostrato nella risposta di seguito.

## ID di terze parti

Se la tua organizzazione utilizza un ID per identificare il visitatore, puoi utilizzare `thirdPartyID` per distribuire contenuti. Tuttavia, devi fornire `thirdPartyID` per ogni [!UICONTROL API di consegna di Adobe Target] chiamala make.

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

La chiamata di esempio precedente mostra un `thirdPartyId`, che è un ID persistente utilizzato dalla tua azienda per identificare un utente finale, indipendentemente dal fatto che interagisca con la tua azienda dai canali web, mobili o IoT. In altre parole, `thirdPartyId` farà riferimento ai dati del profilo utente che possono essere utilizzati tra i canali. In questo scenario, [!DNL Target] genera un `tntId`, poiché non è stato passato alla chiamata originale, che verrà mappata sul fornito `thirdPartyId` come mostrato nella risposta di seguito.

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

[ID cliente](https://experienceleague.adobe.com/docs/id-service/using/reference/authenticated-state.html) può essere aggiunto e associato a un ID visitatore Experience Cloud. Ogni volta che invia `customerIds` il `marketingCloudVisitorId` devono essere fornite. Inoltre, insieme a ciascuno è possibile fornire uno stato di autenticazione `customerId` per ogni visitatore. È possibile prendere in considerazione il seguente stato di autenticazione:

| Stato di autenticazione | Stato dell&#39;utente |
| --- | --- |
| `unknown` | Utente sconosciuto o mai autenticato. Questo stato può essere utilizzato per scenari come quello di un visitatore che è atterrato sul sito facendo clic su un annuncio pubblicitario. |
| `authenticated` | Al momento l&#39;utente è autenticato con una sessione attiva sul sito Web o sull&#39;app. |
| `logged_out` | L&#39;utente si è autenticato ma si è disconnesso in modo attivo. L&#39;utente desiderava disconnettersi dallo stato autenticato. L&#39;utente non desidera più essere trattato come utente autenticato. |

Tieni presente che solo quando l’ID cliente è in `authenticated` Lo stato farà riferimento ai dati del profilo utente memorizzati e collegati all’ID cliente in Target. Se l’ID cliente si trova in `unknown` o `logged_out` stato, l’id cliente verrà ignorato e tutti i dati del profilo utente associati a esso non verranno utilizzati per il targeting di pubblico.

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

La chiamata di esempio precedente illustra come inviare un `customerId` con un `authenticatedState`. Quando si invia una `customerId`, il `integrationCode`, `id`, e `authenticatedState` nonché `marketingCloudVisitorId` sono obbligatori. Il `integrationCode` è l’alias del [file attributi cliente](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/working-with-customer-attributes.html?lang=it) fornite tramite CRS.

## Profilo unito

È possibile combinare `tntId`, `thirdPartyID`, e `marketingCloudVisitorId` nella stessa richiesta. In questo scenario, Adobe Target manterrà la mappatura di tutti questi ID e la fisserà a un visitatore. Scopri come sono i profili [unione e sincronizzazione in tempo reale](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/3rd-party-id.html) utilizzando i diversi identificatori.

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

La chiamata di esempio precedente illustra come combinare `tntId`, `thirdPartyID`, e `marketingCloudVisitorId` nella stessa richiesta. Nella risposta vengono restituiti anche tutti e tre gli ID.
