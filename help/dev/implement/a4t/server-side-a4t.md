---
title: Registrazione lato server per i dati A4T in Experience Platform Web SDK
description: Scopri come abilitare la registrazione lato server per Adobe Analytics for Target (A4T) utilizzando Experience Platform Web SDK.
seo-title: Server-side logging for A4T data in Experience Platform Web SDK
seo-description: Learn how to enable server-side logging for Adobe Analytics for Target (A4T) using the Experience Platform Web SDK.
keywords: a4t;target;web;sdk;platform;logging;
feature: Implementation
source-git-commit: b1b0424bfe61fb8b4e88723e6bb2c565d75f8351
workflow-type: tm+mt
source-wordcount: '144'
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