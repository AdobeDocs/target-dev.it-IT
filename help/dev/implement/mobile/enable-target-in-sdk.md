---
keywords: app mobile, sdk app mobile, target app mobile, sdk target mobile, sdk app mobile, attivare target in sdk
description: Scopri come aggiungere il SDK di Adobe Mobile Services alla tua app mobile.
title: Come posso abilitare [!DNL Target] in [!DNL Adobe Mobile SDK]?
feature: Implement Mobile
exl-id: 4263b96a-23c8-4513-8302-00080122181d
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
    internal-label: Target
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
    internal-label: Implementation
subfeature_v2:
  - id: d051910f-2bda-47ea-a969-6ade9fcd71f1
    internal-label: Implement mobile
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
source-git-commit: 5d119ccf18b09b3ba864a69642458597f65c754f
workflow-type: tm+mt
source-wordcount: '303'
ht-degree: 38%
---
# Abilita [!DNL Target] in SDK

Aggiungi [!UICONTROL Adobe Mobile Services SDK] alla tua app.

>[!IMPORTANT]
>
>Il supporto per gli SDK [!DNL Adobe Mobile] versione 4.*x* è terminato il 31 agosto 2021 e non è più consigliato per gli utenti di dispositivi mobili [!DNL Adobe Target].
>
>[Adobe Experience Platform SDK for Mobile Apps](https://developer.adobe.com/client-sdks/documentation/){target=_blank} è la soluzione consigliata per alimentare le soluzioni e i servizi [!DNL Adobe Experience Cloud] nelle app mobili.

1. Se non hai installato Adobe Mobile Services SDK nell&#39;app, utilizza le credenziali di Analytics o Experience Cloud e scarica SDK dal sito Web di [Adobe Mobile Services](https://mobilemarketing.adobe.com/).

1. Aggiungi [!DNL Adobe Mobile Services SDK] all&#39;app.

   È possibile trovare le istruzioni alla voce [Implementazione principale e Ciclo di durata](https://experienceleague.adobe.com/docs/mobile-services/ios/getting-started-ios/dev-qs.html).

1. Aggiungere il codice client, timeout e abilitare SSL.

   In Experience Cloud, apri Mobile Services, quindi vai su **[!UICONTROL Gestisci impostazioni app]** > **[!UICONTROL Opzioni Target SDK]**.

   Aggiungi il tuo codice client [!DNL Target] e timeout. Il codice client è univoco per il tuo account o azienda. Il timeout è il tempo, in numero di secondi, fino al quale [!DNL Target] attenderà una risposta prima di mostrare il contenuto predefinito. Assicurati che l&#39;opzione **[!UICONTROL Usa HTTPS]** sia selezionata nella pagina Gestisci impostazioni app di Adobe Mobile Services. Se HTTPS non è abilitato, tutte le chiamate in iOS9+ verranno bloccate a meno che non si inserisce nell&#39;elenco Consentiti il server [!DNL Target].

   ![Alt immagine](assets/mobile-clientcode.png)

1. Dopo aver creato/individuato l&#39;app, individua le impostazioni dell&#39;app e scarica il SDK desiderato.

   ![Alt immagine](assets/download-sdk.png)

>[!WARNING]
>
> Se non hai accesso all’interfaccia di mobile marketing, puoi apportare modifiche direttamente nel file di configurazione nel codice dell’app. Tuttavia, non sarà sincronizzato con la pagina delle impostazioni nell’interfaccia utente.
