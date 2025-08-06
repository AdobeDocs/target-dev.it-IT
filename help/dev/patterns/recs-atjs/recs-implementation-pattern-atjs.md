---
title: Schema di implementazione di Recommendations utilizzando at.js
description: Come utilizzare il modello di implementazione per Recommendations con at.js
feature: APIs/SDKs
level: Experienced
role: Developer
exl-id: d568cd1d-acc3-42e0-ae2c-5787e6f361f8
source-git-commit: 3b0bc0b67800ed4b1da6ba2bfa05c677147a78ba
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 0%

---

# Schema di implementazione di [!DNL Recommendations] tramite panoramica di at.js

Questo modello di implementazione consente di comprendere e creare l&#39;implementazione di [!DNL Adobe Target Recommendations] quando si utilizza la libreria JavaScript di [at.js](/help/dev/implement/client-side/atjs/how-atjs-works/how-atjs-works.md).

Fare clic sull&#39;immagine per espandere a schermo intero.

![Diagramma architettura di Adobe Target](/help/dev/patterns/assets/architecture-chart.png){width="600" zoomable="yes"}

Si noti che i numeri nell&#39;immagine non indicano la sequenza di operazioni:

1. SDK lato client per [!DNL Adobe Target] e [!DNL Experience Cloud ID Service]
1. Chiamata [!DNL Target Delivery API]
1. Chiamata di acquisizione [!UICONTROL Experience Cloud ID] (ECID)
1. API di aggiornamento del profilo bulk e servizio [!DNL Customer Attributes] (CA)
1. Acquisizione dei dati del profilo dalle origini dati del cliente all&#39;archivio profili [!DNL Target]
1. Raccogliere dati sul profilo e sul comportamento e decidere quale esperienza mostrare al visitatore
1. Le esperienze eseguono il rendering sulla pagina
1. at.js esegue il rendering delle esperienze sulla pagina

Ogni modello è costituito da parti diverse, ognuna delle quali corrisponde a un requisito di implementazione critico per l&#39;implementazione di [!DNL Target].

Ogni parte è spiegata in un articolo separato di questa guida:

* [Inizializzare gli SDK](/help/dev/patterns/recs-atjs/initialize-sdk.md)
* [Configurare la raccolta dati](/help/dev/patterns/recs-atjs/data-collection.md)
* [Esperienze di rendering](/help/dev/patterns/recs-atjs/render-experiences.md)
* [Notifica Target](/help/dev/patterns/recs-atjs/notify-target.md)
