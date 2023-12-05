---
title: Guida introduttiva all’API di consegna di Adobe Target
description: Come si utilizza [!UICONTROL API di consegna di Adobe Target]?
keywords: api di consegna
exl-id: 142ec3be-b017-4cdc-9079-b1cc173a710a
feature: APIs/SDKs
source-git-commit: e5a1c38d448cb7446b7b26cd0dc882976ba94dd3
workflow-type: tm+mt
source-wordcount: '112'
ht-degree: 0%

---

# Guida introduttiva a [!UICONTROL API di consegna di Adobe Target]

A [!UICONTROL API di consegna di Target] ha un aspetto simile al seguente:

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

Il `clientCode` può essere recuperato da [!DNL Target] Interfaccia utente di **[!UICONTROL Amministrazione]** > **[!UICONTROL Implementazione]**.

Prima di effettuare una [!UICONTROL API di consegna di Target] chiama, segui questi passaggi per garantire che una risposta contenga l’esperienza pertinente per mostrare agli utenti finali:

1. Creare un [!DNL Target] attività (A/B, XT, AP o Recommendations) tramite [Compositore basato su moduli](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?lang=en) o [Compositore esperienza visivo](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html).
1. Utilizza l’API di consegna per ottenere una risposta per le mbox utilizzate in [!DNL Target] attività creata nel passaggio 2.
1. Presenta l’esperienza al visitatore.
