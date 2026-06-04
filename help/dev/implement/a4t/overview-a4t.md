---
title: Registrazione di Adobe Analytics for Target (A4T) in Experience Platform Web SDK
description: Scopri come controllare la raccolta di dati di Adobe Analytics for Target (A4T) utilizzando Experience Platform Web SDK.
seo-title: Adobe Analytics for Target (A4T) Logging in the Experience Platform Web SDK
seo-description: Learn how to control the collection of Adobe Analytics for Target (A4T) data using the Experience Platform Web SDK.
keywords: a4t;registrazione;analytics;sdk;web sdk;
feature: Implementation
exl-id: 886588bf-4416-4f1e-aecc-467e48c8fb23
TQID: https://experienceleague.adobe.com/cShqvj3wSialxA-ajnROnIpzjuz66pNg3CLM6l2xPLg
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2: id: a94ced60-8199-4549-b453-ede2acb4101e
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: aa2f3246-cb95-4b30-8899-fdf7d73550ccid: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: c2be0313-b3ae-45e0-b454-d20bf54b23f2id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 265
ht-degree: 0%

---

# Registrazione di [!DNL Adobe Analytics for Target] (A4T) in [!DNL Experience Platform Web SDK]

Quando si utilizza [!DNL Adobe Target] per la personalizzazione, è possibile scegliere il sistema da utilizzare per la misurazione delle prestazioni. Ogni [attività Target](https://experienceleague.adobe.com/docs/target/using/activities/target-activities-guide.html) ti consente di selezionare tra [!DNL Target] rapporti e rapporti di Adobe [!DNL Analytics].

Se utilizzi il reporting di [!DNL Analytics], [!DNL Target] deve comunicare quanto segue a [!DNL Analytics]:

* Quale attività [!DNL Target] sono state immesse dai visitatori
* Quale esperienza hanno visto
* Quale conversione è stata raggiunta

[!DNL Adobe Experience Platform Web SDK] supporta due tipi di registrazione di [!DNL Analytics] per [!UICONTROL casi di utilizzo di Analytics for Target] (A4T):

| Metodo di registrazione | Descrizione |
| --- | --- |
| Registrazione [!DNL Analytics] lato server | Tutti gli hit [!DNL Analytics] inviati tramite Edge Network sono aumentati con [!DNL Target] dettagli sul lato server, senza dover passare attraverso il processo di unione degli hit. |
| Registrazione [!DNL Analytics] lato client | I dati di [!DNL Target] vengono restituiti sul lato client, consentendo di aumentare e inviare manualmente i dati a [!DNL Analytics] utilizzando [Data Insertion API](https://experienceleague.adobe.com/docs/analytics/import/c-data-insertion-api.html). |

Il metodo di registrazione dipende dal fatto che [!DNL Adobe Analytics] sia abilitato nel [flusso di dati](https://experienceleague.adobe.com/en/docs/experience-platform/datastreams/overview) configurato:

![Flusso di decisione del metodo di registrazione](/help/dev/implement/a4t/assets/analytics-logging.png)

## Passaggi successivi

Questo documento fornisce una breve introduzione ai diversi metodi di registrazione per i dati A4T nel Web SDK. Per informazioni più dettagliate su ciascuno di questi metodi, consulta la seguente documentazione:

* [Registrazione lato server per i dati A4T in Experience Platform Web SDK](/help/dev/implement/a4t/client-side-logging.md)
* [Registrazione lato client dei dati A4T in Experience Platform Web SDK](/help/dev/implement/a4t/client-side-logging.md)
