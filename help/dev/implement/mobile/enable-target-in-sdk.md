---
keywords: app mobile, sdk app mobile, target app mobile, sdk target mobile, sdk app mobile, attivare target in sdk
description: Scopri come aggiungere l’SDK di Mobile Services di Adobe alla tua app mobile.
title: Come posso abilitare  [!DNL Target]  in  [!DNL Adobe Mobile SDK]?
feature: Implement Mobile
exl-id: 4263b96a-23c8-4513-8302-00080122181d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '274'
ht-degree: 29%

---

# Abilita [!DNL Target] nell&#39;SDK

Aggiungi [!UICONTROL Adobe Mobile Services SDK] all&#39;app.

>[!IMPORTANT]
>
>Supporto per [!DNL Adobe Mobile] versione 4.*x* SDK è terminato il 31 agosto 2021 e non è più consigliato per [!DNL Adobe Target] utenti di dispositivi mobili.
>
>L&#39;SDK [Adobe Experience Platform per le app mobili](https://developer.adobe.com/client-sdks/documentation/){target=_blank} è la soluzione consigliata per alimentare le soluzioni e i servizi [!DNL Adobe Experience Cloud] nelle app mobili.

1. Se non hai installato l&#39;SDK di Adobe Mobile Services nell&#39;app, utilizza le credenziali di Analytics o Experience Cloud e scarica l&#39;SDK dal sito Web di [Adobe Mobile Services](https://mobilemarketing.adobe.com/).

1. Aggiungi [!DNL Adobe Mobile Services SDK] all&#39;app.

   È possibile trovare le istruzioni alla voce [Implementazione principale e Ciclo di durata](https://experienceleague.adobe.com/docs/mobile-services/ios/getting-started-ios/dev-qs.html).

1. Aggiungere il codice client, timeout e abilitare SSL.

   Nell&#39;Experience Cloud, apri Mobile Services, quindi vai a **[!UICONTROL Manage App Settings]** > **[!UICONTROL SDK Target Options]**.

   Aggiungi il tuo codice client [!DNL Target] e timeout. Il codice client è univoco per il tuo account o azienda. Il timeout è il tempo, in numero di secondi, fino al quale [!DNL Target] attenderà una risposta prima di mostrare il contenuto predefinito. Verificare che l&#39;opzione **[!UICONTROL Use HTTPS]** sia selezionata nella pagina Gestione impostazioni app di Adobe Mobile Services. Se HTTPS non è abilitato, tutte le chiamate in iOS9+ verranno bloccate a meno che non venga inserito nell&#39;elenco Consentiti il server [!DNL Target].

   ![Alt immagine](assets/mobile-clientcode.png)

1. Dopo aver creato/individuato l&#39;app, individua le impostazioni dell&#39;app e scarica l&#39;SDK desiderato.

   ![Alt immagine](assets/download-sdk.png)

>[!WARNING]
>
> Se non hai accesso all’interfaccia di mobile marketing, puoi apportare modifiche direttamente nel file di configurazione nel codice dell’app. Tuttavia, non sarà sincronizzato con la pagina delle impostazioni nell’interfaccia utente.
