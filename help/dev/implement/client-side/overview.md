---
keywords: implementazione, at.js, adobe experience platform web sdk, aep web sdk
description: Scopri come implementare [!DNL Adobe Target] per web lato client utilizzando  [!DNL Adobe Experience Platform Web SDK] (AEP Web SDK) o la libreria JavaScript at.js.
title: Come si implementa  [!DNL Target] per il Web lato client
feature: at.js
exl-id: b3a850ff-ace0-4eea-955a-aa71dfad256f
TQID: https://experienceleague.adobe.com/KgJyhvTguS8EXbwELaApI1mcs5egnEKHKpnxVYGqT4I
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2:
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 929e1f10bc5dd0741f0fe28cd46435e680a4a308
workflow-type: tm+mt
source-wordcount: 252
ht-degree: 26%

---

# Panoramica: implementa [!DNL Target] per web lato client

In un’implementazione lato client di [!DNL Adobe Target], [!DNL Target] distribuisce le esperienze associate a un’attività direttamente al browser client. Il browser determina quale esperienza visualizzare e la visualizza. Con un’implementazione lato client, puoi utilizzare un editor WYSIWYG, il [Compositore esperienza visivo](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html) o un’interfaccia non visiva, il [Compositore esperienza basato su moduli](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html), per creare esperienze di attività e personalizzazione.

Per implementare [!DNL Target] lato client, è necessario utilizzare una delle seguenti librerie JavaScript:

* [Adobe Experience Platform Web SDK](/help/dev/implement/client-side/aep-web-sdk/aep-web-sdk-overview.md)

  [!UICONTROL Adobe Experience Platform Web SDK] consente di interagire con i vari servizi in [!DNL Adobe Experience Cloud] (incluso [!DNL Target]) tramite [!UICONTROL Adobe Experience Edge Network]. Se si sceglie di eseguire la migrazione a [!UICONTROL Adobe Experience Platform Web SDK], vedere [Che cos&#39;è [!UICONTROL Adobe Experience Platform Web SDK]](/help/dev/implement/client-side/aep-web-sdk/aep-web-sdk-overview.md).

* [Libreria JavaScript at.js di [!DNL Target]](/help/dev/implement/client-side/atjs/how-atjs-works/how-atjs-works.md)

  La libreria JavaScript at.js migliora i tempi di caricamento delle pagine per le implementazioni web, migliora la sicurezza e fornisce migliori opzioni di implementazione per le applicazioni a pagina singola. Se scegli di migrare a at.js, consulta [Funzionamento di at.js](/help/dev/implement/client-side/atjs/how-atjs-works/overview.md) e [[!DNL Adobe Target] Skill Builder: chat per sviluppatori, migrazione di Adobe Target mbox.js in at.js](https://seminars.adobeconnect.com/ptdo6mfo6qn6/?proto=true).


Vedi [Confronto della libreria at.js con Web SDK](/help/dev/implement/client-side/aep-web-sdk/web-sdk-atjs-comparison.md) per informazioni sulle differenze tra i due approcci di implementazione.
