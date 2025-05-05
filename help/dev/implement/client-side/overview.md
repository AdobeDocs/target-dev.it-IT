---
keywords: implementazione, at.js, adobe experience platform web sdk, aep web sdk
description: Scopri come implementare  [!DNL Adobe Target] per il Web lato client utilizzando  [!DNL Adobe Experience Platform Web SDK] (AEP Web SDK) o la libreria JavaScript at.js.
title: Come si implementa  [!DNL Target] per il Web lato client
feature: at.js
exl-id: b3a850ff-ace0-4eea-955a-aa71dfad256f
source-git-commit: 2d2a593df661c7e6c6e6384af6042e8aa4575fdb
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 28%

---

# Panoramica: implementa [!DNL Target] per web lato client

In un’implementazione lato client di [!DNL Adobe Target], [!DNL Target] distribuisce le esperienze associate a un’attività direttamente al browser client. Il browser determina quale esperienza visualizzare e la visualizza. Con un’implementazione lato client, puoi utilizzare un editor WYSIWYG, il [Compositore esperienza visivo](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html?lang=it) o un’interfaccia non visiva, il [Compositore esperienza basato su moduli](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?lang=it), per creare esperienze di attività e personalizzazione.

Per implementare [!DNL Target] lato client, è necessario utilizzare una delle seguenti librerie JavaScript:

* [Adobe Experience Platform Web SDK](/help/dev/implement/client-side/aep-web-sdk.md)

  [!UICONTROL Adobe Experience Platform Web SDK] consente di interagire con i vari servizi in [!DNL Adobe Experience Cloud] (incluso [!DNL Target]) tramite [!UICONTROL Adobe Experience Edge Network]. Se scegli di migrare a [!UICONTROL Adobe Experience Platform Web SDK], vedi [Che cos&#39;è [!UICONTROL Adobe Experience Platform Web SDK]](/help/dev/implement/client-side/aep-web-sdk.md).

* [Libreria JavaScript at.js di [!DNL Target]](/help/dev/implement/client-side/atjs/how-atjs-works/overview.md)

  La libreria JavaScript at.js migliora i tempi di caricamento delle pagine per le implementazioni web, migliora la sicurezza e fornisce migliori opzioni di implementazione per le applicazioni a pagina singola. Se scegli di migrare a at.js, consulta [Funzionamento di at.js](/help/dev/implement/client-side/atjs/how-atjs-works/overview.md) e [[!DNL Adobe Target] Skill Builder: chat per sviluppatori, migrazione di Adobe Target mbox.js in at.js](https://seminars.adobeconnect.com/ptdo6mfo6qn6/?proto=true).


Vedi [Confronto della libreria at.js con Web SDK](https://experienceleague.adobe.com/it/docs/experience-platform/web-sdk/personalization/adobe-target/web-sdk-atjs-comparison){target=_blank} per informazioni sulle differenze tra i due approcci di implementazione.