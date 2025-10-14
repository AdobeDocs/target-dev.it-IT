---
keywords: implementare, at.js, libreria JavaScript
description: Scopri come distribuire la libreria JavaScript at.js di  [!DNL Adobe Target]  utilizzando i tag in [!DNL Adobe Experience Platform] o senza un gestore di tag.
title: Come si distribuisce at.js?
feature: Implement Server-side
exl-id: e62cb27e-ea80-462b-90f8-0a033b128031
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 28%

---

# Come distribuire at.js

Informazioni su come distribuire la libreria JavaScript di [!DNL Adobe Target], at.js, utilizzando i tag in [!DNL Adobe Experience Platform] o senza un gestore di tag.

Puoi implementare at.js utilizzando i seguenti metodi:

* **[Implementare [!DNL Target] utilizzando i tag in Adobe Experience Platform](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md)**: i tag in [!DNL Adobe Experience Platform] sono la nuova generazione di funzionalità di gestione tag di Adobe. I tag offrono ai clienti un modo semplice di implementare e gestire i tag di analisi, marketing e annunci pubblicitari necessari per fornire ai clienti esperienze personalizzate.

>[!NOTE]
>
> [!DNL Adobe Experience Platform Launch] è stato ridefinito come suite di tecnologie di raccolta dati in [!DNL Adobe Experience Platform]. Di conseguenza, sono state introdotte diverse modifiche terminologiche nella documentazione del prodotto. Per un riferimento consolidato delle modifiche terminologiche, consulta il seguente [documento](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html?lang=it).

* **[Implementare [!DNL Target] senza un gestore di tag](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md)**: è possibile implementare [!DNL Target] senza utilizzare un gestore di tag (ad esempio, tag in [!DNL Adobe Experience Platform]).
* **Implementare [!DNL Target] utilizzando un gestore di tag di terze parti**: [I tag in Adobe Experience Platform](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md) sono il metodo preferito per implementare [!DNL Target]. Tuttavia, è possibile implementare [!DNL Target] utilizzando un gestore di tag di terze parti, inclusi Tealium, Ensighten e Google Tag. Per un elenco dei vantaggi dell&#39;utilizzo di Launch, consulta [Vantaggi dell&#39;implementazione di at.js con l&#39;estensione  [!DNL Adobe Target] &#x200B;](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md#advantages-of-implementing-atjs-using-the-target-extension).

  Tuttavia, se sai come implementare [!DNL Target] senza un sistema per la gestione dei tag, puoi facilmente implementarlo con un sistema per la gestione dei tag di terze parti, invece di utilizzare l&#39;hard coding di at.js nel codice del sito.

  Di seguito sono riportati due argomenti rilevanti che ti aiuteranno a implementare [!DNL Target] con un gestore di tag di terze parti:

   * [Prima dell’implementazione](/help/dev/before-implement/prepare-to-implement-target.md)
   * [Implementa [!DNL Target] senza un sistema per la gestione dei tag](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md)

  Per ulteriori informazioni, consulta la documentazione del gestore di tag di terze parti.

Per implementare [!DNL Target] quando si utilizzano le app a pagina singola (SPA), vedere [Implementazione dell&#39;applicazione a pagina singola](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md).
