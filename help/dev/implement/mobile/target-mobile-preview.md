---
keywords: qa, anteprima, collegamento di anteprima, mobile, anteprima mobile
description: Utilizza i collegamenti di anteprima mobile per eseguire un controllo qualità end-to-end per le attività delle app mobili. Puoi iscriverti a diverse esperienze senza utilizzare speciali dispositivi di prova.
title: Come si utilizza il collegamento di anteprima mobile in [!DNL Target] Mobile?
feature: Implement Mobile
exl-id: c0c4237a-de1f-4231-b085-f8f1e96afc13
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '617'
ht-degree: 68%

---

# [!DNL Target] anteprima mobile

Il collegamento di anteprima su dispositivi mobili ti permette di verificare il funzionamento delle attività nell’app mobile e di iscriverti a diverse esperienze direttamente dal tuo dispositivo, senza dover utilizzare particolari dispositivi di prova.

>[!NOTE]
>
>La funzionalità di anteprima mobile richiede che si scarichi e si installi la versione appropriata 4.14 (o successiva) dell&#39;SDK Adobe Mobile.

## Panoramica

La funzionalità di anteprima mobile consente di testare completamente le attività dell&#39;app mobile prima di avviarle in diretta.

## Prerequisiti

1. **Utilizzare una versione supportata dell&#39;SDK:** la funzionalità di anteprima mobile richiede che si scarichi e si installi la versione 4.14 (o successiva) appropriata dell&#39;SDK Adobe Mobile nelle applicazioni corrispondenti.

   Per istruzioni su come scaricare l&#39;SDK appropriato, vedi:

   * **iOS:** [Prima di iniziare](https://experienceleague.adobe.com/docs/mobile-services/ios/getting-started-ios/requirements.html) nel *Guida iOS di Mobile Services*.
   * **Android:** [Prima di iniziare](https://experienceleague.adobe.com/docs/mobile-services/android/getting-started-android/requirements.html) nel *Guida Android di Mobile Services*.

1. **Impostazione di uno schema URL:** il collegamento di anteprima utilizza uno schema URL per aprire l&#39;app. È necessario specificare uno schema URL univoco per l&#39;anteprima.

   L&#39;illustrazione seguente è un esempio su iOS:

   ![immagine alt](assets/mobile-preview-url-scheme-ios.png)

   La seguente illustrazione è un esempio su Android:

   ![immagine alt](assets/Android_Deeplink.png)

1. **Traccia Adobe DeepLink**

   **iOS:** nel delegato dell’app, chiama `[ADBMobile trackAdobeDeepLink:url` quando al delegato viene richiesto di aprire la risorsa con lo schema URL specificato nel passaggio precedente.

   Il seguente frammento di codice è un esempio:

   ```javascript {line-numbers="true"}
   - (BOOL) application:(UIApplication *)app openURL:(NSURL *)url 
                options:(NSDictionary<NSString *,id> *)options { 
   
       if ([[url scheme] isEqualToString:@"com.adobe.targetmobile"]) { 
           [ADBMobile trackAdobeDeepLink:url]; 
           return YES; 
       } 
       return NO; 
   } 
   ```

   **Android:** nell’app, chiama `Config.trackAdobeDeepLink(URL);` quando al chiamante viene richiesto di aprire la risorsa con lo schema URL specificato nel passaggio precedente.

   ```javascript {line-numbers="true"}
    private Boolean shouldOpenDeeplinkUrl() { 
        Intent appLinkIntent = getIntent(); 
        String appLinkAction = appLinkIntent.getAction(); 
        Uri appLinkData = appLinkIntent.getData; 
        if (appLinkData.toString().startsWith("com.adobe.targetmobile")) { 
            Config.trackAdobeDeepLink(appLinkData); 
            return true; 
        } 
        return false; 
     }
   ```

   Per il corretto funzionamento dell&#39;anteprima mobile su Android, è necessario aggiungere anche il seguente frammento di codice in AndroidManifest.xml se si utilizza la versione 5 dell&#39;SDK di Adobe Mobile:

   ```javascript {line-numbers="true"}
   <activity android:name="com.adobe.marketing.mobile.FullscreenMessageActivity" />
   ```

   Se utilizzi la versione 4 dell’SDK di Adobe Mobile, utilizza il seguente snippet di codice:

   ```javascript {line-numbers="true"}
   <activity android:name="com.adobe.mobile.MessageFullScreenActivity" />
   ```

## Generazione di un link di anteprima

1. In [!DNL Target] interfaccia utente, fai clic su **[!UICONTROL Altre opzioni]** (tre puntini di sospensione verticali), quindi seleziona **[!UICONTROL Crea anteprima mobile]**.

   ![immagine alt](assets/mobile-preview-create.png)

1. Seleziona le attività da visualizzare in anteprima, quindi fai clic su **[!UICONTROL Genera collegamento di anteprima mobile]**.

   >[!NOTE]
   >
   >Possono essere selezionate solo le attività AB ed XT basate su moduli.

   ![immagine alt](assets/mobile-preview-select-activities.png)

1. Specifica lo schema URL dell&#39;app.

   Deve essere lo stesso presente nell&#39;app iOS o Android. Ripeti questo processo separatamente per iOS e Android, se necessario.

   ![immagine alt](assets/mobile-preview-enter-url-scheme.png)

1. Fai clic su **[!UICONTROL Genera collegamento di anteprima mobile]**, quindi copia il collegamento.

   ![immagine alt](assets/mobile-preview-generate-and-copy.png)

## Anteprima sul dispositivo

Apri il link in un browser mobile su un dispositivo in cui hai installato l&#39;app. Questa applicazione può essere l&#39;applicazione di produzione che hai scaricato dall&#39;Apple App Store o dal Google Play Store. Non deve essere una build speciale. Se si dispone di un collegamento di anteprima attivo, sarà possibile visualizzare le esperienze sul dispositivo.

1. Apri il link nel tuo browser mobile.

   Condividi il collegamento copiato nel passaggio precedente da [!DNL Target] L’interfaccia utente del dispositivo mobile può essere usata in modo comodo, ad esempio tramite testo, e-mail o Slack.

   |![collegamento diretto anteprima 1](assets/mobile-preview-open-deeplink.png)|![collegamento diretto anteprima 2](assets/mobile-preview-open-app.png)|

   L’app apre e avvia il [!DNL Target] Modalità Anteprima mobile.

1. Seleziona la combinazione di esperienze da visualizzare, quindi fai clic su **[!UICONTROL Avvia esperienze]**.

   |![anteprima mobile 1](assets/mobile-preview-experience-selection-1.png)|![anteprima mobile 2](assets/mobile-preview-experience-result-1-france.png)|![anteprima mobile 3](assets/mobile-preview-experience-result-1-shipfree.png)|
|![anteprima mobile 4](assets/mobile-preview-experience-selection-2.png)|![anteprima mobile 5](assets/mobile-preview-experience-result-2-aus.png)|![anteprima mobile 6](assets/mobile-preview-experience-result-2-10off.png)|

## Limitazioni 

* La visualizzazione deve caricare nuovamente per il nuovo contenuto da visualizzare dopo aver fatto clic sul pulsante **[!UICONTROL Avvia esperienze]**. Il modo più semplice è quello di passare a una schermata diversa e poi tornare alla schermata in cui si prevede che il cambiamento avvenga.
* L&#39;anteprima mobile non è supportata per le versioni Android prima di API-19 (KitKat).
