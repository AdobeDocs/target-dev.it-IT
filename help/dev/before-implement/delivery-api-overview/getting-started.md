---
title: Guida introduttiva all’API di consegna di Adobe Target
description: Utilizzo di [!UICONTROL Adobe Target Delivery API]
keywords: api di consegna
exl-id: 142ec3be-b017-4cdc-9079-b1cc173a710a
feature: APIs/SDKs
source-git-commit: e5a1c38d448cb7446b7b26cd0dc882976ba94dd3
workflow-type: tm+mt
source-wordcount: '96'
ht-degree: 0%

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

1. Creare un&#39;attività [!DNL Target] (A/B, XT, AP o Recommendations) utilizzando il [Compositore basato su moduli](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?lang=it) o il [Compositore esperienza visivo](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html?lang=it).
1. Utilizza l&#39;API di consegna per ottenere una risposta per le mbox utilizzate nell&#39;attività [!DNL Target] creata nel passaggio 2.
1. Presenta l’esperienza al visitatore.
