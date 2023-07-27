---
keywords: lato server, lato server, api, sdk, node.js, nodejs, node js, recommendations api, api, api, api, lato server1
description: Scopri di più su [!DNL Adobe Target] API di distribuzione lato server, SDK e [!DNL Target Recommendations] API.
title: Dove posso saperne di più su [!DNL Target] API e SDK di distribuzione lato server?
feature: Implement Server-side
exl-id: 3eb0a789-cf1a-4d02-acf7-3c895bcb662f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 14%

---

# Lato server: implementare [!DNL Target]

Informazioni su [!DNL Adobe Target] API di distribuzione lato server, SDK e [!DNL Target Recommendations] API.

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

Link: [API di distribuzione lato server](/help/dev/implement/delivery-api/overview.md)

`/rest/v1/delivery`

Attraverso il [!DNL Target] API di consegna, puoi:

* Distribuisci esperienze su web, inclusi SPA, canali mobili e dispositivi IoT non basati su browser, come TV connesse, chioschi o schermi digitali in-store.
* Distribuisci esperienze da qualsiasi piattaforma o applicazione lato server in grado di effettuare chiamate HTTP/s.
* Fornisci esperienze coerenti e personalizzate a un visitatore, indipendentemente dal canale o dai dispositivi utilizzati dal visitatore per interagire con la tua azienda.
* Memorizza nella cache le esperienze di un visitatore all’interno di una sessione sul server, in modo da evitare più chiamate API e ottenere prestazioni migliori.
* Integrazione perfetta con i prodotti Adobe Experience Cloud, come Adobe Analytics, Adobe Audience Manager (AAM) e il servizio ID Experience Cloud dal lato server.

## SDK lato server

Il [!DNL Adobe Target] la documentazione dell’SDK lato server consente di implementare [!DNL Target] sui server nella lingua desiderata.

* [Node.js](node-js/overview.md)
* [Java](java/overview.md)
* [.NET](net/overview.md)
* [Python](python/overview.md)

Da a [!DNL Adobe Target]degli SDK lato server di, puoi:

* Esegui ed esegui **segnalazione di funzioni**, **rollout**, e **Esperimenti A/B** a **latenza prossima allo zero**.
* Distribuisci esperienze in **web**, tra cui **SPA**, e **canali mobili**, nonché non basati su browser **Dispositivi Internet of Things (IoT)** ad esempio un televisore, un chiosco o uno schermo digitale nel negozio.
* Consegna **Esperienze personalizzate basate sull’apprendimento automatico (ML)** a un utente, indipendentemente dal canale o dispositivo utilizzato dall’utente nella tua attività.
* **Integrazione perfetta con Adobe Experience Cloud** prodotti come **Adobe Analytics**, **Adobe Audience Manager** e **Servizio ID Experience Cloud** dal lato server.

Consulta la [Guida introduttiva](sdk-guides/getting-started/getting-started.md) per scoprire come eseguire un caso d’uso semplice di segnalazione di funzioni tramite [decisioning sul dispositivo](sdk-guides/on-device-decisioning/overview.md).

Consulta la nostra [App di esempio](sdk-guides/sample-apps/sample-apps.md) per divertirsi e giocare in giro!

## API di [!DNL Target Recommendations]

Link: [API Recommendations di Target](https://developers.adobetarget.com/api/recommendations) e [Panoramica API di Adobe Recommendations](../../before-administer/recs-api/overview.md).

Le API di Recommendations consentono di interagire in modo programmatico con [!DNL Target] server di consigli. Queste API possono essere integrate con una serie di applicazioni per eseguire funzioni che vengono normalmente eseguite mediante [!DNL Target] dell&#39;utente.
