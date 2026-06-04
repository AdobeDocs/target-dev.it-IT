---
keywords: implementare, at.js, libreria JavaScript
description: Scopri come distribuire la libreria JavaScript at.js di  [!DNL Adobe Target]  utilizzando i tag in [!DNL Adobe Experience Platform] o senza un gestore di tag.
title: Come si distribuisce at.js?
feature: Implement Server-side
exl-id: e62cb27e-ea80-462b-90f8-0a033b128031
TQID: https://experienceleague.adobe.com/V80R3Ds7eaUkkJazzCLK-tIePgqund6rMfQfLBZZvRQ
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ceid: f7c7de77-382f-4f48-8b36-61a170f06d3d
subfeature_v2: id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: bce87dde-a4ab-44c9-8a18-ad66e4ddb377id: d3cdead0-685a-4489-9250-4bb709942f66
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 287
ht-degree: 27%

---

# Come distribuire at.js

Informazioni su come distribuire la libreria JavaScript di [!DNL Adobe Target], at.js, utilizzando i tag in [!DNL Adobe Experience Platform] o senza un gestore di tag.

Puoi implementare at.js utilizzando i seguenti metodi:

* **[Implementare [!DNL Target] utilizzando i tag in Adobe Experience Platform](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md)**: i tag in [!DNL Adobe Experience Platform] rappresentano la nuova generazione di funzionalità di gestione tag di Adobe. I tag offrono ai clienti un modo semplice di implementare e gestire i tag di analisi, marketing e annunci pubblicitari necessari per fornire ai clienti esperienze personalizzate.

>[!NOTE]
>
> [!DNL Adobe Experience Platform Launch] è stato ridefinito come suite di tecnologie di raccolta dati in [!DNL Adobe Experience Platform]. Di conseguenza, sono state introdotte diverse modifiche terminologiche nella documentazione del prodotto. Per un riferimento consolidato delle modifiche terminologiche, consulta il seguente [documento](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html).

* **[Implementare [!DNL Target] senza un gestore di tag](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md)**: è possibile implementare [!DNL Target] senza utilizzare un gestore di tag (ad esempio, tag in [!DNL Adobe Experience Platform]).
* **Implementare [!DNL Target] utilizzando un gestore di tag di terze parti**: [I tag in Adobe Experience Platform](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md) sono il metodo preferito per implementare [!DNL Target]. Tuttavia, è possibile implementare [!DNL Target] utilizzando un gestore di tag di terze parti, inclusi Tealium, Ensighten e Google Tag. Per un elenco dei vantaggi dell&#39;utilizzo di Launch, consulta [Vantaggi dell&#39;implementazione di at.js con l&#39;estensione  [!DNL Adobe Target] ](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md#advantages-of-implementing-atjs-using-the-target-extension).

  Tuttavia, se sai come implementare [!DNL Target] senza un sistema per la gestione dei tag, puoi facilmente implementarlo con un sistema per la gestione dei tag di terze parti, invece di utilizzare l&#39;hard coding di at.js nel codice del sito.

  Di seguito sono riportati due argomenti rilevanti che ti aiuteranno a implementare [!DNL Target] con un gestore di tag di terze parti:

   * [Prima dell’implementazione](/help/dev/before-implement/prepare-to-implement-target.md)
   * [Implementa [!DNL Target] senza un sistema per la gestione dei tag](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md)

  Per ulteriori informazioni, consulta la documentazione del gestore di tag di terze parti.

Per implementare [!DNL Target] quando si utilizzano le app a pagina singola, vedere [Implementazione di un&#39;applicazione a pagina singola](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md).
