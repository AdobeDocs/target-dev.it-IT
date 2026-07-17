---
title: Panoramica dell’API di consegna di Adobe Target
description: Panoramica dell’API di consegna di Adobe Target
keywords: API Delivery
exl-id: e760bddc-b1ae-4b7b-bff2-aba81c6b6d34
feature: APIs/SDKs
TQID: https://experienceleague.adobe.com/gPXGax6ccvZZPklT3jnZbqyOj3mCClEfSpdufAFPtSs
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: b6b447ccb88925a8efb6ff6a80ae475c8780dbc8
workflow-type: tm+mt
source-wordcount: 244
ht-degree: 0%

---

# Panoramica dell’API di consegna

[!DNL Adobe Target Delivery API] è basato su REST. Questa documentazione descrive le risorse che compongono [!DNL Adobe Target] [!DNL Delivery API]. I metodi HTTP vengono utilizzati per eseguire operazioni su tali risorse.

>[!IMPORTANT]
>
>Il [!DNL Delivery API] qui documentato è destinato a [!DNL at.js] e alle implementazioni dirette lato server. Se stai implementando [!DNL Target] utilizzando [!DNL Adobe Experience Platform Web SDK], utilizza l&#39;API Interact, a cui si accede tramite il comando `sendEvent` su [!UICONTROL Experience Platform Edge Network], invece di chiamare direttamente [!DNL Delivery API]. Consulta [Adobe Experience Platform Web SDK](/help/dev/implement/client-side/aep-web-sdk/aep-web-sdk-overview.md) e [Per ulteriori informazioni, confronta la libreria at.js con Experience Platform Web SDK](/help/dev/implement/client-side/aep-web-sdk/web-sdk-atjs-comparison.md).

Utilizzando l&#39;API di consegna di [!UICONTROL Adobe Target], puoi:

* Distribuisci esperienze su web, incluse applicazioni a pagina singola, canali mobili e dispositivi IoT non basati su browser, ad esempio TV connessa, chiosco o schermi digitali in-store.
* Distribuisci esperienze da qualsiasi piattaforma o applicazione lato server in grado di effettuare chiamate HTTP/s.
* Fornisci esperienze coerenti e personalizzate a un utente, indipendentemente dal canale o dai dispositivi coinvolti dall’utente nella tua attività.
* Memorizza nella cache le esperienze di un utente all’interno di una sessione nel server in modo da evitare più chiamate API e, di conseguenza, ottenere prestazioni migliori.
* Integrazione perfetta con [!DNL Adobe Experience Cloud] prodotti come [!DNL Adobe Analytics], [!DNL Adobe Audience Manager] e [!DNL Experience Cloud ID Service] dal lato server.

>[!NOTE]
>
>Puoi comunque accedere alla [documentazione legacy /v1/mbox e /v2/batchmbox API](https://developers.adobetarget.com/api/legacy-api/index.html). Tuttavia, nell’API di consegna verranno sviluppate delle funzioni (come documentato qui) che non verranno supportate nelle API legacy.
