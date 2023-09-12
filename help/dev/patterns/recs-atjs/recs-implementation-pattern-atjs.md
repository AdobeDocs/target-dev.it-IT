---
title: Modello di implementazione di Recommendations utilizzando at.js
description: Come utilizzare il modello di implementazione per Recommendations con at.js
feature: APIs/SDKs
level: Experienced
role: Developer
hide: true
hidefromtoc: true
source-git-commit: 752c52c0db5173f49fd828c297fa7afd7c53c6ce
workflow-type: tm+mt
source-wordcount: '154'
ht-degree: 0%

---

# [!DNL Recommendations] modello di implementazione tramite panoramica di at.js

Questo modello di implementazione consente di comprendere e creare [!DNL Adobe Target Recommendations] implementazione quando si utilizza [Libreria JavaScript at.js](/help/dev/implement/client-side/atjs/how-atjs-works/overview.md).

Fare clic sull&#39;immagine per espandere a schermo intero.

![Diagramma dell’architettura di Adobe Target](/help/dev/patterns/assets/architecture-chart.png){width="600" zoomable="yes"}

Si noti che i numeri nell&#39;immagine non indicano la sequenza di operazioni:

1. SDK lato client per [!DNL Adobe Target] e [!DNL Experience Cloud ID Service]
1. [!DNL Target Delivery API] chiamare
1. [!UICONTROL ID EXPERIENCE CLOUD] (ECID) chiamata di acquisizione
1. API di aggiornamento del profilo bulk e [!DNL Customer Attributes] (CA) servizio
1. Acquisizione dei dati del profilo dalle origini dati del cliente in [!DNL Target] archivio profili
1. Raccogliere dati sul profilo e sul comportamento e decidere quale esperienza mostrare al visitatore
1. Le esperienze eseguono il rendering sulla pagina
1. at.js esegue il rendering delle esperienze sulla pagina

Ogni pattern è costituito da parti diverse, ognuna delle quali corrisponde a un requisito di implementazione critico per [!DNL Target] implementazione.

Ogni parte è spiegata in un articolo separato di questa guida:

* [Inizializzare gli SDK](/help/dev/patterns/recs-atjs/initialize-sdk.md)
* [Configurare la raccolta dati](/help/dev/patterns/recs-atjs/data-collection.md)
* [Esperienze di rendering](/help/dev/patterns/recs-atjs/render-experiences.md)
* [Notifica Target](/help/dev/patterns/recs-atjs/notify-target.md)

