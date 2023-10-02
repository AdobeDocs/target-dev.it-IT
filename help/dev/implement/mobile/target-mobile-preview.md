---
keywords: qa, anteprima, collegamento di anteprima, mobile, anteprima mobile
description: Utilizza i collegamenti di anteprima mobile per eseguire un controllo qualità end-to-end per le attività delle app mobili.
title: Come si utilizzano i collegamenti di anteprima mobili in [!DNL Adobe Target] Mobile?
feature: Implement Mobile
exl-id: c0c4237a-de1f-4231-b085-f8f1e96afc13
source-git-commit: 0bcfa16cb79644e7ce10e33daf6c8385104c197f
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 27%

---

# [!DNL Target] anteprima mobile

Utilizza i collegamenti di anteprima per dispositivi mobili per eseguire facilmente un controllo qualità end-to-end per le attività dell’app mobile e iscriverti a diverse esperienze utilizzando il tuo dispositivo senza dover utilizzare dispositivi di test speciali.

La funzionalità di anteprima mobile consente di testare completamente le attività dell’app mobile prima di avviarle in diretta.

## Prerequisiti

1. **Utilizza una versione supportata dell’SDK:** La funzione di anteprima mobile richiede che si scarichi e si installi la versione appropriata del [!DNL Adobe Mobile SDK] nelle app corrispondenti.

   Per istruzioni su come scaricare l’SDK appropriato, consulta [Versioni SDK correnti](https://developer.adobe.com/client-sdks/documentation/current-sdk-versions/){target=_blank} nel *[!DNL Adobe Experience Platform Mobile SDK]* documentazione.

1. **Impostazione di uno schema URL:** il collegamento di anteprima utilizza uno schema URL per aprire l&#39;app. Specifica uno schema URL univoco per l’anteprima.

   Per ulteriori informazioni, consulta [Anteprima visiva](https://developer.adobe.com/client-sdks/documentation/adobe-target/#visual-preview){target=_blank} in *Configurare l’estensione Target nell’interfaccia utente di Data Connection* nel *[!DNL Mobile SDK]* documentazione.

   I seguenti collegamenti contengono ulteriori informazioni:

   * **iOS**: per ulteriori informazioni sull’impostazione degli schemi URL per iOS, consulta [Definizione di uno schema URL personalizzato per l’app](https://developer.apple.com/documentation/xcode/defining-a-custom-url-scheme-for-your-app){target=_blank} il *Sviluppatore Apple* sito Web.
   * **Android**: per ulteriori informazioni sull’impostazione degli schemi URL per Android, consulta [Creare collegamenti profondi al contenuto dell’app](https://developer.android.com/training/app-links/deep-linking){target=_blank} il *Sviluppatori Android* sito Web.

1. **Configurare `collectLaunchInfo` API (solo i0S)**

   Per ulteriori informazioni, consulta [Anteprima visiva](https://developer.adobe.com/client-sdks/documentation/adobe-target/#visual-preview){target=_blank} in *Configurare l’estensione Target nell’interfaccia utente di Data Connection* nel *[!DNL Mobile SDK]* documentazione.

## Generazione di un link di anteprima

1. In [!DNL Target] interfaccia utente, fai clic su **[!UICONTROL Altre opzioni]** (puntini di sospensione verticali), quindi seleziona **[!UICONTROL Crea collegamento di anteprima mobile]**.

   ![immagine alt](assets/mobile-preview-create.png)

1. Seleziona le attività da visualizzare in anteprima, quindi fai clic su **[!UICONTROL Genera collegamento di anteprima mobile]**.

   >[!NOTE]
   >
   >Puoi selezionare solo Basato su modulo [!UICONTROL Test A/B] e [!UICONTROL Targeting esperienza] (XT) attività.

   ![immagine alt](assets/mobile-preview-select-activities.png)

1. Specifica lo schema URL dell&#39;app.

   Lo schema URL deve essere uguale a quello presente nell’app iOS o Android. Se necessario, ripeti questa procedura separatamente per iOS e Android.

   ![immagine alt](assets/mobile-preview-enter-url-scheme.png)

1. Fai clic su **[!UICONTROL Genera collegamento di anteprima mobile]**, quindi copia il collegamento.

   ![immagine alt](assets/mobile-preview-generate-and-copy.png)

## Anteprima sul dispositivo

Apri il link in un browser mobile su un dispositivo in cui hai installato l&#39;app. Questa app può essere l&#39;app di produzione che hai scaricato da [!DNL Apple App Store] o [!DNL Google Play Store]. L’app non deve essere una build speciale. Se disponi di un collegamento di anteprima attivo, puoi visualizzare le esperienze sul dispositivo.

1. Apri il link nel tuo browser mobile.

   Condividi il collegamento copiato nella sezione precedente da [!DNL Target] Interfaccia utente per il dispositivo mobile in modo pratico, ad esempio utilizzando testo, e-mail o [!DNL Slack].

   |![collegamento diretto anteprima 1](assets/mobile-preview-open-deeplink.png)|![collegamento diretto anteprima 2](assets/mobile-preview-open-app.png)|

   L’app apre e avvia il [!DNL Target] [!UICONTROL Modalità anteprima mobile].

1. Seleziona la combinazione di esperienze da visualizzare, quindi fai clic su **[!UICONTROL Avvia esperienze]**.

   |![anteprima mobile 1](assets/mobile-preview-experience-selection-1.png)|![anteprima mobile 2](assets/mobile-preview-experience-result-1-france.png)|![anteprima mobile 3](assets/mobile-preview-experience-result-1-shipfree.png)|
|![anteprima mobile 4](assets/mobile-preview-experience-selection-2.png)|![anteprima mobile 5](assets/mobile-preview-experience-result-2-aus.png)|![anteprima mobile 6](assets/mobile-preview-experience-result-2-10off.png)|

## Limitazioni 

* La visualizzazione deve caricare nuovamente per il nuovo contenuto da visualizzare dopo aver fatto clic sul pulsante **[!UICONTROL Avvia esperienze]**. Il modo più semplice è quello di passare a una schermata diversa e poi tornare alla schermata in cui si prevede che il cambiamento avvenga.
* L&#39;anteprima mobile non è supportata per le versioni Android prima di API-19 (KitKat).
