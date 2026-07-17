---
keywords: lato server, lato server, api, sdk, node.js, nodejs, node js, recommendations api, api, api, api, lato server1
description: Scopri le [!DNL Adobe Target] API di distribuzione lato server, SDK e [!DNL Target Recommendations] API.
title: Dove posso trovare informazioni su [!DNL Target] API e SDK di distribuzione lato server?
feature: Implement Server-side
exl-id: 3eb0a789-cf1a-4d02-acf7-3c895bcb662f
TQID: https://experienceleague.adobe.com/x5WKb9Eenz2bw-idOnxlpWdtiivTx05n38sNXEt3DNc
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: b050e0cd-2ddd-42cd-a71b-5d9e1fdf75e0id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2: id: a6cc21b9-1a36-4fa6-9c61-4acd04d9c88cid: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: c2be0313-b3ae-45e0-b454-d20bf54b23f2id: d095671a-1355-40aa-8b5f-06c33c68080bid: eb30f47f-d87a-400f-8f78-63ce7979ff56
source-git-commit: 45af56b5ac64eb1db67c1bfdfecd6887dce990ff
workflow-type: tm+mt
source-wordcount: 825
ht-degree: 9%

---

# Lato server: implementare [!DNL Target]

Informazioni su [!DNL Adobe Target] API di distribuzione lato server, SDK e [!DNL Target Recommendations] API.

>[!NOTE]
>
>Se l&#39;implementazione utilizza at.js e [!DNL AppMeasurement] sul lato client, è necessario utilizzare l&#39;[!UICONTROL API di distribuzione di Target] e gli SDK lato server descritti di seguito.
>
>Se l&#39;implementazione utilizza [!UICONTROL Adobe Experience Platform Web SDK], è necessario utilizzare [[!UICONTROL Adobe Experience Platform] [!UICONTROL Edge Network Server API]](https://experienceleague.adobe.com/en/docs/experience-platform/edge-network-server-api/overview){target=_blank}.

Il processo seguente si verifica in un’implementazione lato server di [!DNL Target]:

1. Un dispositivo client richiede un’esperienza attraverso il server.
1. Il server invia la richiesta a [!DNL Target].
1. [!DNL Target] restituisce la risposta al server.
1. Il server decide quale esperienza distribuire al dispositivo client affinché esegua il rendering.

Non è necessario che l’esperienza venga visualizzata in un browser. L’esperienza può essere visualizzata in un’e-mail o un chiosco, tramite un assistente vocale o tramite altre esperienze non visive o su un dispositivo non basato su browser. Poiché il server risiede tra il client e [!DNL Target], questo tipo di implementazione è ideale anche se hai bisogno di maggiore controllo e sicurezza oppure in presenza di processi di backend complessi che devono essere eseguiti sul server.

>[!NOTE]
>
>Un nuovo visitatore può essere inizializzato solo sul lato client. Un nuovo visitatore *non può* essere inizializzato sul lato server. Ciò è dovuto all’ECID, che dipende dal cookie demdex di terze parti e deve quindi essere inizializzato tramite API.js visitatore in un’implementazione in cui è coinvolto il browser.

Le sezioni seguenti forniscono ulteriori informazioni sulle varie API e SDK lato server:

## API di distribuzione lato server

Collegamento: [API di distribuzione lato server](/help/dev/implement/delivery-api/overview.md)

`/rest/v1/delivery`

Tramite l&#39;API di consegna [!DNL Target], puoi:

* Distribuisci esperienze su web, incluse applicazioni a pagina singola, canali mobili e dispositivi IoT non basati su browser, come TV connesse, chioschi o schermi digitali in-store.
* Distribuisci esperienze da qualsiasi piattaforma o applicazione lato server in grado di effettuare chiamate HTTP/s.
* Fornisci esperienze coerenti e personalizzate a un visitatore, indipendentemente dal canale o dai dispositivi utilizzati dal visitatore per interagire con la tua azienda.
* Memorizza nella cache le esperienze di un visitatore all’interno di una sessione sul server, in modo da evitare più chiamate API e ottenere prestazioni migliori.
* Integrazione perfetta con i prodotti Adobe Experience Cloud, come Adobe Analytics, Adobe Audience Manager (AAM) e il servizio Experience Cloud ID dal lato server.

