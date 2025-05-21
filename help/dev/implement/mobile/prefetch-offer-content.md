---
keywords: offerta, preacquisizione, iOS, android, sdk, mobile, mobile sdk, $8
description: Utilizza la funzione di preacquisizione di  [!DNL Adobe Target]  negli SDK iOS e Android Mobile per recuperare il contenuto delle offerte il minor numero di volte possibile, memorizzando nella cache le risposte dal server.
title: Posso prerecuperare il contenuto delle offerte per le app mobili?
feature: Implement Mobile
exl-id: 6f8e8298-f1e9-46f0-828f-717c7d632077
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 43%

---

# Preacquisire il contenuto dell’offerta

La funzione di preacquisizione di [!DNL Target] utilizza gli SDK per dispositivi mobili iOS e Android per recuperare il contenuto delle offerte il minor numero di volte possibile, memorizzando nella cache le risposte dal server.

>[!IMPORTANT]
>
>Supporto per [!DNL Adobe Mobile] versione 4.*x* SDK è terminato il 31 agosto 2021 e non è più consigliato per [!DNL Adobe Target] utenti di dispositivi mobili.
>
>L&#39;SDK [Adobe Experience Platform per le app mobili](https://developer.adobe.com/client-sdks/documentation/){target=_blank} è la soluzione consigliata per alimentare le soluzioni e i servizi [!DNL Adobe Experience Cloud] nelle app mobili.

Questo processo consente di ridurre il tempo di caricamento, evita l’esecuzione di più chiamate di rete e permette di notificare ad [!DNL Target] quale elemento mbox è stato visitato dall’utente dell’app mobile. Tutto il contenuto viene recuperato e memorizzato in cache durante la chiamata di preacquisizione, e da qual momento viene richiamato dalla cache per tutte le chiamate future che includono quel contenuto per il nome di mbox specificato.

Quando utilizzi il metodo di preacquisizione con gli SDK mobili di iOS e Android, considera le seguenti limitazioni:

* Il contenuto di preacquisizione non rimane tra un avvio dell’app e quello successivo. Viene memorizzato nella cache per tutto il tempo in cui l’app rimane attiva oppure fino alla chiamata del metodo `clearPrefetchCache()`.
* La funzionalità di preacquisizione non è supportata per i metodi di allocazione del traffico [!UICONTROL Auto-Allocate] e [!UICONTROL Auto-Target], per i tipi di attività [!UICONTROL Automated Personalization] o [!UICONTROL Recommendations] o per le offerte [recommendations all&#39;interno di un&#39;attività A/B o XT](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-as-an-offer.html?lang=it).

Per ulteriori informazioni, inclusi i metodi di preacquisizione, le classi pubbliche e gli esempi di codice, vedi:

* **iOS:** [Preacquisire il contenuto delle offerte in iOS](https://experienceleague.adobe.com/docs/mobile-services/ios/target-ios/c-mob-target-prefetch-ios.html?lang=it) nella *Guida dell&#39;SDK iOS di Mobile Services*.
* **Android:** [Preacquisire il contenuto delle offerte in Android](https://experienceleague.adobe.com/docs/mobile-services/android/target-android/c-mob-target-prefetch-android.html?lang=it) nella *Guida dell&#39;SDK Android di Mobile Services*.
