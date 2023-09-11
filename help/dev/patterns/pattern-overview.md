---
title: Panoramica sui modelli di implementazione
description: Come utilizzare i modelli di implementazione
feature: APIs/SDKs
level: Experienced
role: Developer
hide: true
hidefromtoc: true
source-git-commit: e15513f5c52240536ccf41f16ba7f4dc6dbf9a04
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 0%

---

# Panoramica sui modelli di implementazione

[!DNL Adobe Target] I modelli di implementazione consentono di comprendere e creare la seguente architettura per [!DNL Target] implementazione.

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

Ogni parte viene illustrata in un argomento separato di questa guida. Ad esempio, il [[!DNL Recommendations] pattern di implementazione utilizzando at.js](/help/dev/patterns/recs-atjs/recs-implementation-pattern-atjs.md) contiene i seguenti argomenti:

* Inizializza SDK
* Configurare la raccolta dati
* Esperienze di rendering
* Notifica [!DNL Target]

