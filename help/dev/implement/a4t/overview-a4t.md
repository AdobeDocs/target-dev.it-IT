---
title: Registrazione di Adobe Analytics for Target (A4T) in Experience Platform Web SDK
description: Scopri come controllare la raccolta di dati di Adobe Analytics for Target (A4T) utilizzando Experience Platform Web SDK.
seo-title: Adobe Analytics for Target (A4T) Logging in the Experience Platform Web SDK
seo-description: Learn how to control the collection of Adobe Analytics for Target (A4T) data using the Experience Platform Web SDK.
keywords: a4t;registrazione;analytics;sdk;web sdk;
feature: Implementation
source-git-commit: b7638f7ab3fe9a6551c9d542a990e22ddb2b27a2
workflow-type: tm+mt
source-wordcount: '235'
ht-degree: 0%

---

# Registrazione di [!DNL Adobe Analytics for Target] (A4T) in [!DNL Experience Platform Web SDK]

Quando si utilizza [!DNL Adobe Target] per la personalizzazione, è possibile scegliere il sistema da utilizzare per la misurazione delle prestazioni. Ogni [attività Target](https://experienceleague.adobe.com/docs/target/using/activities/target-activities-guide.html?lang=it) ti consente di selezionare tra [!DNL Target] rapporti e rapporti di Adobe [!DNL Analytics].

Se utilizzi il reporting di [!DNL Analytics], [!DNL Target] deve comunicare quanto segue a [!DNL Analytics]:

* Quale attività [!DNL Target] sono state immesse dai visitatori
* Quale esperienza hanno visto
* Quale conversione è stata raggiunta

[!DNL Adobe Experience Platform Web SDK] supporta due tipi di registrazione di [!DNL Analytics] per [!UICONTROL Analytics for Target] (A4T) casi d&#39;uso:

| Metodo di registrazione | Descrizione |
| --- | --- |
| Registrazione [!DNL Analytics] lato server | Tutti gli hit [!DNL Analytics] inviati tramite Edge Network sono aumentati con [!DNL Target] dettagli sul lato server, senza dover passare attraverso il processo di unione degli hit. |
| Registrazione [!DNL Analytics] lato client | I dati di [!DNL Target] vengono restituiti sul lato client, consentendo di aumentare e inviare manualmente i dati a [!DNL Analytics] utilizzando [Data Insertion API](https://experienceleague.adobe.com/docs/analytics/import/c-data-insertion-api.html?lang=it). |

Il metodo di registrazione dipende dal fatto che [!DNL Adobe Analytics] sia abilitato nel [flusso di dati](https://experienceleague.adobe.com/it/docs/experience-platform/datastreams/overview) configurato:

![Flusso di decisione del metodo di registrazione](/help/dev/implement/a4t/assets/analytics-logging.png)

## Passaggi successivi

Questo documento fornisce una breve introduzione ai diversi metodi di registrazione per i dati A4T nel Web SDK. Per informazioni più dettagliate su ciascuno di questi metodi, consulta la seguente documentazione:

* [Registrazione lato server per i dati A4T in Experience Platform Web SDK](/help/dev/implement/a4t/client-side-logging.md)
* [Registrazione lato client dei dati A4T in Experience Platform Web SDK](/help/dev/implement/a4t/client-side-logging.md)