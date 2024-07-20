---
title: Autorizzazioni utente per l’API di consegna di Adobe Target
description: Autorizzazioni utente per l’API di consegna di Adobe Target
badgePremium: label="Premium" type="Positive" url="https://experienceleague.adobe.com/docs/target/using/introduction/intro.html?lang=en#premium newtab=true" tooltip="Vedi cosa è incluso in Target Premium."
keywords: api di consegna
exl-id: 332f90bd-4079-4653-aa38-b35837631c94
feature: APIs/SDKs
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 0%

---

# Autorizzazioni utente (Premium)

[!DNL Adobe] consente ai clienti di gestire le autorizzazioni per i propri utenti quando si utilizza Adobe Target. Per effettuare una chiamata [!UICONTROL Adobe Target Delivery API] corretta, è necessario trasmettere un token con le autorizzazioni appropriate all&#39;interno della chiamata API. Per ulteriori informazioni sulle autorizzazioni per gli utenti e su come recuperare il token, visita [questa documentazione](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/properties-overview.html).

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

Una volta ottenuto il token corrispondente, passalo in `property` -> `token` per ogni chiamata API effettuata. Se `property` -> `token` non viene passato all&#39;interno di ogni chiamata API, non verrà restituito alcun `content` da Adobe Target.

```
{
    "status": 200,
    "requestId": "07ce783d-58b9-461c-9f4c-6873aeb00c01",
    "client": "demo",
    "id": {
        "tntId": "d359234570e04f14e1faeeba02d6ab9914e.28_7"
    },
    "edgeHost": "mboxedge28.tt.omtrdc.net",
    "execute": {
        "mboxes": [
            {
                "index": 1,
                "name": "homepage"
            }
        ]
    }
}
```

Come si può vedere in precedenza, senza passare `property` -> `token`, non si otterrà alcun contenuto. Se il contenuto previsto dalla chiamata API non viene recuperato dalla risposta, è probabile che `property` -> `token` non sia stato fornito o che venga trasmesso senza le autorizzazioni corrette.
