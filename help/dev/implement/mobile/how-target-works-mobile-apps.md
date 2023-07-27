---
description: Scopri come utilizzare il [!DNL Adobe Mobile SDK] per mostrare esperienze ottimali ai visitatori della tua app mobile.
title: In che modo [!DNL Target] Lavorare nelle app mobili?
feature: Implement Mobile
exl-id: 33001f01-fde6-48cb-ac02-d1a632b2150d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 34%

---

# Come [!DNL Target] funziona nelle app mobili

Il [!DNL Adobe Mobile SDK] contatta il [!DNL Target] per ottenere il contenuto insieme ad altri punti dati, in modo da mostrare l’esperienza giusta all’utente.

>[!IMPORTANT]
>
>Supporto per [!DNL Adobe Mobile] versione 4.*x* SDK è terminato il 31 agosto 2021 e non è più consigliato per [!DNL Adobe Target] utenti di dispositivi mobili.
>
>Il [SDK di Adobe Experience Platform per le app mobili](https://developer.adobe.com/client-sdks/documentation/){target=_blank} è la soluzione consigliata per [!DNL Adobe Experience Cloud] soluzioni e servizi nelle app mobili.

## [!DNL Target] posizioni e metriche di successo

Una *posizione di destinazione* viene indicata anche come mbox. È possibile abilitare una posizione identificata nell’app a scopo di testing o personalizzazione (ad esempio, per presentare il messaggio di benvenuto nella schermata iniziale). Queste posizioni vengono identificate durante il processo di creazione del test.

Una *[metrica di successo](https://experienceleague.adobe.com/docs/target/using/activities/success-metrics/success-metrics.html)* è un’azione eseguita dall’utente che identifica l’esito di una specifica attività (ad esempio la registrazione, un acquisto, la prenotazione di un biglietto e così via).

![immagine alt](assets/mobile-target-location.png)

* **[!DNL Target]posizione:** Contenuto visualizzato sotto il pulsante di registrazione.

  A questo particolare utente viene offerta la spedizione gratuita fino alle 18:00. Questa posizione può essere riutilizzata in più [!DNL Target] attività per eseguire test A/B e personalizzazione.

* **Metrica di successo:** Azione eseguita dall&#39;utente quando tocca il pulsante di registrazione.

**Comprendere come [!DNL Target] funziona nell&#39;SDK**

![immagine alt](assets/how-target-mobile-works.png)
