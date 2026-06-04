---
title: Registrazione lato server per i dati A4T in Experience Platform Web SDK
description: Scopri come abilitare la registrazione lato server per Adobe Analytics for Target (A4T) utilizzando Experience Platform Web SDK.
seo-title: Server-side logging for A4T data in Experience Platform Web SDK
seo-description: Learn how to enable server-side logging for Adobe Analytics for Target (A4T) using the Experience Platform Web SDK.
keywords: a4t;target;web;sdk;platform;logging;
feature: Implementation
exl-id: 716f7343-69a6-44d7-baec-a0a0df1b6e1f
TQID: https://experienceleague.adobe.com/I7-G2VO2AN3qFsgkk4JX2Pg6WJfZq0HZkcGL4XQNoWg
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 161
ht-degree: 0%

---

# Registrazione lato server per dati A4T in [!DNL Experience Platform Web SDK]

[!DNL Adobe Experience Platform Web SDK] consente di implementare la funzionalità [!UICONTROL Adobe Analytics for Target] (A4T) in [!UICONTROL Experience Platform Edge Network]. Quando la registrazione lato server è abilitata, tutti gli hit [!DNL Analytics] inviati tramite Edge Network vengono aumentati con [!DNL Target] dettagli sul lato server, senza dover passare attraverso il processo di unione degli hit.

La registrazione lato server per [!DNL Analytics] è abilitata quando [!DNL Analytics] è abilitato nella configurazione dello stream di dati:

![Configurazione dello stream di dati di Analytics abilitata](/help/dev/implement/a4t/assets/enable-analytics-datastream.png)

Il diagramma seguente mostra il flusso dei dati nel sistema quando è abilitata la registrazione [!DNL Analytics] lato server:

![Flusso di registrazione lato server](/help/dev/implement/a4t/assets/analytics-server-side-logging.png)

## Passaggi successivi

Questa guida tratta la registrazione lato server dei dati A4T nel Web SDK. Per ulteriori informazioni su come gestire i dati A4T sul lato client, consulta la guida alla [registrazione lato client](/help/dev/implement/a4t/client-side-logging.md).