## SDK lato server

La documentazione di SDK lato server di [!DNL Adobe Target] ti aiuta a implementare [!DNL Target] sui server nella tua lingua.

* [Node.js](node-js/overview.md)
* [Java](java/overview.md)
* [.NET](net/overview.md)
* [Python](python/overview.md)

Tramite gli SDK lato server di [!DNL Adobe Target] puoi:

* Esegui ed esegui **esperimenti di contrassegno di funzionalità**, **rollout** ed **esperimenti A/B** con **latenza prossima allo zero**.
* Distribuisci esperienze su **web**, inclusi **SPA** e **canali mobili**, nonché su dispositivi **Internet of Things (IoT) non basati su browser**, ad esempio TV connessa, chiosco o schermo digitale in-store.
* Distribuisci **esperienze personalizzate basate su Machine Learning (ML)** a un utente, indipendentemente dal canale o dal dispositivo utilizzato dall&#39;utente nella tua azienda.
* **Integrazione perfetta con i prodotti Adobe Experience Cloud** come **Adobe Analytics**, **Adobe Audience Manager** e **Experience Cloud ID Service** dal lato server.

Consulta la pagina [Guida introduttiva](sdk-guides/getting-started/getting-started.md) per scoprire come eseguire un semplice caso d&#39;uso di segnalazione di funzionalità tramite [decisioning sul dispositivo](sdk-guides/on-device-decisioning/overview.md).

Controlla le [app di esempio](sdk-guides/sample-apps/sample-apps.md) per divertirti e divertirti.

## API di [!DNL Target Recommendations]

Collegamento: [API di Target Recommendations](https://developers.adobetarget.com/api/recommendations) e [Panoramica API di Adobe Recommendations](../../before-administer/recs-api/overview.md).

Le API di Recommendations consentono di interagire in modo programmatico con [!DNL Target] server di consigli. Queste API possono essere integrate con una serie di applicazioni per eseguire funzioni che vengono normalmente eseguite tramite l&#39;interfaccia utente [!DNL Target].

## [!DNL Platform Edge Network] chiamate API senza un SDK {#platform-edge-api-user-agent}

[!UICONTROL Adobe Experience Platform Web SDK] e altre integrazioni SDK supportate includono un valore `User-Agent` simile al browser nelle intestazioni della richiesta HTTP quando si chiama [!DNL Experience Platform Edge Network]. Le integrazioni lato server che utilizzano l&#39;API [Interact](https://experienceleague.adobe.com/en/docs/experience-platform/edge-network/server-api/interact){target=_blank} pubblica senza un SDK devono fornire esplicitamente questa intestazione.

Per le chiamate API di interazione non SDK, osserva i seguenti requisiti:

* Includi un `User-Agent` valido simile a quello del browser nelle intestazioni della richiesta HTTP. Un valore visitatore o agente utente nel corpo della richiesta JSON da solo non soddisfa i requisiti di rilevamento di bot per questo modello di integrazione.
* Non utilizzare valori segnaposto o non del browser, ad esempio `MyApp/1.0`, che possono causare la classificazione bot.
* Per le chiamate API pubbliche di Edge non è necessario un nome SDK o una versione SDK. Per questo scenario, l&#39;elemento obbligatorio è un&#39;intestazione HTTP `User-Agent` valida.

Quando [!DNL Target] classifica una richiesta come traffico da bot, la personalizzazione può non riuscire o apparire intermittente perché la ricerca di profili, la valutazione dei segmenti e i contenuti personalizzati per attività come [!UICONTROL Recommendations] e [!UICONTROL Auto-Target] sono soppressi, come descritto di seguito.

Ulteriori informazioni sull&#39;implementazione con SDK nella [[!DNL Adobe Experience Platform Web SDK] panoramica](https://experienceleague.adobe.com/en/docs/target-dev/developer/client-side/aep/aep-web-sdk-overview){target=_blank}.

**Esempio di richiesta Interact API (le intestazioni devono includere `User-Agent`):**

```http
POST https://edge.adobedc.net/ee/v2/interact?dataStreamId=YOUR_DATASTREAM_ID&requestId=YOUR_REQUEST_ID
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/26.5 Safari/605.1.15
Accept: */*
Content-Type: text/plain; charset=UTF-8
```
