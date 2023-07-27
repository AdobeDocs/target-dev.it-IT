---
keywords: offerta, preacquisizione, iOS, android, sdk, mobile, mobile sdk, $8
description: Utilizza il [!DNL Adobe Target] la funzione di preacquisizione preventiva negli SDK mobili per iOS e Android consente di recuperare il contenuto delle offerte il minor numero di volte possibile, memorizzando nella cache le risposte dal server.
title: Posso prerecuperare il contenuto delle offerte per le app mobili?
feature: Implement Mobile
exl-id: 6f8e8298-f1e9-46f0-828f-717c7d632077
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '296'
ht-degree: 39%

---

# Preacquisire il contenuto dell’offerta

La funzione di preacquisizione di [!DNL Target] utilizza gli SDK per dispositivi mobili iOS e Android per recuperare il contenuto delle offerte il minor numero di volte possibile, memorizzando nella cache le risposte dal server.

>[!IMPORTANT]
>
>Supporto per [!DNL Adobe Mobile] versione 4.*x* SDK è terminato il 31 agosto 2021 e non è più consigliato per [!DNL Adobe Target] utenti di dispositivi mobili.
>
>Il [SDK di Adobe Experience Platform per le app mobili](https://developer.adobe.com/client-sdks/documentation/){target=_blank} è la soluzione consigliata per [!DNL Adobe Experience Cloud] soluzioni e servizi nelle app mobili.

Questo processo consente di ridurre il tempo di caricamento, evita l’esecuzione di più chiamate di rete e permette di notificare ad [!DNL Target] quale elemento mbox è stato visitato dall’utente dell’app mobile. Tutto il contenuto viene recuperato e memorizzato in cache durante la chiamata di preacquisizione, e da qual momento viene richiamato dalla cache per tutte le chiamate future che includono quel contenuto per il nome di mbox specificato.

Quando utilizzi il metodo di preacquisizione con gli SDK mobili per iOS e Android, considera le seguenti limitazioni:

* Il contenuto di preacquisizione non rimane tra un avvio dell’app e quello successivo. Viene memorizzato nella cache per tutto il tempo in cui l’app rimane attiva oppure fino alla chiamata del metodo `clearPrefetchCache()`.
* La funzionalità di preacquisizione non è supportata per [!UICONTROL Allocazione automatica] e [!UICONTROL Targeting automatico] metodi di allocazione del traffico, per [!UICONTROL Automated Personalization] o [!UICONTROL Recommendations] tipi di attività o per [offerte di consigli all’interno di un’attività A/B o XT](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-as-an-offer.html).

Per ulteriori informazioni, inclusi i metodi di preacquisizione, le classi pubbliche e gli esempi di codice, vedi:

* **iOS:**  [Preacquisizione del contenuto delle offerte in iOS](https://experienceleague.adobe.com/docs/mobile-services/ios/target-ios/c-mob-target-prefetch-ios.html) nel *Guida all’SDK di Mobile Services per iOS*.
* **Android:**  [Preacquisizione del contenuto delle offerte in Android](https://experienceleague.adobe.com/docs/mobile-services/android/target-android/c-mob-target-prefetch-android.html) nel *Guida dell’SDK per Android di Mobile Services*.
