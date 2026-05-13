---
title: Guida introduttiva all’API di consegna di Adobe Target
description: Utilizzo di [!UICONTROL Adobe Target Delivery API]
keywords: API Delivery
exl-id: 142ec3be-b017-4cdc-9079-b1cc173a710a
feature: APIs/SDKs
TQID: https://experienceleague.adobe.com/DC-YVq6VfAaqMU1utmIMw73gzp4PIJgQjaS0a8FQEO4
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 116
ht-degree: 1%

---

# Guida introduttiva a [!UICONTROL Adobe Target Delivery API]

Una chiamata [!UICONTROL Target Delivery API] si presenta così:

```
curl -X POST \
  'https://`clientCode`.tt.omtrdc.net/rest/v1/delivery?client=`clientCode`&sessionId=d359234570e04f14e1faeeba02d6ab9914e' \
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
            "name" : "homepage",
            "index" : 1
          }
        ]
      }
    }'
```

È possibile recuperare `clientCode` dall&#39;interfaccia utente di [!DNL Target] passando a **[!UICONTROL Administration]** > **[!UICONTROL Implementation]**.

Prima di effettuare una chiamata [!UICONTROL Target Delivery API], segui questi passaggi per assicurarti che una risposta contenga l&#39;esperienza rilevante da mostrare agli utenti finali:

1. Creare un&#39;attività [!DNL Target] (A/B, XT, AP o Consigli) utilizzando [Compositore basato su moduli](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?lang=en) o il [Compositore esperienza visivo](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html).
1. Utilizza l&#39;API di consegna per ottenere una risposta per le mbox utilizzate nell&#39;attività [!DNL Target] creata nel passaggio 2.
1. Presenta l’esperienza al visitatore.
