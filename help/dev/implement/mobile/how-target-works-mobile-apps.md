---
description: Scopri come utilizzare  [!DNL Adobe Mobile SDK]  per mostrare esperienze ottimali ai visitatori della tua app mobile.
title: Come funziona  [!DNL Target]  nelle app mobili?
feature: Implement Mobile
exl-id: 33001f01-fde6-48cb-ac02-d1a632b2150d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '224'
ht-degree: 17%

---

# Funzionamento di [!DNL Target] nelle app per dispositivi mobili

[!DNL Adobe Mobile SDK] contatta il server [!DNL Target] per ottenere il contenuto insieme ad altri punti dati per mostrare l&#39;esperienza giusta all&#39;utente.

>[!IMPORTANT]
>
>Supporto per [!DNL Adobe Mobile] versione 4.*x* SDK è terminato il 31 agosto 2021 e non è più consigliato per [!DNL Adobe Target] utenti di dispositivi mobili.
>
>L&#39;SDK [Adobe Experience Platform per le app mobili](https://developer.adobe.com/client-sdks/documentation/){target=_blank} è la soluzione consigliata per alimentare le soluzioni e i servizi [!DNL Adobe Experience Cloud] nelle app mobili.

## [!DNL Target] posizioni e metriche di successo

Una posizione *di destinazione* è indicata anche come mbox. È possibile abilitare una posizione identificata nell’app a scopo di testing o personalizzazione (ad esempio, per presentare il messaggio di benvenuto nella schermata iniziale). Queste posizioni vengono identificate durante il processo di creazione del test.

Una *[metrica di successo](https://experienceleague.adobe.com/docs/target/using/activities/success-metrics/success-metrics.html?lang=it)* è un&#39;azione eseguita dall&#39;utente che identifica l&#39;esito di una specifica attività (ad esempio la registrazione, un acquisto, la prenotazione di un biglietto e così via).

![Alt immagine](assets/mobile-target-location.png)

* Percorso **[!DNL Target]:** Il contenuto visualizzato sotto il pulsante di registrazione.

  A questo particolare utente viene offerta la spedizione gratuita fino alle 18:00. Questo percorso può essere riutilizzato in più attività [!DNL Target] per eseguire test A/B e personalizzazione.

* **Metrica di successo:** l&#39;azione eseguita dall&#39;utente quando tocca il pulsante di registrazione.

**Comprendere il funzionamento di [!DNL Target] nell&#39;SDK**

![Alt immagine](assets/how-target-mobile-works.png)
